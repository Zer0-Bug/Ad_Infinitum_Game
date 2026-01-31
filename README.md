<h1 align="center">Ad Infinitum - C++ Recreation</h1>

<p align="center">
  <a href="https://en.cppreference.com/w/cpp/20">
    <img src="https://img.shields.io/badge/C%2B%2B-20-blue?style=for-the-badge&logo=c%2B%2B&logoColor=white" alt="C++20">
  </a>
  <a href="https://www.microsoft.com/windows">
    <img src="https://img.shields.io/badge/Platform-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="Windows">
  </a>
  <a href="https://otoidrak.com/">
    <img src="https://img.shields.io/badge/Library-ICBYTES-orange?style=for-the-badge" alt="ICBYTES">
  </a>
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-darkred?style=for-the-badge" alt="License">
  </a>
</p>

<p align="center">
  <b>A modern high-fidelity recreation of the Commodore 64 classic Ad Infinitum.</b><br><br>
  <i>Developed in C++ for Windows using the icbytes low-level graphics engine.</i>
</p>
<br>
<p align="center">
  <a href="#technical-architecture">
    <img src="https://img.shields.io/badge/Architecture-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#project-structure">
    <img src="https://img.shields.io/badge/Structure-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#processing-pipeline">
    <img src="https://img.shields.io/badge/Pipeline-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#detailed-module-specifications">
    <img src="https://img.shields.io/badge/Modules-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#technical-specifications">
    <img src="https://img.shields.io/badge/Specs-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#deployment--installation">
    <img src="https://img.shields.io/badge/Deploy-222222?style=flat" />
  </a>
</p>

---
<br>
<h2 align="center">📜 Historical Context & Heritage</h2>

**Ad Infinitum** is a modern project dedicated to the preservation and recreation of an unreleased prototype for the Commodore 64. This prototype later evolved into the space shooter **W.A.R.** (released by Martech in 1986).

### 🕹️ The Commodore 64 Original
Originally developed in **6502 assembly language**, the prototype leveraged the 1 MHz CPU and 64 KB RAM of the C64. A key technical differentiator was its support for **bidirectional horizontal scrolling**, a feature that was restricted in the final commercial release of *W.A.R.*

> [!NOTE]
> The prototype featured a title screen font and mechanics heavily inspired by *Uridium*, which likely led to the alterations made before its official release to avoid legal complications.

