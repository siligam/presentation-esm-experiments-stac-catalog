---
layout: default
---

# Federation — Many Databases, One Catalog

The STAC API serves multiple DuckDB files simultaneously

- Each experiment has its own `catalog.duckdb`
- New databases can be **registered without restarting** the server
- All registered databases are queried transparently as one unified catalog
- The `/ui` admin page shows which databases are active and lets you add new ones

```
ESM-Tools STAC Catalog
├── all-experiments-v2.duckdb   ● active
├── picontrol.duckdb            ● active      ← add without restart
└── historical.duckdb           ● active      ← add without restart
```

<div class="mt-6 p-3 bg-blue-50 dark:bg-blue-900 rounded text-sm">
The <b>/ui</b> admin page: enter the path to a <code>.duckdb</code> file → click Register → it appears in the catalog immediately.
</div>
