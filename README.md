# Quantum Maze Search Simulator

An educational browser-based simulator that visualizes a Grover-style quantum search on top of a user-provided maze image.

## Overview

This project demonstrates how a maze can be mapped to a simplified quantum search model. The current version uses:
- One entrance marked by a red arrow
- Two exits marked by green arrows
- A 6-qubit model, based on six junction decisions per route
- 64 basis states
- Step-by-step route overlays after each simulation stage

The app is designed for teaching and prototyping, not for physically accurate quantum maze solving.

## Features

- Embedded maze image display
- Two exit choices before simulation start
- Start, pause, step, and reset controls
- Six-qubit circuit-style visualization
- Per-stage route overlays on the maze
- State ranking and probability view
- Measurement histogram across repeated shots
- Execution log for stage transitions and results

## How it works

The simulator models the maze as a Grover-style search problem with one marked target state at a time. After the user selects an exit, the application:
1. Initializes the system
2. Applies Hadamard-style superposition
3. Marks the selected route with an oracle
4. Applies diffusion-style amplification
5. Measures the result across multiple shots

The maze overlay updates after every stage:
- Gray = candidate paths
- Pink = oracle-marked target
- Gold = amplified target route
- Blue = measured output route

## Tech stack

- HTML
- CSS
- Vanilla JavaScript
- SVG overlay rendering

## Running locally

1. Save the project as an `.html` file.
2. Open it in a modern browser such as Chrome, Edge, or Firefox.

No build step is required for the current self-contained prototype.

## Current limitations

- Route overlays are manually fitted to the maze image.
- The maze is not parsed automatically from bitmap data.
- The probability model is educational and scripted, not a full quantum state-vector simulation.
- Exit-to-state mapping is predefined in code.

## Roadmap

- Automatic maze path detection
- Clickable exits directly on the image
- Real quantum backend integration
- Adjustable qubit count
- Support for more than two exits
- Data export for measurements and shots

## Project purpose

This project is intended for:
- Quantum computing education
- Interactive demos
- Concept validation for visual search simulators
- Developer experimentation with front-end quantum visualizations

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
