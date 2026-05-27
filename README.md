# CoSiMo — Concierge System für inklusive Mobilität

CoSiMo is an inclusive mobility concierge system designed to help people safely and accessibly interact with connected mobility environments. Its primary use case is the MonoCab — an autonomous, single-rail vehicle — and the broader goal is to establish a transferable, open standard for inclusive digital mobility.

---

## About the Project

CoSiMo aims to provide a multimodal assistant for real-world mobility scenarios: user guidance, device communication, and inclusive interaction across diverse user needs. The project prioritizes accessibility, data privacy (DSGVO-compliant), and interoperability over closed vendor ecosystems.

The project is part of the **NEXT.IN.NRW** innovation program and is actively developed by three partners.

---

## Partners

| Institution | Role |
|---|---|
| [TH OWL](https://www.th-owl.de) | Lead research & development |
| [Hochschule Rhein-Waal](https://www.hochschule-rhein-waal.de) | Research partner |
| [xignsys GmbH](https://xignsys.com) | Industry partner |

---

## About this Repository

This repository is the **central guide and workplace for the CoSiMo system architecture**. It documents how the system is intended to work, captures the reasoning behind architectural decisions, and serves as a shared reference for all project partners.

It is not a code repository. No production code lives here.

---

## Repository Structure

```
cosimo/
│
├── README.md              # This file
├── how-to.md              # Guide for navigating and contributing to this repo
├── changelog.md           # Log of all changes made to this repository
├── roadmap.md             # Project milestones and open work
├── glossary.md            # Shared terminology across the project
│
├── docs/                  # All written documentation
│   ├── vision/            # Goals, principles, and scope
│   ├── architecture/      # System design and component descriptions
│   ├── governance/        # Roles, responsibilities, and processes
│   ├── workflows/         # Operational and development workflows
│   ├── participation/     # Contribution guidelines and access
│   ├── security/          # Security concepts and requirements
│   └── decisions/         # Architectural Decision Records (ADRs)
│
├── system/                # Structured system definitions
├── infrastructure/        # Hosting, deployment, and infrastructure specs
├── diagrams/              # Visual models and exports
└── research/              # References, Ideas
```

---

## How to Use this Repository

Start with [docs/vision/](docs/vision/) to understand the goals and principles before diving into architecture or decisions. Architectural choices are recorded as **Architectural Decision Records (ADRs)** in [docs/decisions/](docs/decisions/), each capturing the context, options considered, and the reasoning behind the chosen direction. To propose a change or raise a question, open an issue or a pull request against the relevant document.

For a full guide on how to navigate, add, change, or remove content — including the ADR template and file conventions — see [how-to.md](how-to.md).

A running log of all changes to this repository is kept in [changelog.md](changelog.md).
