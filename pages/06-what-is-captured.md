---
layout: two-cols-header
zoom: 0.9
---

# What Is Captured?

Every output file becomes a STAC Item with rich metadata

::left::

### Per file
<ul class="text-[0.82em] mt-1 ml-5 space-y-1">
  <li>Variable name, format (NetCDF / GRIB)</li>
  <li>Model component (echam, fesom, jsbach, …)</li>
  <li>Temporal extent, spatial bounding box</li>
  <li>File size, checksum</li>
</ul>

<div class="mt-4">

### From the run configuration
<ul class="text-[0.82em] mt-1 ml-5 space-y-1">
  <li>Full Fortran namelist (~393 parameters per item)</li>
  <li>Experiment name, experiment type (control, paleo, …)</li>
  <li>CO₂ / CH₄ / N₂O levels, output frequency</li>
  <li>Paleo time period (LGM, Eemian, Mid-Holocene, PI)</li>
</ul>

</div>

<div class="mt-4">

### HPC storage information
<ul class="text-[0.82em] mt-1 ml-5 space-y-1">
  <li>Storage tier, facility, system, last access</li>
</ul>

</div>

::right::

<div class="ml-4 text-sm">

All fields are **queryable** — filter and search across all experiments.

```
General
  Variable          ssh
  Experiment        basic-001
  Component         fesom
  Format            netcdf
  Experiment Type   control

Hpc
  Facility          AWI
  System            albedo
  Storage Tier      hot
  Last Access       2026-04-24

Nml  (393 fields)
  echam:runctl:dt_stop     [1850,4,1]
  echam:radctl:co2vmr      2.8432e-4
  fesom:paths:resultpath   /albedo/…
  …
```

</div>
