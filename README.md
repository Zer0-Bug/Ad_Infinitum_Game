<h1 align="center">Ad Infinitum</h1>

<p align="center">
  <a href="https://visualstudio.microsoft.com/">
    <img src="https://img.shields.io/badge/C%2B%2B-Windows-blue?style=for-the-badge&logo=c%2B%2B&logoColor=white" alt="C++ Windows">
  </a>
  <a href="https://otoidrak.com/">
    <img src="https://img.shields.io/badge/Library-icbytes-darkgreen?style=for-the-badge" alt="icbytes">
  </a>
  <a href="https://en.wikipedia.org/wiki/Commodore_64">
    <img src="https://img.shields.io/badge/Platform-C64%20Recreation-orange?style=for-the-badge" alt="C64 Recreation">
  </a>
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-darkred?style=for-the-badge" alt="License">
  </a>
</p>

<p align="center">
  <b>A modern, performance-oriented recreation of the classic Commodore 64 prototype.</b><br><br>
  <i>Leveraging C++ and the icbytes library to breathe new life into bidirectional scrolling space shooters.</i>
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
<h2 align="center">Technical Architecture</h2>

The Ad Infinitum recreation serves as a bridge between 1980s 8-bit logic and modern 64-bit multi-threaded environments. The architecture is engineered for low-latency graphics and fluid bidirectional movement:

1.  **Rendering Engine (icbytes):** Utilizes direct pixel manipulation and GDI+ wrappers to emulate the VIC-II's sprite behavior.
2.  **State Management:** Implements a discrete game loop that synchronizes player position, enemy AI patterns, and collision matrices.
3.  **Level Orchestration:** A modular loading system that partitions game levels into binary-grouped segments for optimized memory usage.
4.  **Input Mapping:** Asynchronous keyboard polling to enable simultaneous movement and firing, overcoming original hardware constraints.

---
<br>
<h2 align="center">Project Structure</h2>

```
Ad_Infinitum_Game/
├── Ad_Infinitum.exe                          # Compiled production binary
│
├── Audio/                                    # Captured wave-form audio engine
│   ├── Intro.wav                             # High-fidelity initialization sequence
│   ├── enemyHit.wav                          # Collision response feedback
│   └── bullet.wav                            # Asynchronous firing audio
│
├── Levels/                                   # Stage and environment logic
│   ├── 1. LEV_00-09/                         # Primary level cluster (0-9)
│   ├── 2. LEV_0A-0F/                         # Secondary level cluster (A-F)
│   └── Dock/                                 # Post-level transition mechanics
│
├── BOOM/                                     # Frame-based VFX repository
│   ├── BOOM_V1.jpg                           # Ignition frame
│   ├── BOOM_V2.jpg                           # Expansion frame
│   └── BOOM_V3.jpg                           # Dissipation frame
│
├── LICENSE                                   # MIT License terms
├── README.md                                 # Technical documentation
└── [PIXEL_UA assets].jpg                     # Sprite and tile-based definitions
```

---
<br>
<h2 align="center">Detailed Module Specifications</h2>

### 🌌 Levels Module
The core of the game's environment management. Unlike standard shooters, Ad Infinitum supports **Bidirectional Scrolling**.
- **Level Clustering:** Levels are grouped into logical partitions (`LEV_00-09` and `LEV_0A-0F`) to manage asset loading efficiently.
- **Docking Mechanics:** The `Dock/` sub-module implements the "Landing" sequence seen in the original prototype, requiring precision player positioning.
- **Technical Rigor:** Each level is rendered through a tile-based mapping system that emulates the VIC-II's memory-mapped character generator.

### 🔊 Audio Module
A technical adaptation of the legendary MOS Technology 6581 (SID) chip.
- **WAV Orchestration:** High-quality wave captures ensure that the retro "crunchy" feel of the original effects is preserved without the need for an emulator.
- **Concurrency:** The implementation supports concurrent audio channels, allowing the `Intro.wav` to play while `selfDestruct.wav` or `bullet.wav` effects are triggered by game events.

### 💥 Visual Effects (BOOM)
Frame-based sprite animation system.
- **Sequential Rendering:** The `BOOM` module manages a 3-stage sprite sequence (`V1` to `V3`) to simulate cinematic explosions.
- **Alpha Blending:** Emulates the retro transparency effects through color-key masking, ensuring sprites integrate seamlessly with the varying background levels.

---
<br>
<h2 align="center">Technical Specifications</h2>

<table align="center">
  <tr>
    <th align="center">Aspect</th>
    <th align="center">Details</th>
  </tr>
  <tr>
    <td align="center">Base Language</td>
    <td align="center">C++ (Windows Native)</td>
  </tr>
  <tr>
    <td align="center">Graphics Library</td>
    <td align="center">icbytes (Pixel/Sprite Engine)</td>
  </tr>
  <tr>
    <td align="center">Audio Interface</td>
    <td align="center">DirectSound / WinMM (WAV Integration)</td>
  </tr>
  <tr>
    <td align="center">Original Inspiration</td>
    <td align="center">Commodore 64 (VIC-II & SID)</td>
  </tr>
  <tr>
    <td align="center">Display Mode</td>
    <td align="center">Bidirectional Horizontal Scroller</td>
  </tr>
</table>

---
<br>
<h2 align="center">Deployment & Installation</h2>

### 1. Repository Acquisition
```bash
git clone https://github.com/Zer0-Bug/Ad_Infinitum_Game.git
cd Ad_Infinitum_Game
```

### 2. Execution
As the project contains the pre-compiled production binary, you can run the game directly:
1.  Ensure you are on a **Windows 10/11** environment.
2.  Execute `Ad_Infinitum.exe`.
3.  Ensure the `Audio`, `Levels`, and `BOOM` folders are in the same directory as the executable.

### 3. Development Setup
To modify the code or contribute to the engine:
- **IDE:** Visual Studio 2022 (Community or Professional).
- **Library:** Ensure the `icbytes` library headers are linked in your include path.

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
