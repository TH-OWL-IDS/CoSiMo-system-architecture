# CoSiMo — Concierge System für inklusive Mobilität

**CoSiMo** is the inclusive mobility concierge system built for the [MonoCab](https://monocab-system.com) — guiding passengers through the full travel experience in an accessible, privacy-respecting way. The broader goal is a transferable, open standard for inclusive digital mobility that can reach beyond the MonoCab.

This system architecture defines how that experience is built, coordinated, and kept trustworthy.

---

## About the Project

CoSiMo is part of the **NEXT.IN.NRW** innovation program. See the [vision](docs/vision/vision.md) for goals, principles, and scope.

---

## Partners

| Institution | Role |
|---|---|
| [TH OWL](https://www.th-owl.de) | Lead research & development |
| [Hochschule Rhein-Waal](https://www.hochschule-rhein-waal.de) | Research partner |
| [XignSys GmbH](https://xignsys.com) | Industry partner |

---

## About this Repository

This repository is the **central guide and workplace for the CoSiMo system architecture**. It documents how the system is intended to work, captures the reasoning behind every architectural decision, and serves as the shared reference for all project partners.

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
│   ├── system/
│   │   ├── monocab/       # Architecture and infrastructure of the MonoCab vehicle
│   │   ├── station/       # Architecture and infrastructure of the MonoCab station
│   │   ├── cosimo-cloud/  # Architecture and infrastructure of the CoSiMo cloud backend
│   │   └── app/           # Architecture and infrastructure of the CoSiMo apps
│   ├── workflows/         # Operational and development workflows
│   ├── security/          # Security concepts and requirements
│   └── decisions/         # Architectural Decision Records (ADRs)
├── diagrams/              # Visual models and exports
└── research/              # References, ideas, and competitive analysis
```

---

## How to Use this Repository

Start with [docs/vision/](docs/vision/) to understand the goals and principles before diving into architecture or decisions. Architectural choices are recorded as **Architectural Decision Records (ADRs)** in [docs/decisions/](docs/decisions/), each capturing the context, options considered, and the reasoning behind the chosen direction.

For a full guide on how to navigate, add, change, or remove content — including the ADR template and file conventions — see [how-to.md](how-to.md).

A running log of all changes to this repository is kept in [changelog.md](changelog.md).
