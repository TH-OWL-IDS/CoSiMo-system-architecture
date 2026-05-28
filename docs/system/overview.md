# System Overview

This document describes the components that make up the CoSiMo system and how they relate to each other. For the goals and principles behind the system, see [vision.md](../vision/vision.md).

---

## The MonoCab

The MonoCab is a fully autonomous, single-cab vehicle that runs on existing but unused rural railroad infrastructure. It requires no driver and operates on demand — there is no fixed timetable. Passengers book a ride through the app or arrive at a station and board when a cab is available.

A physical prototype and cabin demonstrators exist and are available for testing.

### The Cabin

The cabin is the passenger space inside the MonoCab. It is the primary environment in which CoSiMo operates. The cabin contains hardware components — including controls for seating, doors, lighting, audio, and climate — that CoSiMo can interact with to support and guide the passenger.

The exact hardware configuration and which components are addressable by the concierge are defined as part of AP4 and AP5. See [monocab/architecture.md](monocab/architecture.md) for details as they are established.

---

## The Station

The station is the physical stop where passengers board and alight. It is the entry point to the CoSiMo journey. At the station, the passenger authenticates before the cab unlocks and the journey begins.

The station is also a node in the broader IoT network, with sensors and connectivity infrastructure that feed into the CoSiMo system. See [station/architecture.md](station/architecture.md).

---

## CoSiMo — The Concierge

CoSiMo is the AI-driven concierge that runs across the journey: in the app before boarding, at the station during check-in, and in the cabin throughout the ride. Its name is CoSiMo. It does not have a fixed character or visual persona — it fully adapts to the user's individual profile, communication style, and needs.

CoSiMo bridges the passenger and the system. It handles interaction, guidance, and cabin control within defined boundaries. It does not act as a gatekeeper — it guides, supports, and responds.

---

## CoSiMo Cloud

The CoSiMo Cloud hosts the backend services that power the concierge. This includes AI inference (primarily cloud-based, using DSGVO-compliant EU providers), user and session management, and the communication backbone connecting the cabin, station, and app. Only EU-based, DSGVO-compliant providers may be used.

See [cosimo-cloud/architecture.md](cosimo-cloud/architecture.md).

---

## Touchpoints

The app is one touchpoint — but not the only one. Conceptually, CoSiMo is a carry-on AI: the same concierge that accompanies a user through a journey should be reachable through whatever interface is most natural for that user and that context. The app (mobile and web) is the primary digital touchpoint for booking, preferences, and journey tracking. Physical touchpoints — dedicated devices, wearables, or embedded interfaces at stations and in vehicles — are part of the longer-term vision and are being explored as part of the design process.

The architecture is built so that adding a new touchpoint means connecting to the same CoSiMo platform, not rebuilding the concierge.

See [app/architecture.md](app/architecture.md).

---

## How the Components Connect

```
[ Touchpoints: App / Physical devices / ... ]
   │  (booking, profile, preferences)
   ▼
[ CoSiMo Cloud ]
   │  (AI inference, session management, communication backbone)
   ├──────────────────────┐
   ▼                      ▼
[ Station ]          [ MonoCab Cabin ]
  (auth,               (concierge interaction,
   boarding)            cabin hardware control)
```

A journey follows this sequence:

1. Passenger books via app (or arrives at station without booking)
2. Passenger authenticates at the station
3. Cab unlocks and the concierge takes over in-cabin
4. CoSiMo guides the passenger through the ride
5. Passenger alights; session data is discarded
