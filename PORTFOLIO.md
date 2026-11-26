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

### LanChat — Peer-to-Peer Messaging and Video Calling App That Works With No Internet

An Android app that lets people nearby message and video call each other with no internet and no server. Any phones on the same Wi-Fi or phone hotspot find each other on their own, even when that network has no internet connection. All chats stay on the phones.

**Tech Stack:** .NET MAUI Blazor Hybrid · C# / .NET 10 · TCP/UDP · WebRTC · SQLite · xUnit · HTML/CSS · Android

**APK download:** [Google Drive](https://drive.google.com/file/d/1S_avpJcFVzQIID7j3Tl5P9oH8i6XPBbX/view?usp=sharing)

#### The problem

- No internet, no contact — every message has to travel to a server and back, so people on the same network with no uplink can't reach each other.
- Finding each other — a local network has no accounts or phone numbers, and phones get new IP addresses whenever they reconnect, so an address can't serve as an identity.

#### The solution

- Find people nearby automatically — every phone announces itself every 2 seconds while LanChat is open, so others appear in the Nearby list within about 2 seconds with no setup, sign-up or pairing. The device code (made from a key pair created on first launch) is the identity and the IP address is only how to reach it, so a reconnecting phone keeps its name, code and chat history. People you have chatted with are kept as Contacts with "Last seen …".
- Messages that don't get lost — messages go straight from phone to phone and are acknowledged (✓ sent, ✓✓ arrived). A message typed while the other person is away waits in an outbox and sends itself when they return; a resent message is never shown twice. Chats, contacts and the outbox are saved on the phone, and a notification opens the chat.
- Video calls without a server — one-to-one audio and video calls start from the chat header, with a ringing screen or a notification with Accept and Decline. Setup travels over the same phone-to-phone link as the chat, with no calling server or relay. Missed, declined and finished calls show as rows in the chat.
- Active, Away or Offline — Active (LanChat open) and Away (app closed, phone on the network and receiving) both receive messages at once, Away with a notification; Offline (out of range, no Wi-Fi, or receiving off) messages wait in the outbox. A quiet phone is shown Offline only after a quick connection check fails.
- Storage that stays in check — at 5,000 messages a notice shows the count and space used (e.g. "6,005 messages, about 1.2 MB"). Free up space deletes messages older than 30 days, 90 days or 1 year, or all of them, showing the count and space freed first; only this phone is affected, queued messages are kept, and the database is compacted. Not now hides the notice until the chat grows by another 5,000 messages.

#### Battery optimization

- Sleep by default — with the app closed, no wake lock is held, other phones' broadcasts are ignored, and the phone announces itself every 30 seconds instead of every 2.
- Wake only for messages — the Wi-Fi chip wakes the phone when a connection addressed to it arrives; LanChat stays awake 3 to 10 seconds to save, notify and acknowledge, then sleeps again.
- The sender does the work — each phone remembers its contacts' last addresses and dials a sleeping phone directly.
- No needless wake-ups — idle connections close after 60 seconds, background checks run only while the app is open, and nothing relies on timers.
- This replaced a first fix that kept the CPU awake the whole time, after testing showed the real failure was on the sending side. The app counts its background activity (aiming for under one minute awake on a quiet night), and Settings links to Android's battery settings.

#### Technical approach

- Plain networking, no infrastructure — UDP broadcasts for discovery and direct TCP connections for chat, each message a length-prefixed JSON frame. Works on any home router or phone hotspot, with mobile data on or off.
- Identity that can't be faked casually — each phone makes an ECDsa P-256 key pair on first launch and keeps the private key in Android's secure storage; the device code is a hash of the public key, and a phone presenting a code that doesn't match its key is dropped.
- Calls with what the phone already has — WebRTC built into the app's WebView, call setup over the existing chat link, no STUN or TURN servers, media encrypted between the two phones.
- Web skills, native app — built in C# on .NET 10 with .NET MAUI Blazor Hybrid; the screens started as XAML pages and moved to Blazor Hybrid early so they could be built with HTML and CSS from a design prototype.
- Testable rules — discovery, messaging, the outbox, calls, background modes and wake holds live in plain C# libraries with no MAUI code; xUnit tests run two real nodes against each other over loopback, and a console app acts as a second "phone" on a PC.
- Built to change safely — every announcement and handshake carries a protocol version (now 6) and the oldest version it still understands, so an older app sees "ask them to update" instead of a peer silently missing.
- Offline storage — chats, contacts and the outbox live in SQLite on the phone, with backups turned off so a key never ends up on two phones.

#### Outcome

- People on the same Wi-Fi or hotspot can message and call each other with no internet, no accounts and no server.
- Messages to someone who stepped away wait and arrive when they're back, never twice.
- A closed app keeps receiving without holding the phone awake.

### RoastIT — Offline Booking and Ordering App for Roasting Businesses

An offline booking and ordering app for businesses that sell roasted products, built first for Litson Haus, a lechon roasting shop. It runs on one shared Android phone at the counter, and all data stays on that phone.

**Tech Stack:** .NET MAUI Blazor Hybrid · C# · SQLite · EF Core · xUnit · HTML/CSS · Android

**APK download:** [Google Drive](https://drive.google.com/file/d/1mOhtC-Zu4Xy4QZyB7II5jD4jU3CdyIrz/view?usp=sharing)

#### The problem

- Manual recording — bookings, weights, prices, payments and balances were written down and totalled by hand, so entries got lost or misread and there was no reliable payment history.
- Missed bookings — nothing told staff that a roast should already have started, so a pending order could sit forgotten until the customer arrived.
- Kitchen load — a list of pickup times doesn't show how many roasts overlap at a given hour, so the owner couldn't tell whether a new booking would fit.

#### The solution

- Automated booking records — every product line gets its own booking ID (`YYMMDD-NNN`); totals are computed from per-kilo menu prices; bookings move through Pending → Cooking → Cooked → Completed; payments, discounts and balances are tracked per order with a saved history; a daily dashboard shows revenue, amount collected, balance due and product mix; data is protected by a PIN lock, an exportable backup and an automatic hourly CSV of nearby bookings.
- Missed-booking alerts — each menu item has a cooking time, so the start time is pickup time − cooking time. A Pending booking past that time gets a red stripe and a "Late to start · was due 3:00 PM (20 min ago)" line, escalating to "Not started · pickup was …" once the pickup time passes. The list updates every minute and whenever the app is reopened.
- Kitchen load chart — for today or any future date, shows how many items are on the roasters at each hour. Cooking bookings count from when they actually started; Pending bookings count from their planned start until pickup, with late ones moved to start "now". Tapping a bar shows details such as "3 cooking · 1:00 PM – 3:00 PM". The owner uses it to check capacity before accepting a booking.

#### Technical approach

- Planned first as a web app (React, ASP.NET Core API, PostgreSQL), then rebuilt as a standalone .NET MAUI Blazor Hybrid app, because the shop runs everything from one shared phone and a server, accounts and live sync would have added cost and failure points without solving a real problem.
- Web skills, native app — Blazor Hybrid draws screens with HTML and CSS inside a native shell; every screen was designed first as an HTML prototype and its CSS carried straight into the app with no CSS framework.
- Offline by design — bookings live in SQLite on the phone via EF Core; schema updates run on the device at startup, after a safety copy of the database is saved.
- Testable rules — booking IDs, balances, the late-start alert and the kitchen load live in a plain C# library with no UI or database code, tested with xUnit against real SQLite.
- Built for the shop — money is stored as whole centavos to avoid rounding errors, and "today" always follows Manila time even if the phone's time zone is wrong.

#### Outcome

- One accurate record replaces handwritten booking notes.
- Forgotten roasts show up the minute they are late, not when the customer arrives.
- New bookings are accepted based on what the kitchen can actually handle at that hour.

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
