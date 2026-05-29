# Spring Boot — Considered, Set Aside

**Date:** 2026-05-29  
**Status:** Idea — not a decision, no commitments made

Spring Boot (from the spring.io ecosystem) came up as a possible backend framework for the CoSiMo cloud platform. It offers a mature, production-grade stack with strong support for REST, WebSocket, security, and — via Spring AI — first-class LLM integration.

However, for the current phase of CoSiMo it does not feel like the right fit:

- **Java complexity** — the Java/Kotlin ecosystem adds significant overhead in boilerplate, build tooling, and onboarding compared to a lightweight Python stack
- **Slower iteration** — compile/restart cycles slow down the rapid prototyping and exploration work needed right now
- **Team context** — the project is in an early research and architecture phase; a heavyweight enterprise framework is better justified when requirements are stable and a larger team is in place
- **Python ecosystem advantage** — the AI/ML tooling (Whisper, Ollama, LangChain, paho-mqtt) is primarily Python-native, making FastAPI a more natural fit for the current stack idea

Spring Boot remains a valid option for a future production phase if the team grows and Java expertise is available. For now, FastAPI (Python) is the preferred direction for the cloud backend.
