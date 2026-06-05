---
layout: default
zoom: 0.9
---

# HPC Storage Tier

Every item records where the data physically lives

| Storage Tier | Filesystem | State | Typical location |
|---|---|---|---|
| **hot** | Lustre / GPFS | online | `/albedo/work/…`, `/work/…` |
| **hpss** | Tape archive | offline | `/arch/…`, `/hpss/…` |
| **remote** | S3 / object store | online | remote URL |

Additional fields per item:
- **Facility** — AWI or DKRZ
- **System** — albedo, levante
- **Last Access** — when the file was last read

<div class="mt-6 p-3 bg-blue-50 dark:bg-blue-900 rounded text-sm">
💡 Use the storage tier to check before loading data: if <b>hpss</b>, the file is on tape and must be recalled first (HSM recall can take several minutes).
</div>
