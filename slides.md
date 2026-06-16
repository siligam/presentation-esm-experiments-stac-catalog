---
theme: seriph
background: https://cover.sli.dev
title: Cataloguing ESM Experiments with STAC
author: Pavan Siligam
info: |
  ## Cataloguing ESM Experiments with STAC
  Slides: https://siligam.github.io/presentation-esm-experiments-stac-catalog/
  Source: https://github.com/siligam/presentation-esm-experiments-stac-catalog
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Cataloguing ESM Experiments with STAC

Turning sprawling Earth System Model output into a single, searchable, cloud-native catalog

<div class="mt-4 text-sm opacity-60">Pavan Siligam · June 2026</div>

<div class="pt-10">
  <span @click="$slidev.nav.nextSlide" class="cursor-pointer">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-bl m-6 text-left text-white/60">
  <div class="flex items-center gap-1 text-xs">
    <carbon:logo-github class="text-base" />
    <span>siligam</span>
  </div>
</div>

---

# Why a Catalog?

<p class="opacity-60 text-sm mb-6">A question that should take seconds.</p>

<div class="border border-indigo-300/30 rounded-xl px-5 py-3 bg-indigo-500/5 text-sm italic mb-6">
  "Which of our runs used CO₂ at 280 ppm with ocean coupling enabled?"
</div>

<p class="text-sm font-semibold mb-3">Without a catalog, answering this means:</p>

<div class="grid grid-cols-4 gap-3 text-sm">
  <ItemCard size="sm">
    <template #stat>1 · Ask around</template>
    Find whoever configured the run — if they're still on the team
  </ItemCard>
  <ItemCard size="sm">
    <template #stat>2 · Search directories</template>
    Grep through output paths across HPC systems
  </ItemCard>
  <ItemCard size="sm">
    <template #stat>3 · Read namelists</template>
    Open config files one by one to check parameters manually
  </ItemCard>
  <ItemCard size="sm">
    <template #stat>4 · Maybe an answer</template>
    Minutes or days later — if someone remembers
  </ItemCard>
</div>

<div class="mt-6 rounded-xl px-5 py-3 text-sm" style="border: 1px solid rgba(134,239,172,0.4); background: rgba(34,197,94,0.07);">
  With a catalog, every run's configuration is recorded at run time — that question becomes a single query, answered in seconds.
</div>

---

# Our Catalog Requirements

- **One entry point** — existing and planned experiments runs. A unified view.
- **Multi-site** — ingest runs from any HPC center without manual merging
- **Query & compare** — filter across experiments and understand their differences
- **Metadata-first** — discovery is the goal; data access is optional

---

# Choosing a Catalog Format
Specific for our Use Case

<div class="grid grid-cols-2 gap-6 my-6">
  <ItemCard title="intake-esm" variant="neutral" :inline-title="true">
    <template #stat>—</template>
    <ul>
      <li>Python library, not a queryable service</li>
      <li>Schema tied to the Pangeo ecosystem</li>
      <li>No built-in federation across sites</li>
      <li>No temporal filtering built in</li>
    </ul>
  </ItemCard>
  <ItemCard title="STAC catalog" variant="success" :inline-title="true">
    <template #stat>✓</template>
    <ul>
      <li>Open standard</li>
      <li>Language-agnostic REST API</li>
      <li>Built-in spatial &amp; temporal queries</li>
      <li>Federates across sites naturally</li>
    </ul>
  </ItemCard>
</div>

STAC meets every requirement without locking us into a single language or workflow.

---
layout: two-cols
---

# STAC Core Concepts

A small, JSON spec

::left::

**STAC** — the SpatioTemporal Asset Catalog — started as a spec for satellite imagery, but its JSON object model maps to any file that has a time span and a spatial extent. No database lock-in, no bespoke schema.

**STAC API**: OGC-compliant search over space, time and arbitrary properties.

::right::

