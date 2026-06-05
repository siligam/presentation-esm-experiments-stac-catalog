---
layout: two-cols-header
zoom: 0.9
---

# STAC Hierarchy

How experiments are organized in the catalog

::left::

### Catalog (DuckDB file)
<ul class="text-[0.82em] mt-1 ml-5 space-y-1">
  <li>One or more experiments per file</li>
  <li>New experiment runs produce one file each; federated together on the server</li>
  <li>Entry point for discovery</li>
</ul>

<div class="mt-4">

### Collection
<ul class="text-[0.82em] mt-1 ml-5 space-y-1">
  <li>One per experiment — groups all model output for that run</li>
  <li>Carries experiment-level metadata (temporal extent, spatial extent, components)</li>
</ul>

</div>

<div class="mt-4">

### Item
<ul class="text-[0.82em] mt-1 ml-5 space-y-1">
  <li>One per output file (NetCDF / GRIB)</li>
  <li>Links to the actual file on disk</li>
  <li>Carries per-file metadata: variable, time, namelist parameters, storage tier</li>
</ul>

</div>

::right::

<div class="ml-4">

```
all-experiments-v2.duckdb   ← Catalog (DuckDB file)
├── basic-001          ← Collection (experiment)
│   ├── heat_atm.oasis3mct.nc   ← Item
│   ├── prec_atm.oasis3mct.nc   ← Item
│   └── ...            (116 items)
├── basic-002          ← Collection (experiment)
├── garfield-001       ← Collection (experiment)
└── sylvester-003      ← Collection (experiment)
    └── ...
```

<div class="mt-6 p-3 bg-blue-50 dark:bg-blue-900 rounded text-sm">
Each collection shows its model components as badges:<br>
<b>echam · fesom · jsbach · oasis3mct</b>
</div>

</div>
