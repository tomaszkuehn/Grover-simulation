# Software Requirements Specification
## Project Title
Quantum Maze Search Simulator

## 1. Purpose
The purpose of this application is to provide an educational, browser-based visualization of Grover-style quantum search using a user-supplied maze image. The application maps a maze with one start point and two selectable exits into a 6-qubit search model and visualizes the search process step by step.

## 2. Scope
The system is a single-page web application intended for demonstrations, teaching, and prototype visualization of quantum search concepts. It is not intended to solve arbitrary maze images automatically with a physically accurate quantum backend.

## 3. Product Overview
The application displays a maze image annotated by the user’s conventions:
- Red arrow = entrance / start point
- Green arrow 1 = exit option A
- Green arrow 2 = exit option B

The simulator assumes that each valid route to an exit contains six decision points or junctions. Therefore, the search space is represented by 6 qubits, giving 64 basis states.

## 4. Goals
- Visualize Grover-inspired search on top of a maze image.
- Allow the user to choose one of two exits before simulation starts.
- Show route overlays after each iteration.
- Provide a teaching-oriented explanation of initialization, superposition, oracle marking, diffusion, and measurement.

## 5. Users
- Software developers building educational quantum demos.
- Students learning Grover’s algorithm.
- Instructors presenting quantum search concepts.
- Technical stakeholders evaluating interactive visualization concepts.

## 6. Functional Requirements

### FR-1 Maze Display
The system shall display the uploaded maze image directly in the application UI.
The system shall embed the image in the application to avoid broken external file references in preview environments.

### FR-2 Exit Selection
The system shall provide two selectable exits before simulation starts.
The system shall associate each exit with a predefined target basis state.

### FR-3 Quantum Model
The system shall represent the search space as 6 qubits.
The system shall generate 64 basis states from `000000` to `111111`.
The system shall treat the selected exit as a single marked state for the oracle.

### FR-4 Simulation Controls
The system shall provide Start, Pause, Step, and Reset controls.
The system shall allow the user to advance the simulation one stage at a time.

### FR-5 Simulation Stages
The system shall visualize the following stages:
1. Initialization
2. Hadamard / superposition
3. Oracle
4. Diffusion iteration 1
5. Diffusion iteration 2
6. Measurement

### FR-6 Route Overlay
The system shall render route overlays on top of the maze image after every stage.
The system shall show:
- Gray dashed lines for candidate paths
- Pink line for the oracle-selected target path
- Gold line for the amplified target path
- Blue line for the measured output path

### FR-7 State Visualization
The system shall display a ranked list of basis states and their current probabilities.
The system shall visually highlight the selected target state.
The system shall visually highlight the measured state after measurement.

### FR-8 Histogram
The system shall maintain a histogram of measured outputs across repeated shots.
The system shall update the histogram after each completed measurement event.

### FR-9 Circuit View
The system shall display a six-wire circuit-like diagram.
The system shall highlight the currently active stage in the circuit view.

### FR-10 Execution Log
The system shall maintain a log panel describing simulation progress, including shot number, stage transitions, and measurement results.

## 7. Non-Functional Requirements

### NFR-1 Platform
The application shall run in a modern web browser without server-side dependencies for the base demo.

### NFR-2 Portability
The application shall be delivered as a self-contained HTML artifact for easy local execution.

### NFR-3 Performance
The UI shall remain responsive during animation and step transitions on a typical desktop browser.

### NFR-4 Usability
The application shall be understandable by non-expert users through labels, legends, and stage descriptions.

### NFR-5 Maintainability
Route definitions, target states, stage descriptions, and rendering logic shall be separable in the code for future replacement with a true path extraction module or quantum backend.

## 8. Technical Design Constraints
- Current route overlays are manually fitted to the maze image.
- The probability model is scripted for visualization rather than computed from a full state-vector simulation.
- The current architecture is front-end only.
- No automatic maze parsing is required in the current version.

## 9. Future Enhancements
- Automatic maze path extraction from the uploaded image
- Clickable exit markers directly on the maze
- Real state-vector simulation using Qiskit, Q#, or another quantum framework
- Export of simulation states and measurements
- Support for configurable qubit counts and arbitrary maze topologies

## 10. Acceptance Criteria
- The application loads the correct maze image.
- The user can choose between two exits before starting.
- The simulator uses a 6-qubit, 64-state model.
- Route overlays update after each stage.
- The histogram updates after measurements.
- The application can run locally as a single HTML file.