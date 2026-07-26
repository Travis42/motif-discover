# motif-discover

Fast, accurate de novo motif discovery for ChIP-seq data. A static x86-64 Linux binary — no dependencies required.

## Results

Benchmarked on **132 ENCODE K562 ChIP-seq transcription factor profiles**:

| Metric | motif-discover | STREME | MEME |
|--------|---------------|--------|------|
| AUROC (shuffled neg.) | **0.842** | 0.803 | — |
| AUROC (genomic neg.) | **0.891** | 0.863 | 0.881 |
| Time per TF | **0.3s** | 3.3s | ~90s |
| Speedup vs STREME | — | 1.0× | — |

motif-discover achieves a mean AUROC improvement of **+3.9%** over STREME on shuffled dinucleotide negatives (85 wins, 43 losses, 4 ties; Wilcoxon p = 4.1×10⁻⁷) and **+2.8%** on real genomic hg38 negatives (35 wins, 16 losses; Wilcoxon p = 6.5×10⁻⁴).

On genomic negatives, motif-discover also edges out MEME (0.891 vs 0.881) while running **300× faster**.

### How to run the benchmark

```
./motif-discover --data <data_dir> --ours-only
```

This runs motif-discover only (no STREME/MEME comparison). For each `*_sequences.fa` file, it discovers a motif on the central 100bp and evaluates AUROC against dinucleotide-shuffled negatives on the full 500bp sequences.

## Quick Start

```
git clone https://github.com/Travis42/motif-discover.git
cd motif-discover
chmod +x motif-discover
./motif-discover --data example/ --ours-only
```

The `example/` directory contains 11 ENCODE K562 ChIP-seq profiles. Runtime: ~4 seconds.

## Download

**[motif-discover](motif-discover)** — Static x86-64 Linux binary (928 KB)

## Usage

```
./motif-discover --data <data_dir> [--tf TF_NAME|ALL] [--markov] [--negatives <file>]
```

**Arguments:**
- `--data <dir>` — Directory containing `*_sequences.fa` files (500bp peak sequences)
- `--tf <name>` — Specific transcription factor name, or `ALL` for every TF in the directory
- `--markov` — Use Markov-1 background for scoring (recommended for genomic negatives)
- `--negatives <file>` — External negatives FASTA file (e.g., genomic regions)
- `--ours-only` — Skip STREME/MEME comparison
- `--no-meme` — Skip MEME comparison (use with `--ours-only` for fastest runs)

**Input format:** FASTA files named `<TF>_sequences.fa`, containing 500bp peak-centered sequences. The tool extracts the central 100bp for motif discovery and evaluates on full 500bp sequences.

**Output:** Tab-separated to stderr:

```
TF      Width   AUROC   Time_s  Source  P_value Pos_hit Neg_hit
USF1    12      0.9923  0.3     ours    1.2e-47 189     7
```

## Requirements

- x86-64 Linux (kernel 3.2.0+)
- No dependencies (statically linked)

## Reproducibility

Benchmark data and Jupyter notebooks are included in this repository:

- `benchmark_data/` — Pre-computed results for all 132 TFs (shuffled and genomic negatives)
- `demo_notebook.ipynb` — Live demo + benchmark visualization (also runs on Google Colab)

All benchmarks use identical input data, scoring code, and negative sets across tools. Width range 6–17 for all tools. STREME and MEME invoked with standard parameters from MEME Suite 5.5.5.

## About

Source code available upon request for academic collaboration.

## Citation

*Paper in preparation.*

## Contact

Travis — travis.smith42@pm.me
