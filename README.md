# kraken2-tutorial

Welcome to the Kraken2 analysis component of the wastewater metagenomics workshop!

The sample used in this tutorial is a real hospital wastewater grab collected on **December 23, 2025** from a manhole serving New York City Hospital D (SRR37367583), part of the [CASPER](https://naobservatory.org/casper/) (Coalition for Agnostic Sequencing of Pathogens from Environmental Reservoirs) network. CASPER uses deep, untargeted metatranscriptomic sequencing to monitor pathogens in wastewater across the United States. The full dataset and methods are described in [Justen et al. 2026](https://doi.org/10.64898/2026.03.05.26345726).

Use the left sidebar to browse files, and click on them to open them in the file viewer. Kraken2 should be pre-installed in your terminal. To verify, run:

```
kraken2 --version
```

You'll find each step of the tutorial as a numbered directory in the sidebar, starting with `Step1`. Each directory contains all the instructions and data needed for that step. Go ahead and `cd` into `Step1` and open `Step1.md` to get started.

## What you'll learn

In this tutorial, you'll take a real wastewater metagenomics FASTQ file through the full Kraken2 classification pipeline:

1. **Classify reads** — Run Kraken2 to assign taxonomy to every read in the sample
2. **Interpret the report** — Understand the Kraken2 report format and explore which organisms were detected
3. **Convert to Krona format** — Use KrakenTools to reshape the report for visualization
4. **Visualize with Krona** — Generate an interactive sunburst plot to explore the taxonomic composition of your sample

## Running in GitHub Codespaces

This tutorial is designed to run in [GitHub Codespaces](https://github.com/features/codespaces). All dependencies (Kraken2, KrakenTools, KronaTools) and the Kraken2 viral database are pre-installed in the container — no local installation required.

To launch a Codespace:

1. Click the green **Code** button at the top of this repository
2. Select the **Codespaces** tab
3. Click **Create codespace on main**

The container will build and open in your browser. This takes a few minutes the first time.

## Running locally

If you prefer to run the tutorial locally, create a conda environment using the provided `environment.yaml`:

```
conda env create -f environment.yaml
conda activate kraken2-tutorial
```

You will also need to download the Kraken2 k2_viral database:

```
mkdir -p ~/kraken2-db
wget -qO- https://genome-idx.s3.amazonaws.com/kraken/k2_viral_20240112.tar.gz | tar -xz -C ~/kraken2-db/
export KRAKEN2_DB=~/kraken2-db
```
