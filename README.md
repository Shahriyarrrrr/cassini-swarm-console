# Cassini Swarm Console

A multi-agent orchestration console. Seven named agents plan, build, review, **reject**, rework and finally sign off a release — and you watch the whole negotiation happen on a live message bus.

**[▶ Open the live demo](https://shahriyarrrrr.github.io/cassini-swarm-console/)**

One self-contained HTML file. No build step, no dependencies, no server.

---

## What it actually does

Most "multi-agent" demos are an animation on a timer. This one has a real scheduler underneath.

- **A dependency graph, not a script.** Every task declares what it depends on. An agent starts work only when its dependencies have *completed* and that agent is *free*. Contention, idle time and critical paths emerge from the graph rather than being choreographed.
- **Review gates that actually block.** `SABLE` reviews what `NOVA` builds. A rejection inserts a rework task, inserts a re-review that depends on the rework, and re-points every downstream task at the re-review. The release gate genuinely cannot open until the finding is closed.
- **Artifacts flow between agents.** Each completed task commits a file to a shared build store. Downstream agents read those files as their input. Click any artifact to see its contents.
- **The graph is live.** Messages travel as colour-coded packets along the edges — colour by message type, progress arcs by task completion. Click an agent to isolate its edges and filter the bus to its traffic.

---

## The roster

| Agent | Role | What it owns |
|---|---|---|
| **ATLAS** | Orchestrator | Holds the task graph, assigns work, arbitrates blockers, signs the release |
| **VERA** | Requirements | Turns a vague objective into constraints, SLOs and acceptance criteria |
| **KODA** | Architect | Topology, data contracts, failure modes |
| **NOVA** | Implementation | Services, pipelines, models — and every rework cycle |
| **SABLE** | Adversarial review | Tries to break NOVA's work; rejects what fails the bar |
| **ORION** | Release | Containers, CI, canaries, rollback guards |
| **LYRA** | Documentation | Runbooks, model cards, API references |

---

## Two engines

The console ships with two ways to drive the same seven agents, the same scheduler and the same UI.

### Scripted — works everywhere

Three hand-built missions with full dependency graphs, real artifacts and planted defects that `SABLE` catches. This is what the public demo link runs, and it needs nothing but a browser.

| Mission | Objective |
|---|---|
| `SENTRY-01` | Real-time fraud scoring service — 40 ms budget, regulated audit trail |
| `AEGIS-02` | Autonomous NOC incident response — must be structurally unable to bounce a core router |
| `LUMEN-03` | Bangla/English document extraction — where a passing average hides a failing subgroup |

### Live Claude — only inside Claude

Opened as an artifact on claude.ai, the page declares the `sample` runtime capability and every turn becomes a real model call on the viewer's own Claude account:

1. **ATLAS plans.** Your objective goes to one call that returns a dependency graph. The page validates it — unknown agents dropped, only backward edges kept so a cycle is structurally impossible, terminal sign-off guaranteed.
2. **Agents work in parallel**, two at a time, each with its own persona, the mission context and the upstream artifacts it builds on. Output streams into the bus as it is written.
3. **Agents have a real tool.** `read_artifact(path)` executes *in the page* — the model decides when to pull another agent's file in full instead of working from a summary. Tool calls appear on the bus.
4. **SABLE's verdict is the model's.** It finds the defect, names the owner, and the scheduler reshapes the graph around it. Capped at two rounds, then recorded as a known gap.

> **Note:** Live mode requires `window.claude`, which only exists inside the Claude app. On GitHub Pages — or any public host — the toggle disables itself and says so. Nothing is faked to cover the gap.

---

## Run it

```bash
git clone https://github.com/Shahriyarrrrr/cassini-swarm-console.git
cd cassini-swarm-console
python3 -m http.server 8000
# open http://localhost:8000
```

Or just open `index.html` in a browser.

---

## Built with

Vanilla HTML, CSS and JavaScript. Canvas 2D for the swarm graph. No framework, no bundler, no `node_modules`. The whole thing is one file so it can be dropped on any static host.

Responsive down to phone width, light and dark themes, keyboard-accessible controls, and `prefers-reduced-motion` respected.

---

Built by [Shahriyar Rahman Simoon](https://github.com/Shahriyarrrrr) · [portfolio](https://shahriyarrahmansimoon.netlify.app) · [linkedin](https://linkedin.com/in/shahriyar101)
