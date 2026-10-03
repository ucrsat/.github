## HSP @ UCR's cubesat & payload team

this organization is home to the payload team of the highlander space program (HSP) at the university of california, riverside.
all ongoing and future projects (as well as select archived projects) are version controlled in the repositories within this organization.

## structure

repositories here track electronics hardware, firmware, software, and the simulations that support them.
each repository is one **system** of the larger spacecraft or payload (for example, the cubesat's ADCS). a system can contain
several **projects**: one per board, firmware target, or analysis. a multi-board ADCS assembly, for example, has several kicad
projects, its own firmware, and simulations.

```
cubesat-<system>/                     one repository per system, e.g. cubesat-adcs
├── README.md                         system overview, block diagram, list of projects, status
├── docs/                             system-level requirements, ICDs, interface pinouts, reviews
└── <project>/                        one folder per board / firmware target / analysis
    ├── README.md                     purpose, revision history, bring-up notes, known issues
    ├── pcb/
    │   ├── <project>.kicad_pro
    │   ├── <project>.kicad_sch       root schematic
    │   ├── <project>.kicad_pcb
    │   ├── sch/                      hierarchical sub-sheets
    │   └── fab/                      released outputs (gerbers, drill, BOM, pick-and-place), per revision
    ├── fw/
    │   ├── src/
    │   ├── include/
    │   └── README.md                 toolchain, build and flash instructions
    ├── sw/                           ground / test / host-side software
    ├── sim/                          SPICE, thermal, structural, orbit / attitude simulations
    └── test/                         test procedures, bench data, results
```

not every project needs every folder. leave out what doesn't apply.

## repositories

| repository | description |
|---|---|
| [kicad-lib](https://github.com/ucrsat/kicad-lib) | shared kicad 10 symbol and footprint libraries, plus the `ucrsat-pcba` kicad project template |
| [.github](https://github.com/ucrsat/.github) | this profile and organization-wide defaults |
| cutie-* | archived repositories tracking 2025-2026 cubesat project, not to be used as reference |

## getting started

1. ask an officer to add you to the organization.
2. install **kicad 10.x**, git, and python 3. everyone must use the same kicad major version, because files saved in a newer version can't be opened in an older one.
3. clone [kicad-lib](https://github.com/ucrsat/kicad-lib) and run `python3 tools/setup.py` (see its README).
4. start new boards from **file → new project from template → ucrsat-pcba**.

## conventions

- **naming:** repositories, folders, kicad projects, and libraries are lowercase and hyphenated (`cubesat-adcs`, `sun-sensor-board`, `ucrsat-power`).
- **units:** metric throughout: mm for pcb and mechanical, SI for everything else.
- **parts:** discretes (R, C, L, diodes, generic transistors) use stock kicad symbols, with `MPN` and `Manufacturer` filled in per part. every IC, connector, and module comes from [kicad-lib](https://github.com/ucrsat/kicad-lib) and is never drawn inside a project. need a new part? open a PR there.
- **outputs:** don't commit generated files (kicad backups, `*.kicad_prl`, `fp-info-cache`, build artifacts). released fabrication outputs go in `pcb/fab/<revision>/`.

## workflow

- `main` is always the reviewed, buildable state. do work on a branch (`<name>/<short-description>` or `add-<part>`).
- open a pull request into `main`. at least **one other member** reviews it before merge.
- hardware reviews check the schematic against datasheets, run ERC/DRC with no unexplained violations, and confirm every part has an MPN.
- tag board releases sent to fab as `<project>-rev<X>` (e.g. `sun-sensor-board-revA`) so every manufactured board traces back to a commit.
