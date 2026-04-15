---
theme: ./slidev-theme-cscs
---

## User Documentation at CSCS

Ben Cumming

eth-cscs.github.io/pead26

PEAD at CuG 2026

---
layout: two-cols
layoutClass: gap-2
---

# uenv documentation

---
layout: two-cols
layoutClass: gap-2
---

# Environments are files

# Describing uenv

uenv have a label: `name/version:tag@system%uarch`:
* `name`: the name, e.g. `prgenv-gnu`, `pytorch` or `gromacs`;
* `version`: version or release date, e.g. `2025.1` or `4.3`;
* `tag`: for releases of the same version (bug-fixes and small improvements);
* `system`: the cluster, e.g. `daint`, `eiger`, `santis`, etc;
* `uarch`: the node architecture:
    * `gh200`: clariden, daint, santis;
    * `zen2`: eiger, bristen;
    * `a100`: bristen, balfrin.

uenv commands accept full or partial labels. The following are valid in different contexts:

`prgenv-gnu/24.11:v1@daint`, `@*`, `:v1`, `namd`, `prgenv-gnu%gh200`

---

# Alps

<div class="flex justify-center">
    <img src="./images/alps-blah.png" class="h-45" alt="Alt text for the image">
</div>


