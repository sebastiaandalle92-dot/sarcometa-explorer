# SarcoMeta Explorer

SarcoMeta Explorer is the online companion to my meta-analysis of the human skeletal muscle transcriptome in sarcopenia, ageing and disuse.

**Open the explorer:** https://sebastiaandalle92-dot.github.io/sarcometa-explorer/

Sarcopenia, normal ageing and physical inactivity all cause muscle loss, but it is not clear how much they share at the molecular level. To look at this, I re-analysed 17 public RNA-seq cohorts (677 muscle biopsies) in a uniform way and pooled them into three comparisons:

- sarcopenia vs non-sarcopenic older adults
- old vs young
- older adults before vs after bed rest or reduced activity

The paper describes the main findings. This site lets you look up any gene or pathway yourself.

## What you can do

- Look up a gene and see its effect in every cohort, the pooled estimate per condition, and how it behaves in mouse models.
- Rank genes and pathways for each condition, or by the difference between conditions.
- Compare estimated cell-type composition (fibre type, FAPs, endothelial cells and others).
- Download the full result tables as CSV.

Each gene has its own link, for example `.../#gene/NOS1`.

## Data

All input data come from NCBI GEO; the accession numbers are listed on the Datasets tab. Counts were taken from ARCHS4 or, where ARCHS4 lacked samples, from the NCBI raw counts. Each cohort was analysed with limma-voom, and the results were combined with random-effects meta-analysis. The full methods are in the paper.

The `data/` folder holds the summary results that the site reads.

## Citation

If you use these results, please cite:

Dalle S. Sarcopenia, ageing and disuse in human skeletal muscle: a multi-cohort RNA-seq meta-analysis. Manuscript in preparation (2026).

I will update this with the journal reference once the paper is published.

## Licence

The code is released under the MIT licence. The summary results can be reused with citation (CC BY 4.0).

## Contact

Sebastiaan Dalle, University of Antwerp. Questions, suggestions or errors: please open an issue here or contact me directly: sebastiaan.dalle@uantwerpen.be or sebastiaan.dalle@kuleuven.be
