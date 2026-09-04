# Edge-AI-Based Distributed Fleet Coordination for Autonomous Mobile Robots

Edge-AI distributed architecture for Autonomous Mobile Robots (AMRs) enabling decentralized warehouse coordination, peer-to-peer conflict negotiation, and resilient offline operations. This project was built as a hackathon prototype, demonstrating how on-device intelligence and local communication can enable scalable, robust fleet coordination without depending on a single central server.

Table of contents
- [Vision](#vision)
- [Key features](#key-features)
- [High-level architecture](#high-level-architecture)
- [Components](#components)
- [Getting started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Quickstart (simulation)](#quickstart-simulation)
- [How it works](#how-it-works)
- [Deployment notes](#deployment-notes)
- [Development and testing](#development-and-testing)
- [Contributing](#contributing)
- [License](#license)
- [Contact & acknowledgements](#contact--acknowledgements)


## Vision

Build a resilient, scalable coordination layer for fleets of Autonomous Mobile Robots (AMRs) that minimizes cloud dependency by performing conflict resolution, task negotiation, and basic planning at the edge. The design favors decentralization, graceful degradation to offline operation, and compatibility with typical warehouse AMR hardware and networking (Wi-Fi, ad-hoc, or local mesh).

## Key features
- Decentralized task allocation and negotiation (P2P conflict resolution)
- Local safety checks and collision avoidance primitives
- Fault-tolerant behavior when connectivity to a central server is lost
- Extensible architecture for simulation, hardware-in-the-loop, and real deployments
- Lightweight on-device ML/heuristics for priority and route arbitration

## High-level architecture

1. Per-robot Edge Agent
   - Observes local sensors and state
   - Negotiates with nearby agents (P2P) to resolve path and task conflicts
   - Executes local control loops and safety primitives

2. Optional Central Coordinator
   - Provides global goals, fleet-wide monitoring, and long-term optimization
   - Not required for basic operation — agents continue in degraded mode if unreachable

3. Communication Layer
   - Local peer discovery and message exchange (UDP/TCP, MQTT over local broker, or custom mesh)
   - Message types: heartbeats, proposals, votes/agreements, status updates

4. Simulation / Visualization
   - Lightweight simulator to validate negotiation protocols and emergent behaviors
   - Visual tools to inspect agent states and interactions during experiments

## Components
- edge-agent/ — robot-side software (negotiation, local planner, safety checks)
- coordinator/ — optional central service for analytics, monitoring, and global goals
- sim/ — simulation environment to test coordination protocols without hardware
- docs/ — design notes, protocol definitions, and experiment logs

(Adjust paths if the repo layout differs.)

## Getting started

### Prerequisites
- Python 3.8+ (for simulation and tooling)
- A modern Linux/macOS environment (development tested on Ubuntu/macOS)
- Optional: Docker for containerized runs

### Quickstart (simulation)
1. Clone the repository

   git clone https://github.com/KrucibleCoder/Edge-AI-Based-Distributed-Fleet-Coordination-for-Autonomous-Mobile-Robots.git
   cd Edge-AI-Based-Distributed-Fleet-Coordination-for-Autonomous-Mobile-Robots

2. Create a virtual environment and install dependencies

   python -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt

3. Run the simulator (example)

   python sim/run_simulation.py --agents 6 --map maps/warehouse_small.json

This should spin up a local simulation showing multiple agents negotiating paths and resolving conflicts via the implemented P2P protocol.

If Docker is preferred, look for a Dockerfile in the repo or create one that runs the above startup steps.

## How it works (brief)

- Each agent periodically broadcasts a short “proposal” describing its intended trajectory or action for the next planning horizon.
- Nearby agents that detect potential conflicts evaluate proposals using a shared arbitration rule (heuristics or a trained lightweight model) and reply with acceptance, rejection, or a modified proposal.
- If a conflict is unresolved, a small deterministic tie-breaker (timestamp, agent ID, or priority score) decides the outcome to avoid livelock.
- Central coordinator (if connected) can provide higher-level reassignment or constraint updates but does not interfere with immediate local safety decisions.

## Deployment notes
- Timing and network unreliability: design message timeouts and retries conservatively; prefer idempotent messages.
- Security: in production, secure P2P channels (TLS, authentication) and sanitize any externally-supplied task/goal parameters.
- Resource constraints: tune ML/heuristic model sizes and planning horizons to fit target compute hardware (edge CPUs, low-power accelerators).

## Development and testing
- Use `sim/` to iterate quickly on negotiation logic without hardware.
- Add unit tests for edge-agent arbitration logic and integration tests for multi-agent scenarios.
- Suggested CI checks: linting, unit tests, and running a short headless simulation to catch regressions.

## Contributing
- Open an issue to discuss major changes or feature proposals.
- Fork the repo, create a feature branch, and submit a PR with descriptive commits.
- Keep changes modular: separate negotiation policy, network layer, and planner implementations where possible.

## License
Specify your desired license here (e.g., MIT, Apache-2.0). If you want, I can add a LICENSE file.

## Contact & acknowledgements
- Author: KrucibleCoder
- Built as a hackathon prototype — thanks to contributors and maintainers who helped with design and experiments.

---

If you'd like, I can:
- Add a short demo GIF or screenshot to the README
- Flesh out setup commands based on files in the repository (requirements.txt, Dockerfile, sim entrypoints)
- Add a minimal LICENSE file (MIT/Apache)

