# Step 1: Taxonomic Classification with Kraken2

In this step, you'll classify the reads in a real wastewater metagenomics sample using Kraken2.

## Background

[Kraken2](https://github.com/DerrickWood/kraken2) is a k-mer-based taxonomic classifier. For every read in your FASTQ file, Kraken2 splits the sequence into short substrings of fixed length (k-mers) and looks up each one in a pre-built reference database. The taxonomy label assigned to the majority of a read's k-mers becomes that read's classification.

The database used in this tutorial is `k2_viral` — a pre-built database containing all viral sequences from NCBI RefSeq. This is appropriate for wastewater surveillance, where we're interested in detecting viruses circulating in a community.

## The data

This directory contains 50,000 paired-end reads from a real municipal wastewater composite. The sample comes from Chicago sewershed CHI-B (serving ~1.1 million people), collected as a 24-hour composite on June 15, 2025. Unlike a PCR assay that asks "is pathogen X present?", metagenomic sequencing asks "what viruses are here?" without knowing in advance which pathogens to look for.

Take a look at the first couple of reads from the forward file:

`head -n 8 sample_R1.fastq`

Each read in FASTQ format has four lines:

- Line 1: `@` followed by the read identifier
- Line 2: the nucleotide sequence
- Line 3: `+` (spacer)
- Line 4: the quality scores (one character per base)

`sample_R1.fastq` contains the forward reads and `sample_R2.fastq` contains the reverse reads from each sequenced fragment.

## Run Kraken2

Now, classify the reads. Because this is paired-end data, we use the `--paired` flag and provide both files:

`kraken2 --db $KRAKEN2_DB --paired --report sample.report --output sample.kraken sample_R1.fastq sample_R2.fastq`

Let's break down the options:

- `--db $KRAKEN2_DB` — path to the Kraken2 database (pre-set in this environment)
- `--paired` — tells Kraken2 that the two FASTQ files are paired-end reads
- `--report sample.report` — writes an aggregated summary table, one row per taxon
- `--output sample.kraken` — writes per-read classification results (one row per read)
- `sample_R1.fastq sample_R2.fastq` — forward and reverse reads

Kraken2 will print a brief summary to the terminal showing how many reads were classified and at what percentage. What fraction of reads were classified? Many reads in a real wastewater sample will be **unclassified** — this is expected, because the k2_viral database only contains viral sequences, and most wastewater reads come from bacteria, human DNA, and other sources.

## Check your outputs

Once Kraken2 finishes, you should see two new files:

- `sample.report` — the main output you'll analyze in the next steps
- `sample.kraken` — raw per-read results (large; not usually opened directly)

Take a quick look at `sample.kraken`:

`head sample.kraken`

Each line represents one read pair. The columns are:

1. `C` or `U` — classified or unclassified
2. Read identifier
3. NCBI taxonomy ID of the assigned taxon (0 = unclassified)
4. Read length in bp (shown as `R1length|R2length` for paired reads)
5. Space-separated list of k-mer hits: `taxid:count taxid:count ...`

When you're ready, `cd ../Step2` for the next step!
