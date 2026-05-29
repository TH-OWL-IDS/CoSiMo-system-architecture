# Initial System Idea — CoSiMo Architecture

**Date:** 2026-05-20  
**Status:** Idea — not a decision, no commitments made  
**Author:** Johannes Homann

> This document captures an early sketch of the overall CoSiMo system topology and a first thinking on possible tech stacks per zone. Nothing here is decided. It is a starting point for discussion.

---

## Overview

The system is divided into four zones that communicate with each other:

```
Personal Interfaces (App / Webapp / Gadgets)
        ↕  HTTPS / WebSocket
CoSiMo Cloud Platform
        ↕  MQTT (cloud acts as client, connects to edge brokers)
MonoCabs       Stations
        ↕  MQTT (local broker per node)
Physical Devices (sensors, actuators)
```

---

## 1. CoSiMo Cloud

The central backend. Receives input from personal interfaces, runs the AI pipeline, and issues commands to MonoCabs and stations. The cloud does **not** run an MQTT broker — it runs an MQTT **client** that connects outward to the brokers on each edge node.

### Proposed Services (one container per service)

| Service | Idea | Notes |
|---|---|---|
| Reverse Proxy | Nginx or Traefik | HTTPS termination, routes traffic to internal services |
| Auth | Custom service | Issues and validates scoped, time-limited JWT tokens per session |
| API | FastAPI (Python) | REST + WebSocket; central coordination point for all clients |
| LLM | Ollama (Mistral / LLaMA 3) | Runs on DGX GPU; EU-sovereign, on-premise |
| STT (Speech-to-Text) | OpenAI Whisper (server-side) | Transcribes audio streamed from user devices |
| TTS (Text-to-Speech) | Coqui TTS or edge-tts | Synthesises audio responses; streamed back to user device or cab speaker |
| MCP Layer | Custom service | Translates LLM tool calls into MQTT commands; enforces scope rules — LLM never touches MQTT directly |
| MQTT Client | paho-mqtt service | Connects to all edge brokers (cabs, stations); publishes commands, receives status |
| Scheduler | Custom service | Aggregates trip and ride status from MQTT; feeds departure boards and app |
| Database | PostgreSQL | Sessions, node registry, device capabilities, NFC mappings — no personal data |
| App Server | Nginx (static files) | Serves the compiled frontend app |

### Infrastructure idea

- Hosted on TH OWL DGX server
- All services containerised via Docker Compose, managed via Portainer
- No personal data stored — sessions are anonymous, token-scoped, time-limited

### Open questions

- Which Git hosting? GitHub (public) or internal Gitea?
- Which LLM model? Mistral 7B, LLaMA 3, or other?
- Frontend framework: React, plain HTML/JS, or other?
- Server hosting fallback if DGX is unavailable?
- KI.NRW resources available?

---

## 2. MonoCab

Each MonoCab is a self-contained edge node. It runs its own local MQTT broker. Physical devices connect to the broker via a **Device Bridge** — a small Python service that translates between hardware (GPIO, serial, I2C) and MQTT. The cloud connects to the cab's broker as a remote MQTT client.

### Proposed Components

| Component | Idea | Notes |
|---|---|---|
| Edge Computer | Raspberry Pi 5 | Runs broker and Device Bridge |
| MQTT Broker | EMQX (containerised) | Local message bus for all cabin devices |
| Device Bridge | Python service (paho-mqtt) | Translates hardware signals ↔ MQTT topics |
| Network | Onboard WiFi + USB LTE dongle | WiFi at station, LTE fallback in motion |
| Offline STT (fallback) | whisper.cpp (tiny model, CPU) | Local transcription when cloud is unreachable |
| Offline Agent (fallback) | Simple intent matcher (Python) | Pre-scripted keyword responses for basic commands when offline |

### Physical subsystems

- Air conditioning / climate
- Light system (cabin lighting, signal tones)
- Sound system — Speaker 1 (TTS output), Speaker 2 (signal tones)
- Screen system — Screen 1, Screen 2
- General status — Seat sensor, Door sensor, GPS sensor

### Open questions

- Are microphones permitted inside the cabin? (privacy / legal)
- Where exactly does local inference happen when offline — on the Pi or a separate cabin computer?

---

## 3. Stations

Same structure as MonoCab edge nodes, but stationary. Always on stable wired network — no offline fallback needed.

### Proposed Components

| Component | Idea | Notes |
|---|---|---|
| Edge Computer | Raspberry Pi 5 | Runs broker and Device Bridge |
| MQTT Broker | EMQX (containerised) | Local message bus for all station devices |
| Device Bridge | Python service (paho-mqtt) | Same pattern as MonoCab |
| Network | Ethernet (preferred) | Stable, no LTE fallback needed |

### Physical subsystems

- Light system
- Sound system — Speaker 1 (TTS / arrival announcements), Speaker 2 (signal tones)
- Screen system — arrival / departure board
- General status — Presence sensor (for MonoCab detection), Door sensor

### Open questions

- Which stations are in scope for the prototype? (Detmold and Höxter visible in early diagrams)
- What hardware is already present at stations vs. what needs to be installed?

---

## 4. App / Webapp / Gadgets

Personal user interfaces. All are thin clients — no inference runs on the device. They connect to the CoSiMo Cloud via HTTPS and WebSocket only. They never connect to MQTT brokers directly.

### App (Smartphone)

- Idea: React web app or native app
- Handles audio I/O — microphone streams audio to cloud STT; receives TTS audio back
- Displays trip status, concierge responses, ride progress
- Holds session JWT in memory (not persisted to storage)

### Webapp

- Same codebase as the app, different URL mode parameter
- Also used for cabin display (`?mode=cabin`) and station display (`?mode=station`)

### Gadgets

**NFC Card / Chip**
- No app, no internet on the card itself
- Card UID scanned by a reader at station or cab
- Reader (connected to edge Pi) publishes scan event via MQTT → cloud issues session token
- Most anonymous scenario — no personal device required

**Watch (e.g. Apple Watch)**
- Either as companion to phone (relays audio/input)
- Or standalone thin client — streams audio to cloud, receives haptic + display feedback
- Same JWT-based auth, different input/output surface

### Open questions

- Native app or progressive web app (PWA)?
- Watch: companion only, or standalone?
- Minimum viable interface for prototype — app only, or also gadgets?

---

## Communication Summary

| From | To | Protocol | Notes |
|---|---|---|---|
| App / Webapp / Gadgets | CoSiMo Cloud | HTTPS / WebSocket | Audio stream, status updates, trip requests |
| CoSiMo Cloud MQTT Client | MonoCab Broker | MQTT | Publishes commands, subscribes to status |
| CoSiMo Cloud MQTT Client | Station Broker | MQTT | Publishes commands, subscribes to status |
| Device Bridge | MonoCab Broker | MQTT (local) | Hardware ↔ broker translation on Pi |
| Device Bridge | Station Broker | MQTT (local) | Hardware ↔ broker translation on Pi |
| MonoCab Broker ↔ Station Broker | — | No direct connection | All cross-zone coordination goes via cloud |