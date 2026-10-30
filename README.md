# AdaSparCE Reproduction Package

Replication materials for **AdaSparCE: Loop Invariant Synthesis Guided by Semantics and Proof Feedback**.

The package provides the software, fixed experimental inputs, configurations,
dependency versions, and instructions needed to rerun the reported studies.
Historical result tables, search logs, solver traces, stored proof outcomes,
internal development records, and unpublished extensions are excluded.

本仓库只发布论文复现所需的公开包和使用说明，不包含作者历史实验结果、运行日志或未发表扩展。

## Download and verify

- [reproduction.tar.gz](reproduction.tar.gz): the complete reproduction package (about 1.38 MiB).
- [SHA256SUMS.txt](SHA256SUMS.txt): SHA-256 checksum for the archive.

From this repository's directory:

```bash
sha256sum -c SHA256SUMS.txt
tar -xzf reproduction.tar.gz
cd AdaSparCE
python3 verify.py
```

The archive extracts into `AdaSparCE/`. Its `README.md` contains the full study
protocols, dependency installation commands, input restoration steps, and
reproduction instructions. `verify.py` checks the file inventory, checksums,
source syntax, planned task identities, and fixed candidate pools offline.

## Environment

Use Linux, **Python 3.14.4**, and a system C preprocessor (`gcc`/`cpp`).
After extracting the archive:

```bash
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements.txt
```

The core dependencies are pinned to Z3 5.1.0.0, SymPy 1.14.0,
pycparser 3.0, and mpmath 1.3.0. External-baseline dependencies are installed
separately using the instructions inside the package.

## Run a smoke example

From the extracted `AdaSparCE/` directory, choose a fresh output directory:

```bash
python reproduce.py prepare --study ablation-nl50 --output /tmp/adasparce-smoke --smoke
python reproduce.py run --output /tmp/adasparce-smoke
python reproduce.py reverify --output /tmp/adasparce-smoke
python reproduce.py report --output /tmp/adasparce-smoke
```

This executes one original task and seed with the study's original budget,
then checks the returned invariant independently against the original
INIT, STEP, and EXIT verification conditions. New results and logs are created
locally. Smoke outputs are labeled separately from complete-study results.

## Reproduce complete studies

List the fixed study definitions:

```bash
python reproduce.py list
```

| Study identifiers | Scope |
| --- | --- |
| `main-lin316` | Complete LIN316 main study |
| `ablation-nl50` | NL50 capability ablations |
| `semantic-interfaces` | Semantic-output interface controls |
| `budget-nl50-600s` | Extended native-search budget |
| `shared-pool`, `joint-pool` | Fixed-candidate composition controls |
| `cold-nl50`, `cold-lin316` | Cold-process external comparisons |
| `input-support-*` | Fixed input-support and regression studies |

For a complete study, run the same four stages without `--smoke`:

```bash
python reproduce.py prepare --study ablation-nl50 --output /tmp/adasparce-ablation
python reproduce.py run --output /tmp/adasparce-ablation
python reproduce.py reverify --output /tmp/adasparce-ablation
python reproduce.py report --output /tmp/adasparce-ablation
```

Use separate output directories and run search, independent checking, and
statistics as separate stages. Full studies retain their original task lists,
seeds, configurations, budgets, worker counts, and job order.

The main NL50/LIN316 inputs are included. Additional third-party inputs follow
their pinned retrieval and reconstruction procedures:

```bash
python restore_inputs.py --group all
```

Fixed candidate pools are included as experimental inputs, with their original
generation charges. They contain no formal replay outcomes. See the package's
`THIRD_PARTY.md` for source provenance and retained third-party notices.

## Package scope

The release follows the necessary-software, data, instructions, and anonymization
scope described in the [ICST research-track replication policy](https://conf.researchr.org/track/icst-2027/icst-2027-research-papers).
For double-anonymous review, use the anonymized archive as a supplementary
attachment or through an anonymous mirror; a GitHub repository displays its owner.

This repository tracks only the archive, checksum, README, and ignore rules.
Extracted files, installed environments, and locally generated outputs are
ignored by default.