<div class="grid grid-cols-1 gap-3 my-2">
    <ItemCard size="sm">
        <template #stat>📂 Catalog</template>
        Root container — links to Collections or other Catalogs
    </ItemCard>
    <ItemCard size="sm">
        <template #stat>🗂️ Collection</template>
        A group of Items sharing extent, temporal range &amp; license — maps to one ESM experiment run
    </ItemCard>
    <ItemCard size="sm">
        <template #stat>📄 Item</template>
        A GeoJSON feature with datetime &amp; properties — maps to one variable
    </ItemCard>
    <ItemCard size="sm">
        <template #stat>💾 Asset</template>
        The actual file (NetCDF, Zarr, GRIB) — can live anywhere: tape, POSIX, object store
    </ItemCard>
</div>

---

# Mapping ESM → STAC

<div class="grid grid-cols-4 gap-3 mt-4">
  <ItemCard size="sm" title="experiment">
    <template #stat>Collection</template>
    basic-001, garfield-001, sylvester-003…
  </ItemCard>
  <ItemCard size="sm" title="variable × component">
    <template #stat>Item</template>
    heat_atm / oasis3mct — with spatial &amp; temporal extent
  </ItemCard>
  <ItemCard size="sm" title="netcdf file">
    <template #stat>Asset</template>
    href + <code>application/x-netcdf</code> + roles
  </ItemCard>
  <ItemCard size="sm" title="run metadata">
    <template #stat>Properties</template>
    experiment_type, hpc:*, paleo:*, nml:* — 394 namelist fields per item
  </ItemCard>
</div>

<div class="grid grid-cols-5 gap-3 mt-3">
  <ItemCard size="sm" class="col-span-2">
    <template #stat>JSON → DuckDB</template>
    Embedded columnar engine — no server, fast analytics without a database cluster
  </ItemCard>
  <div class="col-span-3 border border-indigo-300/30 rounded-xl p-4 font-mono text-xs leading-relaxed bg-indigo-500/5">
    <span class="font-semibold">all-experiments.duckdb</span>  ← Catalog (DuckDB file)<br>
    ├── basic-001              ← Collection (experiment)<br>
    │&nbsp;&nbsp;&nbsp;├── heat_atm.oasis3mct.nc  ← Item<br>
    │&nbsp;&nbsp;&nbsp;├── prec_atm.oasis3mct.nc  ← Item<br>
    │&nbsp;&nbsp;&nbsp;└── … (116 items)<br>
    ├── basic-002              ← Collection (experiment)<br>
    ├── garfield-001           ← Collection (experiment)<br>
    └── sylvester-003          ← Collection (experiment)
  </div>
</div>

---

# Building the Catalog

`esm-catalog` — part of ESM-Tools — creates and serves the catalog.

Two ways to populate it

<div class="grid grid-cols-2 gap-8 mt-4">

<div>

### Automatic — during the model run

<ul class="text-sm mt-2">
  <li>Built automatically during the <strong>tidy phase</strong> of esm-tools</li>
  <li>No extra steps needed if you run experiments with esm-tools</li>
  <li>Rich metadata (namelist, HPC paths, spatial/temporal extent) extracted automatically</li>
</ul>

</div>

<div>

### Manual — scan an existing output directory

<div class="text-sm mt-2">

```bash
esm-catalog scan /work/user/experiments/picontrol/outdata/ \
    --db ~/picontrol/catalog.duckdb
```

The experiment name and model component are resolved automatically from the path:

```
…/experiments/{experiment}/outdata/{component}/file.nc
```

</div>

</div>

</div>

<div class="absolute bottom-8 left-8 right-8 border border-indigo-300/30 rounded-xl px-4 py-3 text-xs bg-indigo-500/5">
  For large experiments on Levante, a SLURM batch workflow is available:
  <code>esm-catalog scan-batch</code> → parallel Parquet files → <code>merge-parquet</code> → single DuckDB
</div>

---

# STAC Item - What Is Captured?

Every output file becomes a STAC Item with rich metadata.

<div class="grid grid-cols-3 gap-6 mt-4">

<ItemCard title="output files">
  <template #stat>Per file</template>
  <ul>
    <li>Variable name, format (NetCDF / GRIB)</li>
    <li>Model component (echam, fesom, jsbach, …)</li>
    <li>Temporal extent, spatial bounding box</li>
    <li>File size, grid dimensions</li>
  </ul>
</ItemCard>

