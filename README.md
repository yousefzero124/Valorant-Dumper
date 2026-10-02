# 🛡️ Valorant SDK Dumper Pro: Comprehensive Project Guide
### Software Architecture, Programming Language Stack, Operational Manual & Patch Updating

<div align="center">

![Project Status](https://img.shields.io/badge/Status-Production%20Ready-success?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-v7.0.1%20PRO-red?style=for-the-badge)
![Engine](https://img.shields.io/badge/Engine-Ares--Core%20UE4.25+-blue?style=for-the-badge)
![Author](https://img.shields.io/badge/Developer-yousef__zero-orange?style=for-the-badge)

</div>

---

## 📸 Workflow Architecture & Lifecycle Infographic

The infographic and flowchart below illustrate the complete lifecycle of the software—from initial configuration to dynamic decryption, SDK generation, and recurring patch updates:

<div align="center">

![Workflow Guide](assets/workflow_guide.png)

</div>

```mermaid
flowchart TD
    subgraph S1 [Phase 1: Launch & Environment Setup]
        A["🚀 Execute Valorant-Dumper.exe"] --> B["⚙️ Load Config & Offsets from gui_config.json"]
    end

    subgraph S2 [Phase 2: Offset Configuration & Decryption Verification]
        B --> C{"Is Current Game Patch Supported?"}
        C -- "Yes (Offsets Valid)" --> D["🔨 Compile Dumper-7.dll via MSBuild"]
        C -- "No (Game Received New Patch)" --> E["🔍 Extract New Offsets via IDA Extractor Tab"]
        E --> F["💾 Save Offsets & Sync to C++ Generator Code"]
        F --> D
    end

    subgraph S3 [Phase 3: Safe Injection & SDK Extraction]
        D --> G["🎮 Launch Target Game: VALORANT"]
        G --> H["💉 Inject Dumper-7.dll with 64-bit Pointer Safety"]
        H --> I["📂 Parse FUObjectArray & Reconstruct Complete C++ SDK Headers"]
    end

    subgraph S4 [Phase 4: Maintenance & Patch Lifecycle]
        I --> J["🔄 Track Future Engine Patches & Iterate Pipeline"]
    end

    style S1 fill:#131A29,stroke:#00F5D4,stroke-width:2px,color:#fff
    style S2 fill:#172236,stroke:#FF4655,stroke-width:2px,color:#fff
    style S3 fill:#111726,stroke:#10B981,stroke-width:2px,color:#fff
    style S4 fill:#1F293D,stroke:#F59E0B,stroke-width:2px,color:#fff
```

---

## 💻 Technology Stack & Programming Languages

This project incorporates a multi-tier engineering architecture, pairing low-level systems programming with a reactive desktop management suite:

| Language / Technology | Standard / Version | Project Source Directory | Key Architectural Responsibilities |
| :--- | :--- | :--- | :--- |
| **C++** | ISO C++20 (MSVC v143) | [`Dumper-7-7.0.1/Dumper/`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Dumper-7-7.0.1/Dumper/) | Core injection DLL engine; raw memory pointer manipulation, 64-bit modulo decryption of `FUObjectArray`, Unreal Engine RTTI traversal, and syntactic C++ header generation. |
| **Python** | Python 3.10 – 3.13 | [`gui/`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/gui/) & [`run_gui.py`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/run_gui.py) | Modern Desktop GUI layer, asynchronous worker threads (`QThread`), real-time colored compilation streaming, and IDA decompiled pseudocode parser. |
| **PyQt6** | Qt 6.x Framework | [`gui/`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/gui/) | High-performance hardware-accelerated desktop UI components, layouts, custom title bar styling, and native Per-Monitor High-DPI scaling. |
| **x86-64 Assembly** | AMD64 Instruction Set | Disassembly & Reverse Engineering Docs | Binary inspection inside IDA Pro/Ghidra; calculating RIP-relative displacement (`[rip + offset]`), bitwise circular rotations (`ROR`/`ROL`), and 64-bit multiplication shifts (`IMUL`). |
| **Windows Batch (.bat)** | Windows Command Script | [`Launch-GUI.bat`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Launch-GUI.bat), [`Build-Executable.bat`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Build-Executable.bat), [`Build-Dumper-7.bat`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Build-Dumper-7.bat) | Toolchain automation, environment discovery (`vswhere.exe`), zero-config standalone executable compilation, and MSBuild automation. |
| **QSS / CSS3** | Custom Qt Stylesheets | [`gui/theme.py`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/gui/theme.py) | Custom Cyberpunk & Valorant-inspired dark design system with vibrant neon accents (`#FF4655` red and `#00F5D4` cyan). |
| **JSON** | Standard UTF-8 | [`gui_config.json`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/gui_config.json) | Persistent configuration store for MSBuild paths, target process metadata, and active memory offsets across sessions. |

---

### 🔍 Deep Dive: Core Roles of Each Language

#### 1. C++ (Modern C++20):
- **Purpose:** Executes directly inside the game's address space as a 64-bit Dynamic Link Library (`Dumper-7.dll`).
- **Core Operations:**
  - High-performance pointer arithmetic (`uintptr_t`, `reinterpret_cast`) without garbage collection overhead.
  - Implementation of Valorant's proprietary 7 switch-case mathematical decryption algorithm for `FUObjectArray`.
  - Memory read validation via [`Platform::IsBadReadPtr`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Dumper-7-7.0.1/Dumper/Engine/Private/Platform/Windows/Platform.cpp) to prevent access violation crashes (`STATUS_ACCESS_VIOLATION`) when inspecting unmapped memory pages.
  - Serialization and reconstruction of class inheritance hierarchies, virtual method tables (VTables), property bitmasks, and struct member alignments.

#### 2. Python 3 & PyQt6:
- **Purpose:** Delivers a complete, user-friendly control suite that eliminates the need for manual command-line interaction.
- **Core Operations:**
  - Dynamic discovery of MSBuild and Visual Studio toolchains via `vswhere`.
  - Regular Expression engine that parses raw IDA Pro C++ pseudocode to automatically extract Hexadecimal addresses (`TableOffset`, `KeyOffset`).
  - Python-based mathematical simulation of the 64-bit decryption routine to verify offsets before compilation.
  - Process lifecycle management, memory telemetry, and thread-safe process polling.

#### 3. x86-64 Assembly:
- **Purpose:** Understanding and translating machine code instructions during static and dynamic binary analysis.
- **Core Operations:**
  - Resolving RIP-relative operands: `LEA RAX, [RIP + 0xXXXXXXXX]` to identify the dynamic 7-element global pointer array.
  - Identifying cryptographic arithmetic operations: 64-bit integer multiplication (`imul`), bit rotations (`ror`, `rol`), logical XOR (`xor`), and bit shifting (`shr`, `shl`).

---

## 🚀 User Operation Manual

### 📌 How to Launch the Application:
You can start the application using any of the following three methods:

1. **Standalone Executable (Recommended):**
   - Double-click [`Valorant-Dumper.exe`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Valorant-Dumper.exe) (features the custom embedded cat icon and runs without a console window).
2. **One-Click Batch Launcher:**
   - Double-click [`Launch-GUI.bat`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Launch-GUI.bat).
3. **Python Interpreter (Developer Mode):**
   ```powershell
   python run_gui.py
   ```

---

### 🖥️ Overview of the 6 Application Tabs:

```text
┌────────────────────────────────────────────────────────────────────────┐
│  VALORANT SDK DUMPER PRO                                  [—] [□] [✕] │
├────────────────────────────────────────────────────────────────────────┤
│ [ 🔨 Build ] [ 🎯 Offsets ] [ 🔍 Extractor ] [ 💉 Injector ] [ 📊 Logs ] [ 📚 SDK ] │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│   Active Tab Controls, Diagnostic Telemetry, and Interactive Guides    │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

#### 1. 🔨 Build Engine Tab
- Automatically discovers installed **Visual Studio 2022** and `MSBuild.exe` instances.
- Allows configuration selection (`Release` / `x64` / `v143`).
- **"Build Dumper DLL" button:** Compiles [`Dumper-7.dll`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Dumper-7.dll) with real-time colored log streaming.
- **"Quick Dump Game" button:** Injects the newly compiled DLL directly into the active game process with a single click.

#### 2. 🎯 Offsets & Decryptor Math Tab
- Displays and edits currently active memory offsets:
  - `TableOffset`: Base offset of the 7-element pointer table for `FUObjectArray`.
  - `KeyOffset`: Base offset of the dynamic 32-bit decryption key.
  - `GWorldOffset`: Offset pointing to the active `UWorld` root container.
  - `FNamePoolOffset`: Offset pointing to the global string name registry (`GNames`).
- **Live Math Simulator:** Enter any 32-bit test key and instantly preview the resulting 64-bit decrypted pointer table index and mathematical transformation.
- **"Apply & Save Offsets" button:** Saves updates into [`gui_config.json`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/gui_config.json) and synchronizes C++ source files.

#### 3. 🔍 IDA Pseudocode Extractor Tab
- Simplifies offset updating after new game patches.
- Paste decompiled C++ pseudocode (from `FSeamlessTravelHandler::Tick` in IDA Pro) directly into the input editor.
- Click **"Extract Offsets"**: The software automatically parses `TableOffset` and `KeyOffset`, displays them in clean Hex formatting, and allows you to apply them immediately.

#### 4. 💉 Safe DLL Injector Tab
- Automatically enumerates running processes to detect `VALORANT-Win64-Shipping.exe`.
- Performs safe 64-bit DLL injection utilizing `LoadLibraryW` + `CreateRemoteThread` with automatic `SeDebugPrivilege` escalation.
- Includes an **"Eject DLL"** function (`FreeLibrary`) allowing you to unload the DLL cleanly and re-inject a new build without restarting the game.

#### 5. 📊 Process Monitor & Diagnostics Tab
- Displays live metrics including process ID (PID), memory usage (Working Set & Private Bytes), thread count, and parent process hierarchy.
- Provides historical log inspection and verification of generated output files.

#### 6. 📚 SDK Header Explorer Tab
- Built-in code browser designed to inspect generated Unreal Engine C++ header files (`.hpp` / `.cpp`).
- Features a real-time search and filter bar to instantly locate specific class definitions (e.g., `AresClient`, `ShooterCharacter`, `World`).

---

## 🔄 Step-by-Step Patch Update Guide (Reverse Engineering Workflow)

Whenever Riot Games releases a patch for Valorant, memory offsets change. Follow this standard reverse engineering workflow to update the tool in minutes:

```mermaid
sequenceDiagram
    autonumber
    actor Dev as Reverse Engineer
    participant IDA as IDA Pro (x64)
    participant GUI as Valorant-Dumper GUI
    participant MSB as MSBuild Compiler
    participant Game as VALORANT Process

    Dev->>IDA: Load Unpacked PE Dump (VALORANT-Win64-Shipping.exe)
    Dev->>IDA: Locate FSeamlessTravelHandler::Tick (or search "SeamlessTravel")
    Dev->>IDA: Press F5 to Decompile Decryption Function
    Dev->>GUI: Paste Pseudocode into "Extractor" Tab
    GUI-->>Dev: Extracted TableOffset & KeyOffset verified
    Dev->>GUI: Click "Apply Offsets" (Updates config & C++)
    Dev->>MSB: Click "Build Dumper DLL"
    MSB-->>GUI: Compiled new Dumper-7.dll
    Dev->>Game: Launch Game & Click "Quick Dump Game"
    Game-->>Dev: Fully Reconstructed SDK Generated!
```

### Detailed Steps:

1. **Step 1: Obtain a Clean Memory Dump**
   - Dump the unpacked `VALORANT-Win64-Shipping.exe` from protected memory using your preferred memory dumper.

2. **Step 2: Load into IDA Pro (x64)**
   - Open the dumped binary in IDA Pro x64 and let the initial auto-analysis finish (`AU: idle`).

3. **Step 3: Locate the Decryption Routine**
   - Search for string references to `"SeamlessTravel"` or navigate to `FSeamlessTravelHandler::Tick`.
   - Inspect the sub-routine responsible for accessing `FUObjectArray`.
   - Press `F5` to generate Hex-Rays C++ pseudocode.

4. **Step 4: Extract Offsets via GUI Extractor**
   - Copy the decompiled code block containing the 7-case `switch` statement and global table reference.
   - Switch to **Valorant-Dumper Pro**, open the **"Extractor"** tab, and paste the code.
   - Click **"Extract Offsets"** followed by **"Apply to Project"**.

5. **Step 5: Recompile and Dump**
   - Navigate to the **"Build"** tab and click **"Build DLL"** (or execute [`Build-Dumper-7.bat`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Build-Dumper-7.bat)).
   - Your updated [`Dumper-7.dll`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Dumper-7.dll) is now ready to inject into the new game version!

---

## 🛠️ Project Toolchain & Build Automation Scripts

| Script / Artifact | Description & Usage |
| :--- | :--- |
| **[`Valorant-Dumper.exe`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Valorant-Dumper.exe)** | Compiled, standalone desktop application packaged with all dependencies and custom branding. |
| **[`Build-Executable.bat`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Build-Executable.bat)** | One-click PyInstaller packaging script; compiles `run_gui.py` into a single standalone `.exe`, embeds `cat.ico`, copies output to root, and cleans intermediate build directories. |
| **[`Build-Dumper-7.bat`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Build-Dumper-7.bat)** | Standalone MSBuild compilation batch script; detects Visual Studio 2022, builds `Dumper-7.vcxproj`, and copies `Dumper-7.dll` to root. |
| **[`Launch-GUI.bat`](file:///c:/Users/Zero0/Downloads/Valorant-Dumper/Launch-GUI.bat)** | Smart launcher script; executes `Valorant-Dumper.exe` if present, or falls back to running `python run_gui.py`. |

---

<div align="center">
  <b>Valorant SDK Dumper Suite • Engineered with precision by yousef_zero</b>
</div>
