# Actors

This document defines who and what interacts with the CoSiMo system and in what capacity. Actors include both human roles and system entities that communicate, act, and respond within the CoSiMo ecosystem.

---

## Overview

| Actor | Type | Description | Primary touchpoint |
|---|---|---|---|
| Passenger | Human | A person using the MonoCab | Station, cabin, app |
| Operator | Human | Staff managing and monitoring the CoSiMo system | Web app, admin interface |
| CoSiMo | System | The AI concierge — mediates between passenger and system | All touchpoints |
| MonoCab | System | An individual vehicle instance | Cabin hardware, CoSiMo Cloud |
| Station | System | An individual stop/boarding point | Passenger, MonoCab, CoSiMo Cloud |

---

## Passenger

The passenger is the primary user of CoSiMo. They interact with the system before, during, and after a journey. There is no assumed default passenger — CoSiMo adapts to whoever is using it.

**Touchpoints:** App (booking, preferences), station (authentication, boarding), cabin (concierge interaction throughout the ride).

**Goals:**
- Complete a journey safely and comfortably
- Receive relevant information and assistance without having to ask for it
- Feel in control, oriented, and supported at every stage

**Constraints and considerations:**
- Passengers include people with physical, sensory, or cognitive impairments
- Passengers include elderly people, children travelling alone, and people unfamiliar with the system
- A passenger may have limited digital literacy or require non-standard interaction modes
- Authentication and consent flows must be accessible to all passenger types
- All personal data collected during a session is deleted at the end of the ride

---

## Operator

The operator manages the CoSiMo system on behalf of the service provider. They do not interact with passengers directly through CoSiMo but monitor system health and configure the platform.

**Touchpoints:** Web app or admin interface.

**Goals:**
- Monitor the status of active rides and vehicles
- Configure and maintain the system
- Respond to incidents or escalations

**Constraints and considerations:**
- Operators have elevated access and must be authenticated with strong credentials
- Operator actions affecting live rides must be logged and auditable
- Operators do not have access to personal passenger data beyond what is operationally necessary

---

## CoSiMo

CoSiMo is the AI concierge. It is the mediating actor between the passenger and every other part of the system — translating user intent into actions, surfacing information, and controlling cabin functions within defined boundaries. It acts across all touchpoints and adapts its behaviour to the passenger's persona.

**Communicates with:** Passenger (all touchpoints), MonoCab (cabin hardware), Station (boarding state), CoSiMo Cloud (AI inference, session management).

**Responsibilities:**
- Guide the passenger through the full journey
- Translate passenger input into system actions
- Adapt interaction modality, language, and pace to the passenger's profile
- Respect the system boundary — escalate or decline requests outside its permitted scope

**Constraints:**
- May not directly access safety-critical vehicle functions without going through the authenticated control layer
- Must degrade gracefully if cloud connectivity is lost
- Has no persistent memory of a passenger beyond the current session

---

## MonoCab

Each MonoCab is an individual vehicle instance and a system actor in its own right. It exposes cabin hardware (sensors, actuators) to CoSiMo and reports its state to CoSiMo Cloud. Multiple MonoCabs may operate independently and simultaneously.

**Communicates with:** CoSiMo (in-cabin interaction), CoSiMo Cloud (state reporting, remote management), Station (arrival/departure events).

**Responsibilities:**
- Execute cabin actions requested by CoSiMo within permitted boundaries
- Report operational state (location, door state, occupancy) to CoSiMo Cloud
- Maintain safe operation regardless of concierge availability

**Constraints:**
- Safety-critical functions are not directly accessible to the concierge layer
- Must function in a degraded but safe state if cloud or concierge connectivity is lost

---

## Station

Each station is an individual physical stop and a system actor. It handles passenger authentication before boarding, communicates boarding and departure events, and connects the passenger to the MonoCab and CoSiMo platform.

**Communicates with:** Passenger (authentication, information), MonoCab (arrival/departure coordination), CoSiMo Cloud (session initiation, state sync).

**Responsibilities:**
- Authenticate the passenger before the MonoCab unlocks
- Signal boarding and departure events to the system
- Provide passenger-facing information relevant to the upcoming journey

**Constraints:**
- Must handle passengers who cannot use standard authentication methods (accessibility requirement)
- Must function at a basic level even if cloud connectivity is degraded
