---
layout: default
---

# Comparing Experiments

Select any two (or more) experiments and open the **Collection Comparison** modal

- Side-by-side table of all Fortran namelist parameters
- Toggle **"Show only differences"** to instantly filter to what changed
- Filter by parameter name to focus on a specific namelist section

<div class="mt-4 p-3 bg-blue-50 dark:bg-blue-900 rounded text-sm">
Example — basic-001 vs basic-002 differences:

| Parameter | basic-001 | basic-002 |
|---|---|---|
| echam:runctl:dt_stop | [1850,4,1] | [1850,5,1] |
| echam:runctl:out_expname | basic-001 | basic-002 |
| fesom:paths:resultpath | …/basic-001/… | …/basic-002/… |
</div>

<div class="mt-4 p-3 bg-green-50 dark:bg-green-900 rounded text-sm">
💡 Instantly answers: <i>"What did I change between these two runs?"</i> — without opening any config files.
</div>