<ItemCard title="experiment settings">
  <template #stat>Run configuration</template>
  <ul>
    <li>Full Fortran namelist (~394 parameters per item)</li>
    <li>Experiment name, experiment type (control, paleo, …)</li>
    <li>CO₂ / CH₄ / N₂O levels, output frequency</li>
    <li>Paleo age in years before present (<code>paleo:years_bp</code>)</li>
  </ul>
</ItemCard>

<ItemCard title="infrastructure">
  <template #stat>HPC storage</template>
  <ul>
    <li>Storage tier, facility, system, last access</li>
  </ul>
</ItemCard>

</div>

<p class="mt-6 text-sm opacity-60">Every captured field is queryable — together they unlock fine-grained search across the entire experiment archive.</p>

---

# Search UI

<div class="grid grid-cols-2 gap-6 mt-2 items-start">

  <div>
    <p class="text-xs font-semibold uppercase tracking-widest opacity-50 mb-2">Quick Filters</p>
    <img src="/search-quick-filters.png" class="rounded-xl border w-full h-80 object-cover object-top" />
  </div>

  <div>
    <p class="text-xs font-semibold uppercase tracking-widest opacity-50 mb-2">Additional Filters <span class="text-indigo-400 ml-1">394 properties</span></p>
    <img src="/search-additional-filters.png" class="rounded-xl border w-full h-80 object-cover object-top" />
  </div>

</div>

---

# Searching the Catalog

<p class="opacity-60 text-sm mb-4">The same filters drive two complementary search modes — run both in a single query.</p>

<div class="grid grid-cols-2 gap-8">

  <ItemCard title="search for experiments">
    <template #stat>Collections</template>
    <ul>
      <li>Which experiment runs match my criteria?</li>
      <li>Returns a ranked list of runs</li>
      <li>Compare configurations side by side</li>
    </ul>
    <div class="flex flex-wrap gap-1.5 mt-3">
      <span class="border rounded-full px-2.5 py-0.5 text-xs opacity-50">experiment_type = paleo</span>
      <span class="border rounded-full px-2.5 py-0.5 text-xs opacity-50">nml:co2vmr &lt; 3e-4</span>
    </div>
  </ItemCard>

  <ItemCard title="search for items">
    <template #stat>Output files</template>
    <ul>
      <li>Which files match my criteria?</li>
      <li>Returns individual NetCDF / GRIB assets</li>
      <li>Ready to open or stage for transfer</li>
    </ul>
    <div class="flex flex-wrap gap-1.5 mt-3">
      <span class="border rounded-full px-2.5 py-0.5 text-xs opacity-50">variable = ssh</span>
      <span class="border rounded-full px-2.5 py-0.5 text-xs opacity-50">component = fesom</span>
      <span class="border rounded-full px-2.5 py-0.5 text-xs opacity-50">storage_tier = hot</span>
    </div>
  </ItemCard>

</div>

---

# Comparing Experiments

Select two or more experiments and click **Compare** — a side-by-side diff of all ~394 namelist parameters, filtered to just the differences.

<div class="grid grid-cols-5 gap-4 mt-4">
  <div class="col-span-2">
    <p class="text-xs font-semibold uppercase tracking-widest opacity-50 mb-2">Select experiments</p>
    <img src="/compare_experiments.png" class="rounded-xl border w-full h-48 object-contain object-top" />
  </div>
  <div class="col-span-3">
    <p class="text-xs font-semibold uppercase tracking-widest opacity-50 mb-2">Collection Comparison</p>
    <img src="/ui-compare-diffs.png" class="rounded-xl border w-full h-56 object-cover object-top" />
  </div>
</div>

<p class="mt-4 text-sm opacity-50 italic">"What changed between these two runs?" — answered without opening any config files.</p>

---

# Federation

Each HPC centre produces its own `catalog.duckdb`. Federation merges them under a single discovery URL — no coordination needed between sites.

<div class="grid grid-cols-2 gap-8 mt-4">
<div>

<ul class="mt-2 space-y-2 text-sm">
  <li>Service starts with one or more <code>.duckdb</code> files</li>
  <li>New catalogs registered <strong>live via the admin UI</strong> — no restart required</li>
  <li>All registered catalogs queried as <strong>one unified endpoint</strong></li>
</ul>

