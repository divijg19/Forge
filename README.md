# Forge

---

### Expectations from `forge` (The Rust Toolchain)

**Forge** is the "Performance Tuner" and "Build Orchestrator" specifically for the Rust ecosystem (Game Dev & WASM). It solves the "Physics of Rust" (slow compile times) by automating complex optimizations.

**1. The Tech Stack**
*   **Language:** Written strictly in **Rust**.
*   **Distribution:** Installed via `cargo install forge` (or pre-built binary).

**2. Core Responsibilities (The "Must-Haves")**
*   **Linker Orchestration:** It must automatically detect the OS and download/use **`mold`** (Linux) or **`zld`** (macOS) to speed up linking by 10x without user config.
*   **Compilation Strategy:**
    *   *Dev Mode:* Auto-configure **Dynamic Linking** (bevy_dynamic_plugin) for fast incremental compiles.
    *   *Prod Mode:* Auto-configure **LTO** (Link Time Optimization) and **Static Linking** for max performance.
*   **WASM Bridge:** It must wrap `wasm-pack` or `wasm-bindgen` to make compiling Rust logic for the web one click.
*   **Asset Hot-Reloading:** It needs a watcher that updates textures/models in the running game process without triggering a full code recompile.

**3. The User Experience**
*   **Command:** `forge run` (Replaces `cargo run` but faster).
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
