---
layout: two-cols-header
zoom: 0.95
---

# Finding Experiments and Variables

The search page has two tabs

::left::

### Search for Experiments
<ul class="text-[0.82em] mt-1 ml-5 space-y-1">
  <li>Model components (ECHAM, FESOM, JSBACH, …)</li>
  <li>CO₂ / CH₄ / N₂O level ranges</li>
  <li>Paleo time period (LGM, Mid-Holocene, Eemian, PI)</li>
  <li>Experiment type (Control, Historical, Paleo, Sensitivity, …)</li>
  <li>Output frequency (Monthly, Daily, 6-hourly, …)</li>
</ul>

::right::

### Search for Items
<ul class="text-[0.82em] mt-1 ml-5 space-y-1">
  <li>Variable name, temporal extent</li>
  <li>Collection (experiment), Item IDs</li>
  <li>Any namelist parameter via <strong>Additional Filters</strong></li>
</ul>

<div class="mt-6 p-3 bg-blue-50 dark:bg-blue-900 rounded text-sm">
💡 Example: <code>component = 'fesom'</code> finds all FESOM ocean output files across every experiment — one filter, 700 files.
</div>
