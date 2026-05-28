# CoSiMo — System Architecture Planning

This repository is the central workplace for planning and documenting the architecture of the **CoSiMo** system — the inclusive mobility concierge built for the [MonoCab](https://monocab-system.com). It captures how the system is intended to work, records the reasoning behind every architectural decision, and serves as the shared reference for all project partners.

---

## Where to find what

| I want to… | Go to |
|---|---|
| Understand the goals and principles | [docs/vision/](docs/vision/) |
| Get a system-wide picture | [docs/system/overview.md](docs/system/overview.md) |
| See who interacts with the system | [docs/system/actors.md](docs/system/actors.md) |
| Dive into a specific component | [docs/system/monocab/](docs/system/monocab/) · [docs/system/station/](docs/system/station/) · [docs/system/cosimo-cloud/](docs/system/cosimo-cloud/) · [docs/system/app/](docs/system/app/) |
| Understand a past architectural decision | [docs/decisions/](docs/decisions/) |
| Look up a project-specific term | [glossary.md](glossary.md) |
| Understand security concepts | [docs/security/](docs/security/) |
| Contribute or add content | [how-to.md](how-to.md) |
| See what has changed | [changelog.md](changelog.md) |
| See what's planned | [roadmap.md](roadmap.md) |

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
├── docs/
│   ├── vision/            # Goals, principles, and scope
│   ├── system/
│   │   ├── overview.md    # System-wide component map
│   │   ├── actors.md      # Who interacts with the system and in what role
│   │   ├── monocab/       # MonoCab vehicle — architecture + infrastructure
│   │   ├── station/       # Station / platform — architecture + infrastructure
│   │   ├── cosimo-cloud/  # Cloud backend — architecture + infrastructure
│   │   └── app/           # Mobile / kiosk apps — architecture + infrastructure
│   ├── workflows/         # Operational and development workflows
│   ├── security/          # Security concepts and requirements
│   └── decisions/         # Architectural Decision Records (ADRs)
│
├── diagrams/              # Visual models and exports
└── research/              # References, ideas, and competitive analysis
```
