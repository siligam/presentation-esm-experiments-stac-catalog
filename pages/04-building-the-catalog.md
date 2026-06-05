---
layout: default
---

# Building the Catalog

Two ways to create a `catalog.duckdb` for your experiment

### Automatic — during the model run

<div class="text-[0.82em] ml-5 mt-1">

The catalog is built automatically during the **tidy phase** of esm-tools.  
No extra steps needed if you run experiments with esm-tools.

</div>

<div class="mt-4">

### Manual — scan an existing output directory

<div class="text-[0.82em] ml-5 mt-1">

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

<div class="mt-6 p-3 bg-blue-50 dark:bg-blue-900 rounded text-sm">
For large experiments on Levante, a SLURM batch workflow is available:<br>
<code>esm-catalog scan-batch</code> → parallel Parquet files → <code>merge-parquet</code> → single DuckDB
</div>
