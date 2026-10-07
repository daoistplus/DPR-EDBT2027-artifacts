# DPR: Mitigating Update-Induced Plan Regressions in Learned Query Optimizers

Supplementary artifacts accompanying the manuscript prepared for EDBT 2027.

**Authors:** Kexiao Zhang, Shiyin Wang, Siyu Zhan, and Zhao Kang.

DPR (Decision Preservation Replay) selects historical queries whose plan rankings are at risk during model adaptation and preserves execution-derived plan preferences under a fixed replay budget. This repository distributes the archived implementation and recorded experimental evidence used in the manuscript.

## Download

[Download DPR-supplementary.zip](https://github.com/daoistplus/DPR-EDBT2027-artifacts/raw/refs/heads/main/DPR-supplementary.zip) (20,920,218 bytes, approximately 20.9 MB).

The archive is the artifact package; GitHub's **Code → Download ZIP** downloads this repository, which contains the artifact ZIP rather than its expanded contents. Extract `DPR-supplementary.zip` into an empty directory before using it.

**SHA-256**

```text
734228f3ace6d6877220c46e101b944a146d073d2e4c3bf94c3a431265e0d0b0
```

No sign-in or access request is needed to download the public files. This repository does not embed author-controlled visitor trackers, tracking pixels, or external analytics.

## Quick verification

From the extracted artifact directory, run the following with Python 3. These commands use the Python standard library and do not require PostgreSQL, GPUs, or training dependencies.

```bash
python3 verify_package.py
python3 measurements/verify.py
python3 selection/verify_selection.py
```

The checks validate payload sizes and SHA-256 hashes, recompute the forward Full-DPR CP4/CP9 measurements for 32 test queries with four scored timings per query, and reproduce the forward and reverse DPR/pairwise-MIR top-K selections. They check archived records; they do not execute SQL or retrain a model.

## Package contents

| Path inside the archive | Contents |
| --- | --- |
| `README.md` | Detailed verification and training preparation guide |
| `PACKAGE_MANIFEST.json` | File-level integrity and provenance |
| `RUN_INDEX.json` | Run configurations, seeds, workload names and input/runner hashes |
| `code/formal-v17/` | Archived training driver, replay strategies, preservation loss, CAGrad integration and tests |
| `code/LIMAO-final-workload/` | Upstream LIMAO/Balsa snapshot, workload SQL, dependency declarations and original license |
| `code/analysis/` | Analysis and measurement tools |
| `environment/README.md` | Evidence-backed execution environment and preparation requirements |
| `evidence/` | Forward/reverse experiments, replay-budget comparisons, configuration studies and recorded provenance |
| `measurements/` | Recorded measurements and result recomputation |
| `selection/` | Recorded query selections and selection verification |

## Experiment map

The adopted Full-DPR configurations are:

- **Forward:** `dpr-cagrad05-budget20-forward-dev-v1`, seed 5001, A3 → A4.
- **Reverse:** `cagrad05-budget20-reverse-confirm-v1`, seed 5101, A4 → A3.
- **Selector configuration comparison:** `uniform-selection-cagrad05-budget20-forward-ablation-v1`.
- **Standard-training comparison:** `dpr-budget20-forward-v3`.

Full trajectories consist of five historical rounds, five incoming rounds and one return round. Each configuration has its own CP4. Full-DPR and Uniform+keep share the initial model and training schedule, but not a common CP4 or identical recorded latency labels. Reloaded measurements and development screens are not independent seed repetitions.

## Training preparation and external assets

The recorded execution environment is Python 3.8.20, PostgreSQL 12.5 and pg_hint_plan 1.3.7. See `environment/README.md` and the archived run provenance for the complete environment and input requirements. Upstream requirements are preserved declarations, not a newly validated dependency lockfile.

Full training also requires the IMDb database, compatible LIMAO/Balsa dependencies, model checkpoints, simulator features/data and initial-policy binaries. These large assets and exhaustive logs are excluded from the downloadable package. The authors retain the original full archives; external assets and additional historical freezes can be requested from the corresponding author, subject to original distribution terms.

Use each run's recorded configuration and checksums, restore the required inputs, and replace original machine paths with local paths. Do not run the training driver with defaults and assume it reproduces a paper configuration. Full DPR uses bounded query replay, DPR selection, K=20 and CAGrad with c=0.5.

The package supports result verification and implementation inspection. It is not a self-contained database-and-training image, and a clean-instance training restoration has not been validated. No new training or SQL execution was performed when assembling this release.

## Attribution and use

The package preserves upstream copyright and license notices. LIMAO/Balsa and CAGrad are credited in the manuscript and implementation. Consult the relevant notices inside the archive when reusing third-party code; this repository does not replace those terms with a blanket license.

**Corresponding author:** Zhao Kang, University of Electronic Science and Technology of China, `Zkang@uestc.edu.cn`.
