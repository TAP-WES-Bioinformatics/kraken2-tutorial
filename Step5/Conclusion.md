# Conclusion

Congratulations — you've completed the Kraken2 wastewater metagenomics tutorial!

## What you did

Starting from 50,000 reads of a real Chicago municipal wastewater sample (CASPER CHI-B, June 15, 2025), you:

1. Classified reads against the Kraken2 k2_viral database to assign viral taxonomy to each read
2. Interpreted the Kraken2 report format and found recognizable human-infecting viruses — including Influenza A (H3N2) and human cytomegalovirus — without targeting them in advance
3. Used `kreport2krona.py` to reshape the report for hierarchical visualization
4. Generated an interactive Krona sunburst plot to visually explore the viral community composition

That pathogen-agnostic workflow is the core idea behind early detection in wastewater: sequence first, then ask what is present. The same approach, at national scale and billion-read depth, can track known respiratory pathogens and surface unexpected threats.

## Going further

### Bracken: Improved abundance estimates

Kraken2 classification tends to assign reads conservatively — reads that match multiple taxa at a species level may be placed at a higher (genus or family) node. [Bracken](https://github.com/jenniferlu717/Bracken) (Bayesian Reestimation of Abundance with KrakEN) re-distributes these reads probabilistically to give more accurate species-level abundance estimates. It takes the Kraken2 report as input:

```
bracken -d $KRAKEN2_DB -i sample.report -o sample.bracken -r 150 -l S
```

The resulting `.bracken` file can also be converted for Krona using `kreport2krona.py`.

### Larger databases

The k2_viral database only classifies viral reads. For a broader picture of the microbial community — bacteria, fungi, and archaea — consider using a larger pre-built database:

| Database | Size (compressed) | Content |
|----------|------------------|---------|
| `k2_viral` | ~600 MB | All viral RefSeq genomes |
| `k2_standard_8gb` | ~8 GB | Bacteria, archaea, viruses, human |
| `k2_pluspf` | ~65 GB | Standard + protozoa + fungi |

Available at: [https://benlangmead.github.io/aws-indexes/k2](https://benlangmead.github.io/aws-indexes/k2)

### Multi-sample workflows

To classify and visualize multiple samples at once, KronaTools can combine multiple Krona text files into a single multi-tabbed HTML:

```
ktImportText sample1.krona.txt sample2.krona.txt sample3.krona.txt -o combined.krona.html
```

This is useful for comparing the viral community composition across different sampling sites or time points — for example, following influenza signals across CASPER sewersheds over a season.

## Resources

- [Kraken2 GitHub](https://github.com/DerrickWood/kraken2)
- [Kraken2 manual](https://github.com/DerrickWood/kraken2/wiki/Manual)
- [KrakenTools GitHub](https://github.com/jenniferlu717/KrakenTools)
- [KronaTools GitHub](https://github.com/marbl/Krona/wiki/KronaTools)
- [Bracken GitHub](https://github.com/jenniferlu717/Bracken)
- [Pre-built Kraken2 databases](https://benlangmead.github.io/aws-indexes/k2)
- [CASPER: Deep untargeted wastewater metagenomic sequencing across the United States (Justen et al. 2026)](https://doi.org/10.64898/2026.03.05.26345726)
