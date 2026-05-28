# Security Overview

This document describes the security and privacy framework for the CoSiMo system. It is intended as a working reference for the development and research team (TH OWL, HSRW, XignSys). It covers the data categories processed, the principles the system is built on, the division of security responsibilities across partners, threat model, and open questions that require decisions before implementation.

---

## Core Principles

CoSiMo is designed around three foundational commitments that shape every security and privacy decision:

**Privacy-by-Design** — Data protection is built into the system from the start, not added later. The default behaviour of the system is to collect as little data as possible, process it locally where feasible, and discard it as soon as it is no longer needed for the current interaction.

**Security-by-Design** — Security controls are part of the architecture, not a layer applied on top. Authentication, access control, and encrypted communication are structural requirements, not optional features.

**DSGVO Compliance** — All personal data processing must comply with the General Data Protection Regulation (GDPR / DSGVO). This applies to data collected, transmitted, and processed by all project partners. Since AI inference is primarily cloud-based, only DSGVO-compliant, EU-based cloud providers may be used.

---

## Personal Data Processed

The CoSiMo concierge system processes the following categories of personal data during a ride:

| Category | Description | Examples |
|---|---|---|
| Voice / speech input | Audio captured for interaction with the concierge | Commands, questions, ambient speech |
| User identity / authentication data | Information used to identify or authenticate the user | Booking reference, accessibility profile |
| Sensor / behavioural data | Physical state of the cabin and inferred user behaviour | Seat position, door state, movement patterns |
| Video / camera data | Visual input used for interaction or safety functions | Gesture recognition, cabin occupancy |

### Data Minimisation and Retention

All interaction data is **session-scoped**. It is deleted at the end of each ride and not persisted to long-term storage. No personal data from one session is carried into a subsequent session.

For research and evaluation purposes (through milestone M10), anonymised or aggregated data may be retained under a separate, explicitly defined research data protocol. That protocol has not yet been defined and is listed as an open question below.

---

## System Boundary: Concierge vs. Safety-Critical Functions

The concierge interacts with cabin systems (lighting, audio, climate, seat adjustment, door controls) to support passengers. Not all of these interactions carry the same risk level. The following principles define the boundary:

**Within the concierge's operational scope:**
Comfort-related functions such as lighting adjustment, audio playback, climate preferences, and information requests. The concierge may initiate or suggest these without heightened authorisation requirements.

**Restricted — requires explicit authorisation and safety controls:**
Functions that affect passenger safety or vehicle operation: door locking/unlocking, emergency signalling, overrides of vehicle systems. These functions may only be triggered through authenticated, audited pathways. The concierge layer must not have direct, unmediated access to these functions.

**Out of scope for the concierge:**
Vehicle propulsion, braking, and any function classified as safety-critical by the MonoCab system specification. The concierge must fail gracefully if it cannot reach these systems, not attempt to compensate.

---

## Authentication and Access Control

Authentication design is XignSys's responsibility. The following requirements must be reflected in the authentication architecture:

- Users interacting with the concierge must be identifiable at a level appropriate to the action they are requesting.
- Comfort-level interactions may use lightweight or implicit authentication (e.g. session token from booking).
- Safety-relevant interactions require explicit, strong authentication.
- Authentication mechanisms must be accessible to vulnerable user groups, including elderly users and users with disabilities. (See open questions.)
- All access events involving safety-critical functions must be logged and auditable.

---

## Cloud Inference and EU Data Sovereignty

AI inference in CoSiMo is primarily cloud-based. The following constraints apply:

- Only cloud providers that are **EU-based and DSGVO-compliant** may be used for any processing of personal data.
- Voice, video, and behavioural data transmitted to cloud services must be encrypted in transit and at rest.
- The system must not transmit raw personal data to providers outside the EU or to providers that cannot demonstrate DSGVO compliance.
- Where technically feasible, data should be anonymised or pseudonymised before transmission to cloud services.

The specific cloud provider(s) have not yet been selected. Provider selection is an open decision and must include a data protection impact assessment (DPIA).

---

## Decentralisation and Resilience

CoSiMo aims to minimise dependency on any single central service, including the cloud AI backend. This applies end-to-end:

- The IoT/sensor layer (cabin hardware communication via Matter or MQTT) must function independently of cloud connectivity.
- The concierge system must continue to operate in a degraded but safe mode if cloud services are unavailable.
- No single point of failure should render the MonoCab cabin inaccessible or unsafe for passengers.

### Graceful Degradation

If the AI backend loses connectivity during a ride, the system must degrade gracefully:

- The concierge should communicate to the user that AI-assisted interaction is unavailable.
- Basic cabin functions (lighting, audio, climate) should remain accessible through a fallback interface or default state.
- Safety-critical systems must remain fully operational regardless of concierge availability.
- No personal data in transit should be lost or exposed due to a connectivity failure.

The specific fallback behaviour is not yet defined and is listed as an open question below.

---

## Threat Model (High Level)

The following is a lightweight threat model intended to guide design decisions. It is not exhaustive and should be refined as the architecture matures.

### Attack Surfaces

| Surface | Description |
|---|---|
| Network layer (MQTT, Matter) | Communication between cabin sensors/actuators and the concierge system |
| Cloud API endpoints | Interface between the on-site system and cloud-based AI inference |
| Voice input channel | Natural language input from users to the concierge |
| Physical cabin hardware | Direct physical access to cabin components during testing or deployment |
| User authentication flow | The process of identifying and authorising a user |

### Attacker Types

**Malicious rider** — A passenger who attempts to manipulate the concierge through voice commands, exploit unprotected cabin functions, or access another passenger's data. Mitigation: session isolation, authenticated command pathways for safety-relevant functions, input validation.

**External network attacker** — An adversary intercepting or injecting messages on the MQTT/Matter network. Mitigation: encrypted channels, certificate-based device authentication, network segmentation.

**Prompt injection / AI manipulation** — An attacker who crafts voice input or data to cause the AI agent to behave in unintended ways (e.g. bypassing safety boundaries, disclosing system information). Mitigation: input sanitisation, system prompt hardening, output filtering, XignSys-controlled action layer that the AI cannot bypass.

**Compromised cloud service** — A scenario where the cloud AI provider is breached or subpoenaed. Mitigation: data minimisation before transmission, EU-only providers, DPIA, contractual data processing agreements.

**Insider / misconfigured component** — A component introduced during development that inadvertently exposes a security gap. Mitigation: Security-by-Design review at each milestone, access control enforced at the architecture level.

---

## Vulnerable User Groups

CoSiMo is explicitly designed for inclusive use, including elderly users and users with disabilities. Security and privacy mechanisms — particularly authentication flows and consent processes — must not create barriers for these groups. This is acknowledged as a design constraint that must be addressed in coordination between XignSys (authentication design) and TH OWL (UX/accessibility). A dedicated approach is to be defined during AP3/AP4.

---

## Open Questions and Decisions Required

The following security-relevant decisions have not yet been made and must be resolved before the corresponding system components are built:

| # | Question | Relevant milestone |
|---|---|---|
| 1 | Which cloud provider(s) will be used for AI inference? A DPIA must be conducted before selection is finalised. | Before M5 |
| 2 | What is the research data protocol for anonymised data retained between M3 and M10? | Before M3 |
| 3 | What is the specific fallback behaviour of the concierge when cloud connectivity is lost? | Before M6 |
| 4 | How will authentication be made accessible to users with cognitive or physical impairments? | AP3/AP4 |
| 5 | What logging and audit requirements apply to safety-critical actions, and who owns that log? | Before M7 |
