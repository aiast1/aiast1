I build the GPU layer under industrial process control: physics engines fast enough to sample thousands of futures per control tick, and the control system that spends that budget.

Cofounder and technical lead at **[Acaysia](https://acaysia.com)**, backed by a16z Speedrun. EE at Harvard. From Samos, Greece.

---

### What I'm building

**[AcaysiaRT](https://acaysia.com/engines/acaysia-rt)**, a high-throughput GPU simulation runtime for ensemble prediction. One templated CUDA core (`template<class Model>`) serves 127 registered process models, from a pH-CSTR to a 168-state chromatography column, each one bit-exact parity-tested against a PyTorch reference.

| backend | 10,000 rollouts × H=100 | throughput |
| --- | --- | --- |
| PyTorch (CPU, reference only) | 29.85 ms | ~33 M reactor-steps/sec |
| CUDA (RTX 5080, Blackwell sm_120) | 0.164 ms | ~6.1 B reactor-steps/sec |

A throughput figure without its batch size, horizon and GPU is not a claim, so those are attached. 2,650 tests behind it: per-model CUDA/PyTorch parity, physics invariants, and closed-loop MPPI convergence for every controllable model.

**Rete**, the flowsheet layer. A plant is a loop, not a pile of unit operations, and every recycle stream changes the duty of everything upstream. Rete marches the whole thing at once, so one MPPI sample is one whole-plant rollout. Fusing a plant into a single kernel is worth roughly 22× over running the same units one at a time on the same card.

**[AcaysiaDRT](https://acaysia.com/engines/acaysia-drt)**, which lifts RT into 3D. D3Q19 lattice Boltzmann with BGK collision, Strang splitting for transport-reaction coupling, structure-of-arrays layout, multi-GPU domain decomposition.

**[AcaysiaGEM](https://acaysia.com/engines/acaysia-gem)**, the geometry authority. A spatial solve is only as good as the shape you run it in, so GEM authors the vessel's *interior* parametrically and tags every boundary face at the moment it builds it. An untagged face, a stranded fluid pocket or an internal poking through the wall fails on the desk, rather than after a night of GPU time on a domain that leaked.

**[AcaysiaCORE](https://acaysia.com/core)**, in build: all four engines behind one browser application, for people who want to run these models rather than build them.

Most of that is private. Below is what isn't.

---

### Public work

**[aios](https://github.com/aiast1/aios)**, an operating system where the graph is the computer. Programs are written in Ferrite, a declarative language that compiles to a content-addressed DAG instead of machine code, and the runtime does hash-based incremental rebuild: it knows what changed, what depends on it, and what can be skipped. Rust execution engine via PyO3, custom CUDA backend (NVRTC + cuBLAS, torch-free at runtime), CUDA Graphs collapsing per-kernel dispatch into a single ~3 µs replay.

* **1.60×** PyTorch on a 2-layer MLP where every node is dirty every step, the worst case, where caching buys nothing
* **2.19×** on a 4-layer MLP with two layers frozen, where dirty detection skips them outright
* loss parity under 1e-6 on both, and 1900+ FPS on a Doom raycaster running as a 26-node DAG
* 752 tests

**[csproject](https://github.com/aiast1/csproject)**, MPPI pointed at something that is not a reactor. It takes a soprano line and harmonizes it in four voices by treating voice leading as trajectory optimization: sample many possible futures, score them against encoded music theory, commit to the best first step, advance. Same controller shape as the day job, considerably better demo.

---

### Stack

CUDA, C++, Rust, Python, lattice Boltzmann, MPPI, OPC UA / EtherNet/IP

[acaysia.com](https://acaysia.com) &nbsp;·&nbsp; [@aiastatsis](https://x.com/aiastatsis) &nbsp;·&nbsp; [linkedin](https://linkedin.com/in/aias-tatsis)