<div class="mt-4 border border-indigo-300/30 rounded-xl px-4 py-3 text-xs bg-indigo-500/5">
  Each centre (DKRZ, AWI, ECMWF …) pushes its own catalog — anyone with the URL can browse and search the whole archive.
</div>

</div>
<div>

<img src="/federation-admin-ui.png" class="rounded-xl border w-full h-72 object-cover object-top" />

</div>
</div>

---

# Access Tiers

<p class="opacity-60 text-sm mb-6">Federation enables the community tier. Three levels of sharing — two are available today.</p>

<div class="grid grid-cols-3 gap-5">

<ItemCard title="private" variant="success">
  <template #stat>Your own catalog</template>
  <p class="text-sm mt-1">Load any <code>.duckdb</code> file locally. Only you can see it — no server, no credentials, no coordination.</p>
  <p class="text-xs opacity-50 mt-3 pt-3 border-t border-current/10"><strong>My Collections</strong> — star any experiment to bookmark it across sessions.</p>
</ItemCard>

<ItemCard title="group" variant="neutral">
  <template #stat>Shared within a team</template>
  <p class="text-sm mt-1 opacity-60">Access control via LDAP group membership — site-specific integration with each HPC centre's directory service.</p>
  <p class="text-xs opacity-40 mt-3 pt-3 border-t border-current/10">Not yet implemented.</p>
</ItemCard>

<ItemCard title="community" variant="success">
  <template #stat>Central discovery</template>
  <p class="text-sm mt-1">A shared <code>esm-catalog serve</code> instance hosting federated catalogs — anyone with the URL can browse and search.</p>
  <p class="text-xs opacity-50 mt-3 pt-3 border-t border-current/10">AWI central deployment planned; usable today for any group with a shared server.</p>
</ItemCard>

</div>

<p class="mt-5 text-xs opacity-40">Early prototype — private and community tiers are functional; group-level sharing is the next milestone.</p>

---

# HPC Storage Tier

<p class="opacity-60 text-sm mb-4">Every item captures storage tier, facility, system, and last access — filter <em>before</em> you load to avoid triggering a tape recall.</p>

<div class="grid grid-cols-2 gap-8 mt-2">

<div>

| Storage Tier | Filesystem | State |
|---|---|---|
| **hot** | Lustre / GPFS | online |
| **hpss** | Tape archive | offline |
| **remote** | S3 / object store | online |

<ul class="text-sm mt-4 space-y-1 opacity-70">
  <li><strong>Facility</strong> — AWI or DKRZ</li>
  <li><strong>System</strong> — albedo, levante</li>
  <li><strong>Last Access</strong> — when the file was last read</li>
</ul>

</div>

<div class="border border-indigo-300/30 rounded-xl px-5 py-4 bg-indigo-500/5 text-sm flex flex-col justify-center">
  <p class="font-semibold mb-2">💡 Avoid the wait</p>
  <p>Once you find a file via search, check the storage tier. If <code>hpss</code>, the file is on tape — an HSM recall can take several minutes.</p>
  <p class="mt-3">Filter by <code>storage_tier = hot</code> upfront to work only with immediately accessible data.</p>
</div>

</div>

---
class: text-center
---

# What's Next?

<div class="flex flex-col items-center justify-center gap-6 mt-8">

<div class="grid grid-cols-2 gap-6 w-full max-w-2xl text-left">

<ItemCard title="available now" variant="success">
  <template #stat>Live on AWI systems</template>
  <ul class="mt-1">
    <li>Local catalog &amp; scan on Albedo / Levante</li>
    <li>Search, compare, storage-tier filtering</li>
    <li>STAC Browser UI</li>
  </ul>
</ItemCard>

<ItemCard title="in progress">
  <template #stat>ESM-Tools+ 2026</template>
  <ul class="mt-1">
    <li>Federated service &amp; central AWI deployment</li>
    <li>Group-level access control</li>
    <li>Multi-site search across DKRZ, AWI, ECMWF</li>
  </ul>
</ItemCard>

</div>

<p class="font-mono text-sm opacity-50 mt-2">github.com/esm-tools/esm_tools/src/esm_catalog</p>

<p class="text-sm opacity-40 mt-3">Pavan Siligam · Paul Gierz · Miguel Andres-Martinez</p>

</div>
