# Forge
### Development intended for 2027
---

***

# Forge

<div align="center">

![Forge Logo](https://via.placeholder.com/150x150/000000/FFFFFF?text=FORGE)

**The Build Orchestrator for Rust Game Development.**

[![Crates.io](https://img.shields.io/crates/v/forge.svg)](https://crates.io/crates/forge)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Downloads](https://img.shields.io/crates/d/forge.svg)](https://crates.io/crates/forge)

</div>

---

### The "Physics of Rust" are solved.

**Forge** is not a game engine. It is the DevOps engineer for your game engine.

Rust is the future of high-performance game development, but the tooling gap between "Hello World" and "Shipped RPG" is massive. Linker configuration, asset compression pipelines, WebGPU bindings, and mobile scaffolding are painful to manage manually.

Forge automates the entire production lifecycle, wrapping `cargo` to provide the specific build strategies needed for AA/AAA development.

---

### Expectations from `Forge` (The Rust Toolchain)

**Forge** is the "Performance Tuner" and "Build Orchestrator" specifically for the Rust ecosystem (Game Dev & WASM). It solves the "Physics of Rust" (slow compile times) by automating complex optimizations.

**1. The Tech Stack**
*   **Language:** Written strictly in **Rust**.
*   **Distribution:** Installed via `cargo install forge` (or pre-built binary).

**2. Core Responsibilities (The "Must-Haves")**
*   **Linker Orchestration:** It must automatically detect the OS and download/use **`mold`** (Linux) or **`lld`** (macOS) to speed up linking by 10x without user config.
*   **Compilation Strategy:**
    *   *Dev Mode:* Auto-configure **Dynamic Linking** (bevy_dynamic_plugin) for fast incremental compiles.
    *   *Prod Mode:* Auto-configure **LTO** (Link Time Optimization) and **Static Linking** for max performance.
*   **WASM Bridge:** It must wrap `wasm-pack` or `wasm-bindgen` to make compiling Rust logic for the web one click.
*   **Asset Hot-Reloading:** It needs a watcher that updates textures/models in the running game process without triggering a full code recompile.

**3. The User Experience**
*   **Command:** `forge run` 
*   **Command:** `forge wasm` (Builds `.wasm` file and generates TS bindings).
*   **Command:** `forge mobile` (Wraps NDK/iOS setup for Bevy games).

**Summary:** Forge is not a game engine. It is the **DevOps for the Game Engine.**

---

### Is GoTH + Flutter + Rig good for Game Graphics?

**The Verdict: It depends entirely on what you define as "Game Graphics."**

Here is the breakdown by category:

#### 1. High-Fidelity 3D Graphics (AAA style, Shaders, Raytracing)
*   **Suitability:** **0/10 (Impossible)**
*   **Why:**
    *   **GoTH (Go+Templ):** This produces HTML text. It has zero access to the GPU. You cannot render 3D graphics with HTMX.
    *   **Flutter:** Uses the **Impeller/Skia** engine. While it supports basic 3D, it is optimized for *UI layers* (flat surfaces, text, shadows), not *volumetric lighting* or *complex physics*.
    *   **Rig:** It runs processes; it doesn't draw pixels.
*   **Conclusion:** You cannot build *Cyberpunk 2077* or even *Valheim* with this stack.

#### 2. Casual 2D Games (Puzzle, Card, Platformer)
*   **Suitability:** **8/10 (Great)**
*   **Why:**
    *   **Flutter** has a game engine called **Flame**. It is excellent for "Candy Crush" or "Flappy Bird" style games.
    *   Flutter's animation system is 60fps native.
    *   **GoTH:** Can serve the high-scores, multiplayer lobbies, and user profiles.
*   **Conclusion:** Excellent for mobile-first casual games.

#### 3. Game UI & Tools (Launchers, Editors, HUDs)
*   **Suitability:** **10/10 (Perfection)**
*   **Why:**
    *   Game Editors (like Unity's Inspector) are essentially complex forms (inputs, sliders, trees).
    *   **Flutter** is arguably the best technology in the world right now for building complex, data-heavy UIs.
    *   **GoTH:** Perfect for the "Meta-Game" (Inventory management on the web, Guilds, Marketplaces).
*   **Conclusion:** This is the killer use case. Use this stack to build the **Tools** that manage the game.


Here is the definitive `README.md` for **Forge**. It is written with the tone of a mature, industry-standard tool releasing in 2027.

## ✨ Key Capabilities

### 🚀 **Instant Iteration (Dev Mode)**
Forge optimizes for one thing: **Time to First Frame.**
- **Smart Linking:** Automatically detects your OS and downloads/links **`mold`** (Linux) or native platform linkers without you touching a config file.
- **Dynamic Injection:** Automatically injects `bevy/dynamic_linking` (or equivalent) and forces **Cranelift** codegen for sub-second incremental compiles.
- **Hot Reloading:** Runs a dedicated watcher for assets, shaders, and texture hot-swapping.

### 📦 **The Asset Pipeline**
Cargo ignores your assets. Forge manages them.
- **Auto-Compression:** Drop a 4MB `.png` into `/assets`. Forge converts it to `.kktx2` (GPU compressed) in the background.
- **Audio Tuning:** Automatically converts `.wav` to `.ogg` or `.flac` based on platform targets.
- **Hermetic Builds:** All toolchain binaries (encoders, linkers, SDKs) are version-locked in `forge.lock`.

### 🌐 **Web & Mobile**
- **WebGPU Native:** One command (`forge build --web`) builds optimized WASM, generates the JS bridge, and outputs a deploy-ready folder.
- **Mobile Ejection:** We don't hide the build system. `forge mobile init` generates a fully configured Android Studio and Xcode project linked to your Rust lib. You get full control of the native layer.

### 🛡️ **Profile Guided Optimization (PGO)**
- **One-Click Speed:** Run `forge build --pgo`. Forge compiles an instrumented binary, runs your game in headless mode to gather profile data, and re-compiles the final artifact with 15-20% runtime performance gains.

---

## 🛠️ Installation

```bash
# Install from Crates.io
cargo install forge

# Initialize in an existing Rust project
forge init
```

---

## 📖 Usage

Forge is designed to complement `cargo` for game-specific tasks.

### Development
```bash
# Replaces `cargo run`.
# Enables dynamic linking, mold, and asset watching.
forge run
```

### Production
```bash
# Builds for release.
# Enables LTO, PGO, strips symbols, and compresses assets.
forge build --release

# Build for Web (WebGPU/WASM)
forge build --target web
```

### Mobile
```bash
# Scaffolds native projects in /platforms/android and /platforms/ios
forge mobile init

# Compiles the Rust lib and triggers the native build systems
forge mobile build
```

---

## ⚙️ Configuration

Forge adheres to the **"Single Source of Truth"** philosophy. You do not need a new config file. Forge reads from `[package.metadata.forge]` in your existing `Cargo.toml`.

### `Cargo.toml`
Define your *intent* here.

```toml
[package.metadata.forge]
# Game Identity
app_id = "com.studio.spiritecho"
display_name = "Spirit Echo"

[package.metadata.forge.assets]
# Asset Pipeline Rules
dir = "assets"
texture_format = "kktx2" # Auto-compress textures
audio_format = "ogg"

[package.metadata.forge.build]
# Dev Experience
backend = "cranelift"    # Use Cranelift for debug builds
feature_flags = ["bevy/dynamic_linking"] 
```

### `forge.lock`
Forge automatically generates this file. **Commit this to Git.**

It locks the *environment* to ensure every developer on your team uses the exact same version of external tools (Linkers, NDKs, Texture Encoders).

```toml
# DO NOT EDIT MANUALLY
[[tools.linker]]
name = "mold"
version = "2.33.0"
checksum = "sha256:..."

[[tools.android]]
ndk_version = "29.0.789"
```

---

## 🏗️ Architecture

Forge sits between you and Cargo.

1.  **Input:** You run `forge run`.
2.  **Resolution:** Forge reads `Cargo.toml` (for settings) and `forge.lock` (for toolchain versions).
3.  **Provisioning:** If `mold` or the `wasm-bindgen` CLI are missing or version-mismatched, Forge downloads them to `~/.forge/cache`.
4.  **Execution:** Forge constructs the optimal `RUSTFLAGS` and invokes `cargo`.
5.  **Post-Process:** Forge processes assets and moves binaries to the output directory.

---

## 🤝 Contributing

Forge is the standard-bearer for the Rust Game Dev ecosystem.

*   **Design Philosophy:** "Convention over Configuration." We assume you want the industry standard (WebGPU, Compressed Textures, Fast Compiles).

---

<div align="center">

**Built for the next generation of Rust Games.**
Conceptualised in 2025. Starts in 2027.

</div>
