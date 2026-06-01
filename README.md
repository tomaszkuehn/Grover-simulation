# Quantum Maze: A Realistic Grover Search Simulator

An educational browser-based simulator that visualizes a Grover-style quantum search on top of a 25×25 maze, with three distinct fidelity modes — from ideal theory to hardware-inspired shot-based statistics.

## Overview

This project demonstrates how a maze can be mapped to a simplified quantum search model. It uses:

- A 25×25 maze with one entrance and two possible exits
- A 6-qubit Grover-inspired model (64 basis states, ~3 oracle–diffuser rounds)
- Three simulation modes: **Ideal theory**, **Noisy simulation**, and **Hardware-inspired**
- Step-by-step maze overlays after each circuit stage
- Shot-based measurement histograms with uncertainty estimates

The app is designed for teaching and prototyping — it prioritizes visual intuition over physically accurate quantum simulation.

---

## Simulation Modes

The simulator offers three levels of realism, selectable via the mode switcher:

### 1. Ideal Theory
- Pure Grover algorithm with theoretical amplitude amplification
- Uses the textbook round count: `⌊π/4 × √64⌋ = 6` (3 oracle–diffuser pairs)
- Probabilities come directly from the Grover formula: `sin²((2r+1) × arcsin(1/√64))`
- 128 shots per run
- No noise, no imperfections — exposes simulator-internal probabilities

### 2. Noisy Simulation
- Retains the Grover structure but attenuates the target amplitude
- Target probability is scaled by **0.88×** with a floor of 4%
- Small probability spread (0.08% per fallback candidate) redistributes weight to nearby maze paths
- 256 shots per run
- Models decoherence-like amplitude leakage without a full noise model

### 3. Hardware-inspired
- Shot-statistics-first presentation: 1024 shots per run
- Target probability is scaled by **0.60×** with a floor of 2%
- Stronger probability spread (0.3% per fallback candidate) to nearby states
- **8% random readout flip** — a measured state can be randomly replaced by another, simulating readout assignment errors
- Designed as an approximation, not a direct hardware dashboard

### Mode Comparison

| Property | Ideal | Noisy | Hardware-inspired |
|---|---|---|---|
| Shots per run | 128 | 256 | 1024 |
| Target ampl. scaling | 1.00× | 0.88× | 0.60× |
| Fallback spread | none | 0.08%/candidate | 0.3%/candidate |
| Readout error | none | none | 8% random flip |
| Min. target prob. | theoretical | 4% | 2% |
| Histogram uncertainty | ✓ | ✓ | ✓ |

---

## Simulation vs. Real Hardware

This simulator is an **educational model**, not a quantum device emulator. Key differences:

### What the simulator does
- Runs a deterministic step-by-step animation through initialization → Hadamard → oracle–diffuser rounds → measurement
- Samples measurement outcomes from a computed probability distribution
- Redistributes probability among fallback maze-path candidates to simulate noise
- Uses a single clean 8% readout flip in hardware-inspired mode

### What real quantum hardware would do
- Execute actual gate operations subject to qubit decoherence (`T₁`, `T₂`), gate infidelity, and cross-talk
- Produce measurement results governed by the full density matrix of the physical system
- Exhibit state-dependent readout errors (not uniform across all states)
- Require error mitigation techniques (readout calibration, measurement error mitigation, dynamical decoupling)
- Show drift over time due to qubit parameter shifts and environmental coupling

### What the simulator does NOT model
- Gate-level noise channels (depolarizing, amplitude damping, phase damping)
- Qubit connectivity constraints and swap-network overhead
- State-preparation-and-measurement (SPAM) error decomposition
- `T₁` relaxation and `T₂` dephasing times
- Cross-talk between adjacent qubits
- Leakage to non-computational states

The hardware-inspired mode provides a **qualitative feel** for how noise degrades Grover search contrast — but it should not be used for quantitative hardware benchmarking.

---

## How It Works

1. **Maze encoding**: The 25×25 maze is represented as a binary grid (`o` = open, `x` = wall). Each of the 64 basis states encodes a sequence of binary junction decisions that steer a path through the maze.

2. **Exit selection**: The user picks one of two exits (top or right). Each exit maps to a specific 6-bit marked state (`011110` for the top exit, `101000` for the right).

3. **Simulation stages**:
   - **Initialization** — all amplitude starts in `|000000⟩`
   - **Hadamard layer** — uniform superposition over all 64 states
   - **Oracle 1–3 + Diffuser 1–3** — Grover iterations that phase-mark the target and amplify its amplitude
   - **Measurement** — sample one basis state per shot

