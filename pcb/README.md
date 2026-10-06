# PCB

Design tool: **Autodesk Fusion** (Electronics / PCB workspace).

Fusion keeps the live design in its cloud project, so this folder holds **exports and
snapshots** of it, committed at meaningful milestones. The commit history here is the
record of how the design evolved.

| Folder | Contents |
|--------|----------|
| `schematics/` | Schematic PDF exports and Fusion design archives (`.f3z`) per milestone |
| `layout/` | Board layout exports (PDF/PNG views, board archives) per milestone |
| `libraries/symbols/` | Custom symbol / library exports |
| `libraries/footprints/` | Custom footprints / package exports |
| `libraries/3d-models/` | 3D models for parts |
| `simulations/` | Simulation files and results |
| `outputs/` | Renders, screenshots, and other generated views (not manufacturing files) |

## Suggested snapshot naming

`<what>_rev<letter>_<YYYY-MM-DD>.<ext>`, e.g. `schematic_revA_2026-10-20.pdf`.

Fabrication outputs (Gerbers, drill, pick-and-place) go in [`../manufacturing/`](../manufacturing/).
