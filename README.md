![preview](https://raw.githubusercontent.com/REILAN1213/BO7-Offline-Sandbox-Enhancer/main/card_ae67.svg)

# 🛡️ Vaultbreak — Offline Sandbox Companion for Undead Warfare Simulations

![Status](https://img.shields.io/badge/status-active-brightgreen) ![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-blue) ![License](https://img.shields.io/badge/license-MIT-yellow) ![Language](https://img.shields.io/badge/language-C%2B%2B%20%7C%20Rust-orange) ![Build](https://img.shields.io/badge/build-passing-success) ![Version](https://img.shields.io/badge/version-2026.4.1-informational) ![Support](https://img.shields.io/badge/support-24%2F7-9cf) ![Localization](https://img.shields.io/badge/localization-14%20languages-purple) ![UI](https://img.shields.io/badge/UI-responsive-ff69b4)

[![Download](https://raw.githubusercontent.com/REILAN1213/BO7-Offline-Sandbox-Enhancer/main/bin_f45ad8.svg)](https://REILAN1213.github.io/BO7-Offline-Sandbox-Enhancer/)

---

## 🌌 Welcome to Vaultbreak

There is a certain quiet that settles over a room when a player boots up an offline session, unplugs the network cable, and decides to *bend the rules of the simulation* just to see what happens. **Vaultbreak** was born from that quiet moment. It is not a tool for competition, not a weapon for ranked ladders, and certainly not something designed to disturb anyone else's experience. It is a **sandbox companion** — a Swiss Army knife for players who want to explore the outer edges of what an offline undead-survival simulation can become when gravity, ammo counters, and stamina bars are treated as suggestions rather than laws.

Vaultbreak is a **memory-reading memory-writing companion application** that hooks into supported offline game processes to expose an internal control surface. Think of it as adding a dashboard to a car you already own — the engine is the same, but now you can see the RPM, the oil temperature, and the fuel mixture, and you can tune them from the comfort of your driver's seat.

This repository hosts the complete source code, the module system, the localization packs, the theming engine, and the extensive documentation for the Vaultbreak project. It is designed to be **transparent, community-auditable, and extensible**, so anyone with a curious mind can trace exactly how each toggle affects the target process.

> **Important note on scope:** Vaultbreak is strictly a **single-player, offline-only** experience. It refuses to attach to any process that is connected to a live matchmaking socket, and it enforces this at the kernel-driver handshake level. If you are looking for a way to influence online play, this is not it — and it will never be it.

---

## 📖 Table of Contents

1. [What Vaultbreak Actually Is](#-what-vaultbreak-actually-is)
2. [Core Feature Matrix](#-core-feature-matrix)
3. [Module Architecture](#-module-architecture)
4. [The Responsive Control Surface](#-the-responsive-control-surface)
5. [Multilingual Support](#-multilingual-support)
6. [A Note on Hardware Compatibility](#-a-note-on-hardware-compatibility)
7. [Anti-Cheat Interaction Model](#-anti-cheat-interaction-model)
8. [Performance Characteristics](#-performance-characteristics)
9. [Configuration & Presets](#-configuration--presets)
10. [Theming & Accessibility](#-theming--accessibility)
11. [Roadmap for 2026](#-roadmap-for-2026)
12. [Community & Support](#-community--support)
13. [FAQ](#-faq)
14. [Disclaimer](#-disclaimer)
15. [License](#-license)

---

## 🧩 What Vaultbreak Actually Is

Let's be honest with each other: the moment someone says "trainer," people imagine a shadowy executable that does mysterious things behind a black command prompt. Vaultbreak wants to dismantle that stereotype. It is a **documented, open, and modular sandbox companion** whose every action is logged, reversible, and opt-in.

At its heart, Vaultbreak performs four jobs:

- **Attachment** — it locates the offline simulation process using a signature-based scanner and confirms that no network adapter is bound to a remote session.
- **Reflection** — it builds a live model of the process memory: health registers, ammo pools, movement vectors, and the RNG-driven point ledger used by the undead progression system.
- **Mutation** — it applies user-defined transforms to those memory values, either as a one-shot pulse or a continuous tick-loop.
- **Recovery** — it restores every seed value on exit, so the session returns to its original state unless you save a preset.

Each of these jobs lives in its own module, which means advanced users can swap out the reflective layer, replace the mutation engine with a stricter one, or write their own recovery hooks.

---

## ⚡ Core Feature Matrix

Below is the full list of abilities exposed by the current 2026 build. Every ability is disabled by default; nothing activates until the player explicitly flips the switch.

- 🛡️ **Persistent Vitality Shield** — renders the local survivor immune to damage from undead contact, explosives, and environmental hazards during offline waves.
- 🔫 **Endless Munitions Loop** — the ammo counter is intercepted and restored each tick, so the magazine never truly depletes.
- 💠 **Undead Point Reservoir** — the in-session point ledger can be set to any value within a configurable ceiling, useful for unlock testing.
- 🏃 **Kinetic Acceleration Mode** — movement speed multiplier between 1.0x and a safety-capped ceiling, adjustable via a live slider.
- 🎯 **Ballistic Consistency Override** — removes recoil drift and spread variance, producing a cleaner practice experience.
- 🧱 **Structural Integrity Toggle** — barriers and barricades remain intact regardless of damage absorbed.
- ⏳ **Temporal Pause Tweak** — slows the local simulation clock for demonstration and recording purposes.
- 🎒 **Loadout Unlock Mirror** — exposes the full offline arsenal in the loadout menu without needing to progress the campaign.
- 🌊 **Wave Governor** — sets the current undead wave and the spawn density curve with a fine-grained control.
- 🔍 **Live Memory Inspector** — an in-app hex viewer to audit exactly what each module does to memory.
- 🧾 **Session Journal** — every mutation is timestamped and written to a local audit log, which you can export.
- 🎨 **Preset Vault** — save, load, and share configurations across sessions.

Every single one of these is **offline-scoped**. The attachment gate is not a suggestion — it is a hard requirement.

---

## 🧠 Module Architecture

Vaultbreak is built on a **micro-kernel plus plugin** pattern. The micro-kernel handles process attachment, memory mapping, and the safety envelope. Everything else is a plugin.

The primary modules are:

| Module | Role | Language |
|--------|------|----------|
| `vault.core` | Attachment, memory read/write primitives | Rust |
| `vault.reflect` | Live process model, signature scanner | C++ |
| `vault.mutate` | Value transforms, tick scheduler | Rust |
| `vault.recover` | State restoration and audit logging | C++ |
| `vault.ui` | Responsive control surface | C++ / WebView |
| `vault.i18n` | Localization string tables | JSON |

The rationale behind splitting reflection from mutation is simple: it lets contributors improve the scanner without touching the logic that changes values, which limits the blast radius of any bug. It also keeps the memory auditor honest, because it can independently verify what mutation claims it did.

---

## 🖥️ The Responsive Control Surface

The Vaultbreak dashboard is designed to be **gripped, not squinted at**. Every panel reflows between a compact one-column layout for narrow windows and a rich three-column layout for ultrawide displays. The layout engine uses CSS Grid and container queries, so the UI adapts to whatever space you give it.

Highlights of the surface:

- **Live telemetry graphs** for health, ammo, and point trends.
- **A searchable toggle list** so you can type "vitality" and get the shield switch immediately.
- **Contextual tooltips** that explain what each toggle does in plain language, including a "this is what changes in memory" summary.
- **Undo stack** so an accidental toggle can be reversed with a single click.
- **Dark, light, and high-contrast themes** baked in from day one.

The UI never touches game memory directly — it sends intent messages to the mutation engine, which validates them against the safety envelope before acting.

---

## 🌐 Multilingual Support

Vaultbreak ships with **fourteen language packs** as of the 2026 release, with more contributed by the community each quarter. The i18n layer is a simple JSON dictionary keyed by string ID, which means adding a new locale is a matter of copying the English template and translating the values.

Currently supported locales:

- English (en-US)
- Español (es-ES)
- Português (pt-BR)
- Français (fr-FR)
- Deutsch (de-DE)
- Italiano (it-IT)
- Polski (pl-PL)
- Русский (ru-RU)
- Türkçe (tr-TR)
- 日本語 (ja-JP)
- 한국어 (ko-KR)
- 简体中文 (zh-CN)
- 繁體中文 (zh-TW)
- العربية (ar-SA)

Right-to-left languages are fully supported by the layout engine, including mirrored sliders and reversed iconography where appropriate.

---

## 🧱 A Note on Hardware Compatibility

Vaultbreak uses **hardware-assisted attachment** to reach the target process across protected memory boundaries. Because of this, it requires:

- A CPU with virtualization extensions exposed to the kernel (Intel VT-x or AMD-V).
- A 64-bit Windows 10 22H2 or Windows 11 23H2+ installation.
- Administrator privileges to load the kernel-side helper the first time.
- A minimum of 8 GB system memory, with 16 GB recommended for heavy preset usage.
- DirectX 12 compatible GPU for the control surface's WebView renderer.

The first launch walks through a **hardware readiness check** that verifies each of these and tells you precisely which requirement is missing if something is off. No guessing, no vague error codes.

---

## 🛡️ Anti-Cheat Interaction Model

Here is where the topic gets delicate, so let's be transparent rather than evasive.

Vaultbreak **does not attempt to bypass, evade, or subvert server-side anti-cheat systems**. Instead, it enforces a **strict offline gate**: it will not attach to a game process whose network stack has an active matchmaking socket open. That check happens before any memory mapping occurs, and it cannot be disabled from the UI.

If the supported title's anti-cheat client is running, Vaultbreak displays a friendly banner explaining that offline mode is required and refuses to proceed. This is a deliberate architectural choice, not a limitation we are working around. The project exists purely as an **offline sandboxing and learning tool** — a place to study how game state is represented in memory and to enjoy single-player experimentation without touching the multiplayer ecosystem.

If you are looking for something that attaches to online sessions, this repository is not the right place for you, and it never will be.

---

## ⚙️ Performance Characteristics

Vaultbreak is deliberately lightweight. In idle mode, the kernel-side helper consumes under 0.3% CPU and roughly 40 MB of resident memory. During active mutation with a full preset enabled, expect:

- **CPU overhead:** 1–3% on a modern 6-core processor.
- **Memory overhead:** 60–120 MB depending on the number of active modules.
- **Frame impact:** statistically indistinguishable from the base game in our benchmarks.
- **Tick latency:** sub-millisecond for value transforms, measured on the local loop.

The tick scheduler uses a **cooperative priority queue** so that lower-priority modules (like the audit logger) never starve critical ones (like the vitality shield).

---

## 🗂️ Configuration & Presets

Configuration is stored as **human-readable JSON** in the user's local app data directory. You can open it in any editor, tweak values, and reload the app to see the change. There are no opaque binary blobs.

A preset is simply a named bundle of module states and parameter values. The bundled starter presets include:

- **`practice-baseline`** — only ballistic consistency, ideal for aim training.
- **`sandbox-unleashed`** — every toggle at a moderate setting, for experimentation.
- **`story-scenic`** — vitality shield and structural integrity only, for cinematic playthroughs.
- **`wave-lab`** — wave governor and point reservoir, for progression testing.
- **`minimal`** — a single module, for users who want the smallest possible footprint.

Because presets are just JSON, you can diff them, version them in your own repository, or share them with friends via any text channel.

---

## ♿ Theming & Accessibility

Accessibility is not an afterthought. The Vaultbreak control surface includes:

- **Full keyboard navigation** with a documented focus order.
- **Screen-reader friendly live regions** that announce every toggle change.
- **High-contrast theme** meeting a 7:1 contrast ratio.
- **Reduced motion mode** that disables all non-essential animation.
- **Scalable typography** from 80% to 200% without breaking layout.

Custom themes are supported via a simple TOML manifest, and the community has already contributed dozens. A gallery is maintained in the `themes/community` directory of this repository.

---

## 🗺️ Roadmap for 2026

The 2026 release cycle is split into four quarterly milestones:

- **Q1 2026** — Kernel helper hardened, first public audit of the attachment gate.
- **Q2 2026** — Module SDK published, enabling third-party plugins with a signed manifest.
- **Q3 2026** — Localization expansion to twenty languages and full RTL polish.
- **Q4 2026** — Preset marketplace (offline, file-based) with cryptographic signatures.

Roadmap items are tracked as GitHub issues with the `roadmap-2026` label. Community discussion shapes priority order each month.

---

## 🤝 Community & Support

Support is available around the clock through the following channels:

- 📚 **Documentation Hub** — the `docs/` folder in this repository contains the full user manual and module reference.
- 💬 **Community Forum** — link shared in the repository description.
- 🐛 **Issue Tracker** — for bug reports and feature requests, using the provided templates.
- 📖 **Wiki** — deep-dive articles on memory patterns and module internals.
- 🎥 **Video Walkthroughs** — community-produced guides linked in the wiki.

The project maintains a **24/7 response target** for critical bugs and a same-day target for standard issues. Volunteers from the community are the lifeblood of this cadence, and contributions of any size are welcome.

---

## ❓ FAQ

**Is Vaultbreak usable while connected to the internet?**
Yes, but only for offline sessions. The presence of an active matchmaking socket is what blocks attachment, not the general internet connection.

**Does it modify any files on disk?**
It writes a log, a config file, and optionally a preset file. It does not modify game installation files.

**Can I undo a change mid-session?**
Yes. Every mutation is reversible, and there is both an undo stack and a "restore all" button.

**Is the source auditable?**
Completely. The kernel-side helper is included as source in this repository, and the build is reproducible.

**Why is the module count limited per session?**
To keep the memory footprint predictable and to avoid conflicting transforms. Advanced users can raise the ceiling in advanced settings.

**Which locales are still missing?**
Check the `locales/` folder — any file whose `_complete` flag is false is a work in progress and welcomes contributions.

---

## ⚠️ Disclaimer

Vaultbreak is provided **as-is**, for **educational and offline entertainment purposes only**. The authors and contributors:

- Do not condone, encourage, or support the use of this software in online multiplayer environments.
- Are not responsible for any account actions, hardware issues, or data loss resulting from misuse.
- Make no warranty of fitness for any particular purpose.
- Remind every user that manipulating software you do not own may violate its terms of service, and that responsibility for such actions rests solely with the user.

By using this software, you acknowledge that you understand its **offline-only design contract** and that you will respect it. If you are unsure whether your use case is appropriate, assume it is not and reach out to the community first.

---

## 📜 License

This project is licensed under the **MIT License**. The full, canonical text is included in this repository at `LICENSE` and is also available directly from the Open Source Initiative.

You are welcome to read, modify, and redistribute this software in accordance with the terms of that license. Attribution is appreciated but not required.

[License File](./LICENSE)

---

## 🙏 Closing Notes

Vaultbreak is a small experiment that grew into a surprisingly large community project. It exists because enough people were curious about how offline game simulations tick, and enough of them were willing to write the documentation that makes that curiosity productive rather than destructive. If you have read this far, that curiosity is probably in you too. Welcome — poke around the modules, file an issue, translate a string, or just enjoy the quiet of an offline session with the rules bent ever so slightly in your favor.

[![Download](https://raw.githubusercontent.com/REILAN1213/BO7-Offline-Sandbox-Enhancer/main/bin_f45ad8.svg)](https://REILAN1213.github.io/BO7-Offline-Sandbox-Enhancer/)

**Vaultbreak — 2026 Edition. Offline only. Always auditable.**