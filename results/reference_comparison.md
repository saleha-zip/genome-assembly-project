# Comparison with *S. cerevisiae* S288C Reference Genome (R64)

Reference source: [GCF_000146045.2_R64](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/146/045/GCF_000146045.2_R64/GCF_000146045.2_R64_assembly_stats.txt)

---

## Assembly-Level Comparison

| Metric | Hap1 | Hap2 | S288C R64 |
|--------|------|------|-----------|
| Total genome size | 12,160,988 bp | 11,304,582 bp | 12,157,105 bp |
| # chromosomes / scaffolds | 17 | 16 | 17 (16 nuclear + mito) |
| Largest scaffold | 1,531,728 bp | 1,532,843 bp | 1,531,933 bp (ChrIV) |
| Scaffold N50 | 923,452 bp | 922,430 bp | ~924,431 bp |
| GC content | 38.18% | 38.35% | ~38.2% |
| # gaps | 0 | 0 | Minimal |
| % complete BUSCOs | 95.2% | 87.9% | ~99% |

---

## Key Observations

**Genome size.** Hap1's total length (~12.16 Mb) closely matches the reference (~12.16 Mb), though this likely reflects residual haplotypic duplication rather than a true match — the slight size inflation seen in gfastats supports this. Hap2 (~11.30 Mb) is ~860 kb shorter than the reference, suggesting some sequence was collapsed or lost during haplotype resolution.

**Chromosome count.** Hap2's 16 scaffolds map cleanly onto the 16 nuclear chromosomes of S288C. Hap1's 17th scaffold likely represents either a split chromosome or an unresolved duplicated region rather than the mitochondrial chromosome, since the mitochondrial genome was not explicitly assembled here.

**Contiguity.** Both haplotypes achieve scaffold N50 values (~922–923 kb) effectively indistinguishable from the reference (~924 kb), confirming chromosome-scale assembly quality comparable to a finished reference.

**Gene content.** Hap1's BUSCO completeness (95.2%) approaches reference-level completeness. Hap2's lower score (87.9%), particularly the elevated missing BUSCOs (8.5% vs ~1% in the reference), suggests a meaningful fraction of gene-containing sequence was collapsed or not recovered in that haplotype.

**Sequence accuracy.** Merqury QV scores (>79 for hap1, effectively perfect for hap2) indicate base-level accuracy comparable to reference-grade sequence. The near-zero error rates confirm that HiFi reads produce highly accurate consensus sequences even without polishing.

**Repeat and GC content.** GC content in both haplotypes (38.18–38.35%) is in excellent agreement with the reference (~38.2%). GenomeScope estimated ~0.72 Mb of repeat sequence, consistent with the known low repeat content of *S. cerevisiae*.

---

## Summary

Hap1 is the more biologically complete assembly (higher BUSCO score, matches reference size) but carries minor structural redundancies. Hap2 is the structurally cleaner haplotype (16 scaffolds matching 16 chromosomes, no inflated size), though at the cost of some missing gene content. Together, the two haplotypes provide a comprehensive, high-accuracy representation of the diploid *S. cerevisiae* genome, with contiguity and base accuracy on par with the S288C reference.
