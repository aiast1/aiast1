![A process plant built by AcaysiaGEM: vessels, columns, routed piping and pipe racks](https://acaysia.com/assets/images/engines/gem-plant-metal.jpg)

<sub>Not a drawing of a plant. This is the geometry our solver actually runs in, rendered straight from the model. Twelve units, 678,374 triangles, about 6 seconds.</sub>

### Hi, I'm Aias

I build the GPU layer under industrial process control. The short version: chemical plants are run by controllers that were designed in the 1940s, and GPUs are now fast enough to simulate thousands of possible futures every control tick and steer toward the best one. That's the whole idea.

I'm a cofounder at **[Acaysia](https://acaysia.com)**, where I work on the engines underneath that:

* **[AcaysiaRT](https://acaysia.com/engines/acaysia-rt)** is the GPU runtime. 127 process models, from a pH-CSTR to a 168-state chromatography column, all on one templated CUDA core.
* **Rete** wires those models into whole plants, recycle loops and all, because a plant is a loop rather than a pile of unit operations.
* **[AcaysiaDRT](https://acaysia.com/engines/acaysia-drt)** lifts it into 3D with lattice Boltzmann.
* **[AcaysiaGEM](https://acaysia.com/engines/acaysia-gem)** builds the vessel shapes. That's the picture up top.
* **[AcaysiaCORE](https://acaysia.com/core)** puts all four in a browser. Still in build.

My favourite number from it: one RTX 5080 steps about **6.1 billion reactor-steps per second**, roughly 182× the PyTorch reference. (10,000 rollouts, horizon 100, sm_120. A throughput figure without its conditions isn't a claim.)

Most of that lives in private repos, so here's what doesn't.

### Things you can actually click

**[aios](https://github.com/aiast1/aios)** is an operating system where the graph is the computer. Programs are written in Ferrite, a declarative language that compiles to a content-addressed DAG instead of machine code, and the runtime skips whatever didn't change. Rust engine via PyO3, custom CUDA backend, 752 tests. It beats PyTorch by 1.60× on a fully-dirty MLP step and 2.19× when layers are frozen, and it runs a Doom raycaster at 1900+ FPS as a 26-node graph.

**[csproject](https://github.com/aiast1/csproject)** is MPPI pointed at music instead of a reactor. Give it a soprano line and it harmonizes all four voices by treating voice leading as trajectory optimization: sample a lot of possible futures, score them against music theory, commit to the best first step. Same controller as the day job, much better demo.

### Elsewhere

[acaysia.com](https://acaysia.com) &nbsp;·&nbsp; [@aiastatsis](https://x.com/aiastatsis) &nbsp;·&nbsp; [linkedin](https://linkedin.com/in/aias-tatsis)

<sub>CUDA · C++ · Rust · Python · lattice Boltzmann · MPPI · OPC UA / EtherNet/IP</sub>
