# Glossary

Shared terminology used across the CoSiMo system architecture. Alphabetically sorted. Add new terms here before using them in documents.

---

**ADR (Architectural Decision Record)**
A short document capturing the context, options considered, and reasoning behind a significant architectural choice. ADRs live in `docs/decisions/` and follow the naming pattern `NNN-short-title.md`. See [how-to.md](how-to.md) for the template.

**DSGVO (Datenschutz-Grundverordnung)**
The German name for the EU General Data Protection Regulation (GDPR). Governs how personal data may be collected, processed, stored, and deleted. All CoSiMo data processing must comply with DSGVO.

**Foundation Model (FM)**
A large-scale AI model trained on broad data and used as a base for downstream tasks. In CoSiMo, Foundation Models — in particular Large Language Models (LLMs) — form the core of the AI agent. Only DSGVO-compliant, EU-based providers may be used.

**Graceful Degradation**
The ability of a system to continue operating in a reduced but safe and usable state when one or more components are unavailable. In CoSiMo, the concierge must degrade gracefully if cloud connectivity is lost, ensuring cabin safety and basic usability are preserved.

**Open Standard**
A publicly available specification that allows different systems, vendors, or platforms to interoperate without proprietary constraints. CoSiMo is built on open standards to ensure long-term transferability and independence from closed ecosystems.

**Privacy-by-Design**
An approach where data protection is built into the system from the outset rather than added retrospectively. CoSiMo applies Privacy-by-Design by defaulting to minimal data collection, session-scoped retention, and encrypted transmission.

**Security-by-Design**
An approach where security controls — authentication, access control, encrypted communication — are structural requirements of the architecture, not optional additions. In CoSiMo, Security-by-Design is the responsibility of XignSys GmbH at the system architecture level.
