# Genome Assembly Project — *Saccharomyces cerevisiae*

A haplotype-resolved, chromosome-scale *de novo* genome assembly of *S. cerevisiae* generated using PacBio HiFi long reads, Hi-C chromatin contact data, and BioNano optical mapping. The pipeline was executed on [Galaxy](https://usegalaxy.eu/) following the [VGP Genome Assembly tutorial](https://training.galaxyproject.org/training-material/topics/assembly/tutorials/vgp_genome_assembly/tutorial.html).

---

## Table of Contents

1. [Directory Structure](#directory-structure)
2. [Reproducibility Instructions](#reproducibility-instructions)
3. [Execution Checkpoints](#execution-checkpoints)
4. [Interpretation of Key Results](#interpretation-of-key-results)
5. [Comparison with S288C Reference Genome](#comparison-with-s288c-reference-genome)

---

## Directory Structure

```
genome-assembly-project/
├── README.md
├── data/
│   └── download_links.txt           # Zenodo URLs for all input datasets
├── docs/
│   └── workflow_documentation.pdf   # Step-by-step execution record with screenshots
└── results/
    ├── reference_comparison.md      # Detailed comparison with S. cerevisiae S288C
    ├── assembly/
    │   ├── Hap1 assembly bionano.fasta
    │   ├── Hap1 contigs FASTA.fasta
    │   ├── Hap2 contigs FASTA.fasta
    │   └── YaHS Scaffolds FASTA (hap1).fasta
    ├── busco/
    │   ├── BUSCO hap1 - Full table.tabular
    │   ├── BUSCO hap1 - Short summary.txt
    │   ├── BUSCO hap1 - Summary image.png
    │   ├── BUSCO hap2 - Full table.tabular
    │   ├── BUSCO hap2 - Short summary.txt
    │   └── BUSCO hap2 - Summary image.png
    ├── genomescope/
    │   ├── GenomeScope on meryldb histogram Linear plot.png
    │   ├── GenomeScope on meryldb histogram Log plot.png
    │   ├── GenomeScope on meryldb histogram Model parameters.tabular
    │   ├── GenomeScope on meryldb histogram Summary.txt
    │   ├── GenomeScope on meryldb histogram Transformed linear plot.png
    │   ├── GenomeScope on meryldb histogram Transformed log plot.png
    │   └── meryldb histogram.tabular
    ├── merqury/
    │   ├── hap1 and hap2 Merqury QV stats (using merged meryldb)/
    │   │   ├── output_merqury.assembly_01.tabular
    │   │   ├── output_merqury.assembly_02.tabular
    │   │   └── output_merqury.tabular
    │   ├── hap1 and hap2 Merqury plots (using merged meryldb)/
    │   │   ├── output_merqury.assembly_01.spectra-cn.ln.png
    │   │   ├── output_merqury.assembly_02.spectra-cn.ln.png
    │   │   ├── output_merqury.spectra-asm.ln.png
    │   │   └── output_merqury.spectra-cn.ln.png
    │   └── hap1 and hap2 Merqury stats (using merged meryldb)/
    │       └── output_merqury.completeness.tabular
    ├── scaffolding/
    │   ├── Pretext Snapshot for PretextMap output YaHS/
    │   │   ├── pretext_snapshotFullMap.png
    │   │   ├── pretext_snapshotscaffold_1.png
    │   │   └── pretext_snapshotscaffold_2.png
    │   └── Pretext Snapshot for PretextMap output of BAM Hi-C reads/
    │       ├── pretext_snapshotFullMap.png
    │       ├── pretext_snapshotSuper-Scaffold_1.png
    │       ├── pretext_snapshotSuper-Scaffold_2.png
    │       └── pretext_snapshoth1tg000007l_path_obj.png
    └── stats/
        ├── Hap1 stats.tabular
        ├── Hap2 stats.tabular
        ├── gfastats on hap1 and hap2 (full).tabular
        └── gfastats on hap1 and hap2 contigs.tabular
```

> **Note:** Only a representative subset of scaffolding snapshots (scaffolds 1–2 for both the Hi-C BAM and YaHS contact maps) has been retained. The BioNano-scaffolded hap1 assembly is included because it is referenced as an intermediate in the Hi-C scaffolding steps.

---

## Reproducibility Instructions

### Input Data

All raw input data are publicly available on Zenodo. URLs are listed in `data/download_links.txt`. The key datasets are:

| File | Source | Type |
|------|--------|------|
| `HiFi_synthetic_50x_01.fasta` | [zenodo.org/record/6098306](https://zenodo.org/record/6098306) | PacBio HiFi reads (FASTA) |
| `HiFi_synthetic_50x_02.fasta` | [zenodo.org/record/6098306](https://zenodo.org/record/6098306) | PacBio HiFi reads (FASTA) |
| `HiFi_synthetic_50x_03.fasta` | [zenodo.org/record/6098306](https://zenodo.org/record/6098306) | PacBio HiFi reads (FASTA) |
| `SRR7126301_1.fastq.gz` | [zenodo.org/record/5550653](https://zenodo.org/record/5550653) | Hi-C forward reads (fastqsanger.gz) |
| `SRR7126301_2.fastq.gz` | [zenodo.org/record/5550653](https://zenodo.org/record/5550653) | Hi-C reverse reads (fastqsanger.gz) |
| `bionano.cmap` | [zenodo.org/records/5887339](https://zenodo.org/records/5887339) | BioNano optical map (cmap) |

### Platform

All steps were run on [Galaxy Europe](https://usegalaxy.eu/). To reproduce this analysis:

1. Create an account at usegalaxy.eu.
2. Create a new History named (e.g.) `VCF assembly pipeline`.
3. Upload data via **Upload → Paste/Fetch Data** using the URLs above, setting the correct datatypes (`fasta`, `fastqsanger.gz`, `cmap`).
4. Rename Hi-C datasets to `Hi-C_dataset_F` (forward) and `Hi-C_dataset_R` (reverse).
5. Collect the three HiFi FASTA files into a **Dataset Collection** (list) named `Hi-Fi fasta files`.
6. Execute each tool in the order described in [Execution Checkpoints](#execution-checkpoints), using the parameter settings documented in `docs/workflow_documentation.pdf`.

### Tool Versions Used

| Tool | Galaxy Version |
|------|---------------|
| Cutadapt | 5.2+galaxy9 |
| Meryl | 1.3+galaxy6 |
| GenomeScope | 2.1.0+galaxy9 |
| Hifiasm | 0.25.0+galaxy3 |
| gfastats | 1.3.11+galaxy2 |
| BUSCO | 5.8.0+galaxy2 |
| Merqury | 1.3+galaxy4 |
| Bionano Hybrid Scaffold | 3.7.0+galaxy3 |
| BWA-MEM2 | 2.3+galaxy0 |
| Filter and merge (Arima) | 1.0+galaxy1 |
| PretextMap | 0.2.4+galaxy0 |
| Pretext Snapshot | 0.0.7+galaxy0 |
| YaHS | 1.2a.2+galaxy3 |
| Column join | 0.0.3 |
| Search in textfiles (grep) | 9.5+galaxy3 |

---

## Execution Checkpoints

### 1. Data Upload

Three HiFi FASTA files (50× synthetic coverage each) were uploaded from Zenodo with datatype set to `fasta`. Two Hi-C datasets were uploaded with datatype `fastqsanger.gz` and renamed `Hi-C_dataset_F` and `Hi-C_dataset_R`. The three HiFi files were organised into a dataset list collection (`Hi-Fi fasta files`).

---

### 2. Adapter Trimming — Cutadapt

HiFi reads were pre-processed using **Cutadapt** to remove PacBio-specific adapter sequences. Reads containing an adapter were **discarded entirely** (rather than trimmed) to exclude low-quality reads.

**Key parameters:**

| Parameter | Value |
|-----------|-------|
| Adapter 1 | `ATCTCTCTCAACAACAACAACGGAGGAGGAGGAAAAGAGAGAGAT` |
| Adapter 2 | `ATCTCTCTCTTTTCCTCCTCCTCCGTTGTTGTTGTTGAGAGAGAT` |
| Adapter position | 5' or 3' (Anywhere) |
| Minimum overlap length | 35 |
| Maximum error rate | 0.1 |
| Discard trimmed reads | **Yes** |
| Match wildcards in adapters | Yes |
| Look for reverse complement | Yes |

**Output:** `HiFi_collection (trimmed)` — a list of 3 trimmed FASTA datasets.

---

### 3. Genome Profiling — Meryl (k-mer counting)

K-mer count distributions were generated using **Meryl** in three runs:

1. **Run 1 (Count):** Meryl was run in `Count` mode on each of the 3 HiFi FASTA files individually (k-mer size = 31), producing a collection of 3 merylDB databases.
   - K-mer size 31 was chosen because it yields ~96.96% unique k-mers for a human-scale genome and is appropriate for yeast as well.

2. **Run 2 (Union-sum):** The 3 merylDB files were merged into a single database (`Merged meryldb`) using `union-sum`.

3. **Run 3 (Histogram):** The merged merylDB was used to generate a k-mer frequency histogram (`meryldb histogram`), which serves as input to GenomeScope.

---

### 4. Genome Profiling — GenomeScope

**GenomeScope** was run on the meryldb histogram to infer genome properties without a reference.

**Parameters:**

| Parameter | Value |
|-----------|-------|
| Ploidy | 2 |
| K-mer length | 31 |
| Output | Summary of the analysis |

**Outputs generated:** Linear plot, Log plot, Transformed linear plot, Transformed log plot, Summary text, Model parameters.

> The genome size estimated by GenomeScope (**11,743,432 bp**) was used as the expected genome size in all downstream gfastats evaluations (slightly different from the tutorial's expected value of 11,747,160 bp).

---

### 5. *De Novo* Assembly — Hifiasm

**Hifiasm** was used to generate a haplotype-resolved assembly in **Hi-C mode**.

**Parameters:**

| Parameter | Value |
|-----------|-------|
| Assembly mode | Hi-C partition |
| Input reads | `HiFi_collection (trimmed)` |
| Hi-C R1 reads | `Hi-C_dataset_F` |
| Hi-C R2 reads | `Hi-C_dataset_R` |
| Bits for bloom filter | 37 |
| Assembly options | Default |
| Options for purging duplicates | Default |

**Outputs:** Hap1 contigs graph (GFA), Hap2 contigs graph (GFA), Hi-C primary/alternate contig graphs, raw unitig graph, and a collection of 6 GFA files without sequence. The haplotype outputs were manually tagged `#hap1` and `#hap2`.

---

### 6. GFA to FASTA Conversion — gfastats

The Hap1 and Hap2 GFA contig graph files were converted to FASTA format using **gfastats** in batch mode.

**Parameters:**

| Parameter | Value |
|-----------|-------|
| Input | Hap1 contigs graph + Hap2 contigs graph |
| Tool mode | Genome assembly manipulation |
| Output format | FASTA |

**Outputs:** `Hap1 contigs FASTA`, `Hap2 contigs FASTA`.

---

### 7. Assembly Statistics — gfastats (Stats mode)

Assembly statistics were generated for both haplotypes using **gfastats** in summary statistics mode.

**Key parameter:**

| Parameter | Value |
|-----------|-------|
| Expected genome size | 11,743,432 |
| Report mode | Genome assembly statistics (--nstar-report) |

**Outputs:** `Hap1 stats`, `Hap2 stats`.

---

### 8. Merging Stats — Column Join

The Hap1 and Hap2 stats tabular files were merged side-by-side using **Column join** (identifier column = 1, 0 header lines). The combined output (`gfastats on hap1 and hap2 (full)`) was then filtered with **grep** (excluding lines matching `scaffold`, case-insensitive) to produce a contig-focused summary (`gfastats on hap1 and hap2 contigs`).

---

### 9. Assembly Completeness — BUSCO

**BUSCO** was run against the `saccharomycetes_odb10` lineage dataset in genome mode (`euk_genome_met`), using Metaeuk as the gene predictor, on both Hap1 and Hap2 FASTA files in batch mode.

> BUSCO was initially unresponsive during the tutorial run but completed successfully on re-run. Results are included in `results/busco/`.

---

### 10. K-mer Based Evaluation — Merqury

**Merqury** was run in default (diploid) mode using the `Merged meryldb` as the k-mer database, with Hap1 contigs FASTA as assembly_01 and Hap2 contigs FASTA as assembly_02.

**Outputs (3 collections):**
- QV stats (consensus accuracy quality values)
- Plots (spectra-cn and spectra-asm for both assemblies)
- Completeness stats

---

### 11. Scaffolding — BioNano Hybrid Scaffold

The BioNano optical map (`bionano.cmap`) was used to scaffold Hap1 contigs using **Bionano Hybrid Scaffold** in VGP mode.

**Key parameters:**

| Parameter | Value |
|-----------|-------|
| Input NGS FASTA | Hap1 contigs FASTA |
| BioNano CMAP | bionano.cmap |
| Configuration mode | VGP mode |
| Restriction enzyme | CTTAAG |
| Genome/Sequences conflict filter | Cut contig at conflict |

The scaffolded and unscaffolded trimmed outputs were concatenated using **Concatenate datasets** to produce `Hap1 assembly bionano`.

---

### 12. Hi-C Scaffolding Pre-processing — BWA-MEM2 + Filter and Merge

Hi-C reads were mapped to the BioNano-scaffolded Hap1 assembly using **BWA-MEM2** (Simple Illumina mode, BAM sorted by read name), run separately for forward and reverse reads. The two BAM outputs were then merged and chimeric reads removed using **Filter and merge chimeric reads from Arima Genomics**, producing `BAM Hi-C reads`.

---

### 13. Pre-scaffolding Contact Map — PretextMap + Pretext Snapshot

**PretextMap** converted the merged BAM file into a genome contact map (no high-resolution mode, unsorted). **Pretext Snapshot** then rendered the contact map as PNG images (1000 px resolution) — one full map and one image per scaffold — for visual inspection before YaHS scaffolding.

---

### 14. Hi-C Scaffolding — YaHS

**YaHS** performed Hi-C-based scaffolding of the BioNano assembly using the merged Hi-C BAM.

**Key parameter:**

| Parameter | Value |
|-----------|-------|
| Restriction enzyme sequence | CTTAAG |
| Input AGP file | None (not rescaffolding) |

**Outputs:** YaHS Scaffolds FASTA, BAM forward YaHS, BAM reverse YaHS.

---

### 15. Post-scaffolding Contact Map — Filter & Merge + PretextMap + Pretext Snapshot

The YaHS BAM outputs (forward and reverse) were merged using **Filter and merge** to produce `BAM Hi-C reads YaHS`. **PretextMap** and **Pretext Snapshot** were re-run on this merged BAM to generate the final post-scaffolding contact map for quality assessment.

---

## Interpretation of Key Results

### GenomeScope Summary

| Property | Min | Max |
|----------|-----|-----|
| Homozygous (aa) | 99.4165% | 99.4241% |
| Heterozygous (ab) | 0.5759% | 0.5835% |
| Genome Haploid Length | 11,739,513 bp | 11,747,352 bp |
| Genome Repeat Length | 723,114 bp | 723,597 bp |
| Genome Unique Length | 11,016,399 bp | 11,023,756 bp |
| Model Fit | 92.52% | 96.52% |
| Read Error Rate | 0.00094319% | 0.00094319% |

GenomeScope estimates a haploid genome size of approximately **11.74 Mb**, consistent with the known *S. cerevisiae* genome. The genome is highly homozygous (~99.4%) with low heterozygosity (~0.58%). The majority of the genome is unique sequence (~11.0 Mb), with modest repetitive content (~0.72 Mb). The very low read error rate (~0.00094%) confirms high-quality HiFi sequencing, and the strong model fit (92–96%) indicates good agreement between the observed k-mer distribution and the inferred genome model. These properties — low complexity, high homozygosity, minimal repeats — make this genome well-suited for accurate assembly.

The **GenomeScope linear plot** shows two clear k-mer peaks at ~25× and ~50× coverage, corresponding to heterozygous and homozygous loci respectively, confirming the diploid structure. The dominant higher peak and narrow distributions reflect high sequencing quality and low repeat content, while the minimal low-coverage tail (error k-mers) further supports data quality.

---

### Assembly Statistics (gfastats)

| Metric | Hap1 | Hap2 |
|--------|------|------|
| # scaffolds | 17 | 16 |
| Total scaffold length | 12,160,988 bp | 11,304,582 bp |
| Scaffold N50 | 923,452 bp | 922,000 bp |
| Scaffold auN | 904,515 bp | ~895,000 bp |
| Scaffold L50 | 6 | 6 |
| Largest scaffold | 1,531,728 bp | — |
| # gaps | 0 | 0 |
| GC content | 38.18% | — |

**Hap1:** The assembly consists of 17 scaffolds totalling ~12.16 Mb, slightly exceeding the expected genome size (~11.74 Mb). This modest inflation may indicate residual haplotypic duplication or incomplete collapsing of homologous regions. The high scaffold N50 (~923 kb) and auN (~905 kb) indicate strong contiguity. The absence of gaps (0 gaps) reflects the high continuity achieved by HiFi long reads.

**Hap2:** With 16 scaffolds totalling ~11.30 Mb — closely matching the expected genome size — hap2 is likely a more accurate structural representation. Its scaffold count aligns with the known 16-chromosome count of *S. cerevisiae* S288C. The similarly high N50 (~922 kb) confirms comparable contiguity to hap1. The closer agreement with expected size suggests fewer duplicated or redundant regions, implying a cleaner haplotype representation overall.

---

### BUSCO Assessment

| Metric | Hap1 | Hap2 |
|--------|------|------|
| Complete BUSCOs (C) | 95.2% (2034/2137) | 87.9% (1879/2137) |
| Complete & single-copy (S) | 93.3% (1994) | 86.5% (1849) |
| Complete & duplicated (D) | 1.9% (40) | 1.4% (30) |
| Fragmented (F) | 3.4% (73) | 3.6% (77) |
| Missing (M) | 1.4% (30) | 8.5% (181) |
| Lineage | saccharomycetes_odb10 | saccharomycetes_odb10 |

**Hap1** shows high biological completeness (95.2% complete BUSCOs), with the large majority being single-copy (93.3%), indicating accurate gene representation without significant redundancy. The low duplication rate (1.9%) and small missing/fragmented fractions suggest a high-quality, near-complete assembly.

**Hap2** shows notably lower completeness (87.9%), with a considerably higher proportion of missing genes (8.5%). While duplication remains low (1.4%), the elevated missing and fragmented BUSCOs suggest reduced gene recovery, possibly due to collapsed or incompletely reconstructed regions. Hap2 is therefore less complete from a gene-content perspective, despite its better agreement with expected genome size.

---

### Merqury — Consensus Accuracy (QV Stats)

| Assembly | k-mers with errors | Total k-mers | QV | Error rate |
|----------|-------------------|--------------|-----|------------|
| assembly_01 (Hap1) | 4 | 12,160,478 | 79.74 | 1.06 × 10⁻⁸ |
| assembly_02 (Hap2) | 0 | 11,304,102 | — | 0 |
| Both | 4 | 23,464,580 | 82.60 | 5.50 × 10⁻⁹ |

The Merqury QV statistics indicate exceptionally high consensus accuracy across both assemblies, with near-zero error rates. The high QV scores (>79 for hap1, effectively perfect for hap2) confirm that the base-level sequence produced by HiFi reads is highly accurate.

### Merqury — Spectra-cn Plot

The spectra-cn (copy-number spectra) plot shows that the vast majority of k-mers are found at copy number 1 (red peak, ~25× and ~50× multiplicity), reflecting single-haplotype representation. The near-absence of k-mers at copy numbers ≥2 confirms minimal artificial duplication. The small read-only k-mer signal (black, low coverage) indicates minimal sequence is missing from the assembly, supporting overall completeness.

### Merqury — Spectra-asm Plot

The spectra-asm plot reveals the k-mer distribution across both assemblies. The large shared (green) peak at ~50× demonstrates that most genomic content is correctly captured and shared between hap1 and hap2. The overlapping but distinct assembly-specific peaks (red for hap1, blue for hap2) at ~25× reflect genuine haplotype-level differences rather than assembly errors. The low read-only signal (black) confirms minimal missing sequence overall.

---

### Hi-C Contact Map (YaHS — pretext_snapshotFullMap.png)

The post-YaHS Hi-C contact map shows a strong, continuous diagonal with well-defined square blocks, each corresponding to an individual chromosome-scale scaffold. The clear separation between blocks and the near-absence of off-diagonal signal indicate correctly ordered and oriented contigs with minimal misassemblies — no major inter-chromosomal contacts or structural inconsistencies are visible. This reflects a high-quality scaffolding outcome with chromosome-scale organisation and good long-range structural accuracy.

---

## Comparison with S288C Reference Genome

> Detailed comparisons are maintained in [`results/reference_comparison.md`](results/reference_comparison.md).

The *S. cerevisiae* S288C reference genome (assembly R64; GCF_000146045.2) serves as the benchmark for this assembly. Key reference statistics are available at:  
`https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/146/045/GCF_000146045.2_R64/GCF_000146045.2_R64_assembly_stats.txt`

A summary comparison is provided below:

| Metric | This Assembly (Hap1) | This Assembly (Hap2) | S288C Reference (R64) |
|--------|---------------------|---------------------|----------------------|
| Total genome size | ~12.16 Mb | ~11.30 Mb | ~12.16 Mb |
| # chromosomes / scaffolds | 17 | 16 | 17 (16 chr + mito) |
| Scaffold N50 | ~923 kb | ~922 kb | ~924 kb |
| GC content | 38.18% | — | ~38.2% |
| % complete BUSCOs | 95.2% | 87.9% | ~99% (reference) |
| Gaps | 0 | 0 | Minimal |

**Summary observations:**

- **Genome size:** Hap2 (~11.30 Mb) matches the S288C reference size more closely than Hap1 (~12.16 Mb), suggesting Hap1 retains some redundant or duplicated sequence. Hap1's total length coincidentally matches the reference total but likely includes minor haplotypic duplications.
- **Chromosome count:** Hap2's 16 scaffolds align exactly with the 16 nuclear chromosomes of *S. cerevisiae*, whereas Hap1's 17 scaffolds may reflect one chromosome being split or an unresolved duplication.
- **Contiguity:** Both haplotypes achieve scaffold N50 values (~922–923 kb) comparable to the reference, indicating chromosome-scale assembly quality.
- **Gene content:** Hap1's BUSCO completeness (95.2%) approaches reference-level completeness, while Hap2's (87.9%) suggests some gene content is missing or collapsed.
- **Sequence accuracy:** The near-zero Merqury error rates for both haplotypes demonstrate that HiFi-based assembly achieves reference-grade base accuracy.

See [`results/reference_comparison.md`](results/reference_comparison.md) for chromosome-level size comparisons, synteny analysis, and further discussion of structural differences relative to S288C.