4. **Maze overlay** — updates after each stage:
   - <span style="color:#9aa7b7">Gray</span> = candidate paths (fallback states)
   - <span style="color:#ff79c6">Pink</span> = oracle-marked target
   - <span style="color:#ffcc66">Gold</span> = amplified target route
   - <span style="color:#7aa2ff">Blue</span> = correctly measured output route
   - <span style="color:#ff5555">Dark red</span> = incorrectly measured output route

5. **Histogram** — shows the measurement distribution across all shots with binomial uncertainty estimates (`σ = √(p(1−p)/n)`). States measured only once are grouped under "other".

---

## Usage

### Running the simulator

1. Clone or download the repository.
2. Open `quantum-maze.html` in a modern browser (Chrome, Edge, or Firefox).
3. No build step, server, or dependencies required — it's a single self-contained HTML file.

### Recommended setup

- **Window width**: 1550 px or above for optimal layout. At narrower widths, controls may wrap and the maze overlay may be harder to read. A warning banner appears when the window is too narrow.
- **Browser**: any modern evergreen browser with SVG support.

### Controls

| Control | Action |
|---|---|
| **Exit selector** | Choose which maze exit to target (changes the marked state) |
| **Mode switcher** | Toggle between Ideal / Noisy / Hardware-inspired |
| **Start** | Begin or resume the simulation run |
| **Pause** | Pause the current run |
| **Step** | Advance one stage manually |
| **Reset** | Reset the entire run (preserves exit and mode selection) |
| **Pause on error** | When enabled, auto-pauses on an incorrect measurement so you can inspect the wrong path |
| **Speed** | Normal (180 ms/tick) or Fast (40 ms/tick) |

### Understanding the display

- **Left panel**: exit picker, mode switcher, controls, and live statistics (rounds, shots, target hit rate, success rate, uncertainty)
- **Center panel**: maze with overlaid paths, circuit diagram, and timeline
- **Right panel**: state probability ranking, measurement histogram, and mode description notes
- **Bottom**: execution log with timestamps

---

## Recent Changes

- **Title & branding**: renamed to *Quantum Maze: A Realistic Grover Search Simulator*
- **Mode renamed**: *Hardware-like* → *Hardware-inspired* (more accurate naming)
- **Probability fixes**: negative-probability guard in noisy/hardware modes, zero-sum guard in sampling
- **Readout model**: simplified hardware-inspired readout error to a single clean 8% random flip (removed the pre-normalization weight adjustment that was double-compounding)
- **Timer fixes**: eliminated zombie timers on continue-after-completion and start-after-completion edge cases
- **Performance**: lazy path computation (64 paths generated on-demand instead of eagerly at load), exit picker and mode switch no longer rebuilt every tick
- **Code quality**: deduplicated state-row DOM construction, decoupled probability computation from global UI state, added stale-call guard in `tick()`
- **UX**: Step button now logs a message when clicked during error pause instead of silently ignoring; only the selected exit's marker dot is rendered on the overlay

See `FIXES.md` for the full code review and fix log.

---

## Tech Stack

- HTML5
- CSS3 (custom properties, grid, flexbox, SVG)
- Vanilla JavaScript (no frameworks, no dependencies)
- SVG overlay rendering for maze paths

---

## Current Limitations

- Route overlays are manually fitted to the maze image; the maze is not auto-parsed from bitmap data
- The probability model is educational and scripted — not a full quantum state-vector simulation with unitary matrices
- Exit-to-state mapping is predefined in code (two exits hardcoded)
- 3-way maze junctions can only select between two moves (binary bit), never the third option at a T-junction
- Qubit count is fixed at 6 (64 states); changing it requires touching many scattered constants

---

## Roadmap

- Automatic maze path detection from bitmap images
- Clickable exits directly on the maze image
- Adjustable qubit count with derived constants
- Full unitary state-vector simulation mode
- Real quantum backend integration (IBM Qiskit, AWS Braket)
- Support for more than two exits
- Data export for measurements and shots (CSV/JSON)

---

## Project Purpose

This project is intended for:
- Quantum computing education and outreach
- Interactive demos and conference presentations
- Concept validation for visual quantum search simulators
- Developer experimentation with front-end quantum visualizations
- Understanding the gap between ideal quantum algorithms and real-world implementation

---

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