### 🎥 Media & References
*   **Original Gameplay:** [Watch on YouTube](https://www.youtube.com/watch?v=PGzjrlfvbmE)
*   **Technical Goal:** Replicate the VIC-II chip's sprite-based graphics and smooth scrolling behavior within a C++ environment.

---
<br>
<h2 align="center">Technical Architecture</h2>

The core architecture of **Ad Infinitum** is designed to replicate the constraints and aesthetics of 8-bit hardware within a modern C++ environment. It utilizes a modular approach for its engine components:

1.  **Low-Level Graphics (ICBYTES):** The engine provides direct pixel access and hardware-accelerated sprite blitting, enabling smooth 60 FPS bidirectional scrolling.
2.  **State-Driven Level Management:** Levels are treated as independent architectural entities, loaded dynamically from high-resolution assets and mapped to collision matrices.
3.  **Real-Time Sound Synthesis:** An interrupt-driven audio system that manages concurrent `.wav` streams for background music and tactical sound effects.
4.  **Collision & Physics Engine:** A custom AABB (Axis-Aligned Bounding Box) logic tailored for sprite-to-sprite and sprite-to-environment interactions.

---
<br>
<h2 align="center">Project Structure</h2>

```
Ad_Infinitum_Game/
├── Ad_Infinitum.exe                       # Compiled Windows executable
├── LICENSE                                # MIT License
├── README.md                              # Project documentation
├── .gitattributes                         # Git configuration
│
├── Audio/                                 # Sound Engineering Sub-Project
│   ├── Intro.wav                          # High-fidelity theme music
│   ├── bullet.wav                         # Projectile discharge effects
│   ├── enemyHit.wav                       # Entity destruction feedback
│   ├── fuel.wav                           # Resource acquisition signal
│   ├── selfDestruct.wav                   # Player termination feedback
│   └── End.wav                            # Game-over sequence audio
│
├── BOOM/                                  # Explosion Animation System
│   ├── BOOM_V1.jpg                        # Frame 01: Initial ignition
│   ├── BOOM_V2.jpg                        # Frame 02: Expansion phase
│   └── BOOM_V3.jpg                        # Frame 03: Dissipation phase
│
├── Levels/                                # World Building & Level Design
│   ├── 1. LEV_00-09/                      # Primary Stage Data (0-9)
│   ├── 2. LEV_0A-0F/                      # Advanced Stage Data (A-F)
│   └── Dock/                              # Specialized Docking Logistics
│
└── Sprites (Root Assets)/                 # Visual Entity Definitions
    ├── PIXEL_UcakV1.jpg                   # Player aircraft configurations
    ├── PIXEL_YananUcakV1.jpg              # Damage state representations
    ├── Ucak_SagAlt.jpg                    # Directional vector sprites
    └── ...                                # Directional and State-based assets
```

---
<br>
<h2 align="center">Processing Pipeline</h2>

### 1. Engine Initialization
Utilizing the **icbytes** library, the system initializes the Windows GDI/DirectX context. It allocates the necessary memory buffers for the back-buffer rendering system to prevent flickering during high-speed horizontal scrolling.

### 2. Asset Serialization
The game logic serializes JPG assets from the `Levels/` and `BOOM/` directories into memory-resident textures. Level data is analyzed to define the "playable" space and "lethal" boundaries.

### 3. Input Handling & Flight Dynamics
The `W-A-S-D` input matrix is processed every frame. Unlike standard shooters, this engine supports **bidirectional velocity**, allowing the craft to rotate and scroll the entire world in reverse, maintaining the technical spirit of the original prototype.

### 4. Collision Detection Matrix
The engine executes a continuous check between the player's aircraft bounding box and active enemy entities.
```cpp
// Logic conceptualization of the icbytes-based collision
bool checkCollision(Sprite a, Sprite b) {
    return (a.x < b.x + b.w && a.x + a.w > b.x &&
            a.y < b.y + b.h && a.y + a.h > b.y);
}
```

### 5. Audio Concurrency
Sound effects are triggered via asynchronous threads to ensure that audio playback (e.g., `bullet.wav`) does not block the main rendering loop, providing a seamless auditory experience.

---
<br>
<h2 align="center">Detailed Module Specifications</h2>

### 1. Audio Management Project (Audio/)
This module serves as the auditory backbone of the game. It is not merely a collection of files but a choreographed soundscape:
- **Intro & End Logic:** High-bitrate WAV files that handle the game's emotional arc.
- **Feedback Loop:** Dedicated sound triggers for `fuel` and `enemyHit` provide the player with essential non-visual game state information.
- **Technical Implementation:** Uses multi-channel mixing to overlay weapon sounds on top of the looping theme music.

### 2. Sprite Animation System (BOOM/)
The `BOOM` project manages the visual representation of destruction within the engine.
- **Frame Sequencing:** A procedural state machine iterates through `BOOM_V1` to `BOOM_V3` when an entity's health reaches zero.
- **Visual Fidelity:** Uses pixel-accurate JPG frames to simulate 8-bit particle effects while maintaining modern resolution compatibility.
- **Resource Management:** Optimized for rapid loading and frequent re-use across multiple simultaneous explosion events.

### 3. Level Design & World Architecture (Levels/)
The levels are organized using a hexadecimal nomenclature reflecting the project's low-level roots:
- **LEV_00-09:** Represents the "Standard Exploration" phase, focusing on narrow corridors and rapid enemy spawns.
- **LEV_0A-0F:** Advanced zones with increased complexity and bidirectional scrolling requirements.
- **Docking Module:** A unique sub-project within the levels that manages the transition between flight combat and atmospheric docking, requiring specialized sprite transformations.

---
<br>
<h2 align="center">Technical Specifications</h2>

<table align="center">
  <tr>
    <th align="center">Component</th>
    <th align="center">Specification</th>
  </tr>
  <tr>
    <td align="center">Language</td>
    <td align="center">C++ (ISO Standard)</td>
  </tr>
  <tr>
    <td align="center">Graphics API</td>
    <td align="center">icbytes (GDI/DirectX Wrapper)</td>
  </tr>
  <tr>
    <td align="center">Original Hardware</td>
    <td align="center">Commodore 64 (MOS 6510)</td>
  </tr>
  <tr>
    <td align="center">Resolution</td>
    <td align="center">Modern Scaled Windowed Mode</td>
  </tr>
  <tr>
    <td align="center">Genre</td>
    <td align="center">Horizontal / Bidirectional Shooter</td>
  </tr>
</table>

---
<br>
<h2 align="center">Input / Output Context</h2>

- **Input Handling:**
    - **W, A, S, D:** Translation vectors for the player's aircraft across the X and Y axes.
    - **Space:** Trigger for the projectile emission system.
- **Output Visualization:**
    - Real-time rendering of sprite states and level parchment.
    - Dynamic sound output via the Windows MMSystem.

---
<br>
<h2 align="center">Deployment & Installation</h2>

### Repository Acquisition
To synchronize this project locally:
```bash
git clone https://github.com/Zer0-Bug/Ad_Infinitum_Game.git
```

### Environment Configuration
The project is optimized for **Visual Studio 2022**. 
1. Open the `.sln` file (if available) or create a new "Empty C++ Project".
2. Link the `icbytes` library headers and binaries.
3. Ensure the `Audio/`, `BOOM/`, and `Levels/` directories are in the relative execution path.

### Execution
Simply run the pre-compiled binary:
```bash
./Ad_Infinitum.exe
```

---
<br>
<h2 align="center">Contribution</h2>

Contributions are always appreciated. Open-source projects grow through collaboration, and any improvement—whether a bug fix, new feature, documentation update, or suggestion—is valuable.

To contribute, please follow the steps below:

1. Fork the repository.
2. Create a new branch for your change:  
   `git checkout -b feature/your-feature-name`
3. Commit your changes with a clear and descriptive message:  
   `git commit -m "Add: brief description of the change"`
4. Push your branch to your fork:  
   `git push origin feature/your-feature-name`
5. Open a Pull Request describing the changes made.
<br>
All contributions are reviewed before being merged. Please ensure that your changes follow the existing code style and include relevant documentation or tests where applicable.

---

<br>
<p align="center">
  <a href="mailto:777eerol.exe@gmail.com">
    <img src="https://cdn.simpleicons.org/gmail/D14836" width="40" alt="Email">
  </a>
  <span> × </span>
  <a href="https://www.linkedin.com/in/eerolexe/">
    <img src="https://upload.wikimedia.org/wikipedia/commons/c/ca/LinkedIn_logo_initials.png"
         width="40"
         alt="LinkedIn">
  </a>
</p>

---

<p align="center" style="margin-top:10px; letter-spacing:4px;">
  ∞
</p>
