# James Abad Gabriel — Web Developer

## About Me

I'm a web developer with 4 years of experience turning ideas into functional, user-friendly applications. What excites me most about development is going beyond the browser — integrating web apps with desktop and mobile platforms to create experiences that feel connected and seamless across devices.

I have a genuine passion for problem-solving. Whether it's tracking down a stubborn bug, untangling a messy workflow, or finding a smarter way to structure a process, I enjoy the challenge of making things work better. For me, development isn't just about writing code — it's about building solutions that are efficient, reliable, and built to last.

---

## Experience

### Web Developer — [Eversun Software Philippines Corporation](https://www.agentsofvalue.com/)
**April 2023 – Current**

- Developed and maintained responsive website designs, improving user experience across various devices.
- Optimized system performance by up to 80%, restructuring application flow, implementing caching, rewriting inefficient SQL queries, and redesigning database/table indexes based on actual usage patterns.
- Migrated Laravel queue worker to a dedicated server, resolving recurring server overload issues and improving overall system stability.
- Refactored backend codebase for a cleaner, more maintainable architecture, introducing test scripts to support quality assurance.
- Refactored frontend (React) to use context providers, streamlining data flow and improving code structure.
- Integrated two separate systems via API-based data fetching and scheduled cron jobs, enabling real-time data synchronization across platforms.
- Developed and maintained a Python-based desktop application, implementing UI updates and resolving bugs.
- Configured and managed server-side infrastructure to support application performance and reliability.
- Documented development processes, creating comprehensive guides for future maintenance and updates.

### Junior Programmer — [Lee Systems Technology Ventures](https://lstventures.com/)
**September 2022 – March 2023**

Supported development of an accounting and payroll system across web (PHP) and desktop (C# Windows Forms) platforms, resolving bugs and implementing new features under senior developer guidance.

---

## Projects

### Full-Stack SPA + Native Desktop Client — Auth, Real-Time Messaging & Video Calling

A multi-client messaging platform: a Laravel/React web SPA and a native Python desktop app, sharing one backend, one set of accounts, and one real-time infrastructure — a browser user and a desktop user can message and video-call each other interchangeably.

**Tech Stack:** Laravel 13 (API-only) · PostgreSQL · React 19 + TypeScript · Tailwind CSS v4 · Laravel Reverb (WebSockets) · Sanctum · Python 3.14 · PySide6/Qt · aiortc (WebRTC) · qasync

**Live demo:** [mygab-app.duckdns.org](https://mygab-app.duckdns.org/)
**Desktop build:** [Download (Google Drive)](https://drive.google.com/file/d/1Od4Y7U45SIpR_T6ItsM_jH-0R1C5YBFQ/view?usp=sharing)
**Web app repo:** [github.com/jamesgabriel-github/RoleBasedAuth](https://github.com/jamesgabriel-github/RoleBasedAuth)
**Desktop app repo:** [github.com/jamesgabriel-github/GabApp-Desktop](https://github.com/jamesgabriel-github/GabApp-Desktop)

#### Web SPA

- Dual-mode session auth — Sanctum stateful cookies for the browser, with the same login endpoint branching to issue a Bearer token for non-browser clients (detected via Sanctum's own frontend-origin check), so one auth endpoint serves two fundamentally different client types without duplicating logic.
- Layered RBAC — enum-based roles (`client`/`admin`/`super admin`) combined with `spatie/laravel-permission`, enforced via middleware/policies on the backend and mirrored in the React UI.
- OAuth (Google/Facebook/GitHub via Socialite) unified into the same session flow.
- Real-time messaging over Reverb/Echo with per-participant read receipts and policy-enforced conversation privacy.

#### Native Desktop Client (GabApp)

- Cross-client interoperability by contract replication, not reinvention — rather than build a separate backend surface, the desktop app replicates the web app's exact REST, WebSocket (Pusher protocol), and WebRTC signaling contracts, verified end-to-end against the real backend at each build phase.
- Custom Pusher-protocol client written from scratch over raw `websockets`, because the standard Python Pusher client (`pysher`) can only listen — it can't send the `client-*` "whisper" events that WebRTC call signaling requires.
- 1:1 video/voice calling via `aiortc`, mirroring the browser's WebRTC peer connection logic exactly — including matching an ICE-candidate queuing behavior needed because `aiortc` doesn't trickle ICE the way the browser does.
- One asyncio event loop for everything (`qasync`) — GUI, WebSocket client, and WebRTC media all share a single loop instead of juggling threads.
- Zero-config hardware handling — camera/mic are auto-detected at runtime (including disambiguating Windows' merged microphone/speaker PnP class), so a packaged build runs unmodified on machines with unknown hardware.
- Frictionless sign-up handoff — clicking "Sign up" opens the web app's registration page in the system browser, then auto-logs the desktop app in via a one-shot local loopback callback server — no native registration form to keep in sync with the web one.
- Packaged as a standalone Windows build via PyInstaller (`dist/GabApp/`) — no Python install required on target machines.

---

## Skills

**Backend:** Laravel · Python · C#
**Frontend:** React · Tailwind CSS · TypeScript
**Database:** SQLite · MySQL · PostgreSQL

---

## Contact

📧 Gmail: [jamesgabrielbsit@gmail.com](mailto:jamesgabrielbsit@gmail.com)
💼 LinkedIn: [linkedin.com/in/jamesgabriel1234](https://www.linkedin.com/in/jamesgabriel1234?utm_source=share_via&utm_content=profile&utm_medium=member_android)
📘 Facebook: [facebook.com/share/19JuE7qVFT](https://www.facebook.com/share/19JuE7qVFT/)
