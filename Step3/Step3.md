# Step 3: Converting the Report for Krona

In this step, we'll reshape the Kraken2 report into the format that [KronaTools](https://github.com/marbl/Krona) expects, using the `kreport2krona.py` script from [KrakenTools](https://github.com/jenniferlu717/KrakenTools).

A pre-computed copy of `sample.report` is here in `Step3/` for reference.

## What is Krona?

Krona is a visualization tool that creates interactive, multi-layered pie charts (sunburst plots) representing hierarchical data — perfect for showing the nested structure of taxonomic classifications. The final output is a self-contained HTML file you can open in any browser.

## Convert the report

Run the following command to transform the Kraken2 report into a Krona-compatible text file:

`kreport2krona.py -r sample.report -o sample.krona.txt`

Options used:
- `-r sample.report` — input Kraken2 report
- `-o sample.krona.txt` — output file in Krona text format

## Inspect the output

Take a look at the Krona text file:

`head -n 5 sample.krona.txt`

Each line represents one taxon's contribution to the sample. The format is:

```
<count>  <rank1>  <rank2>  <rank3>  ...
```

The first column is the number of reads **directly assigned** to that taxon (column 3 from the Kraken2 report). The remaining columns are the taxonomic path from the broadest rank down to that taxon, tab-separated. Krona uses these paths to build the nested hierarchy in the sunburst plot.

For example, a line like:

```
5	k__Viruses	p__Negarnaviricota	c__Insthoviricetes	o__Articulavirales	f__Orthomyxoviridae	g__Alphainfluenzavirus	s__Alphainfluenzavirus_influenzae
```

tells Krona that 5 reads were directly assigned to Influenza A — nested inside its full lineage. That's the early-detection signal: a human respiratory virus recovered from an untargeted wastewater metagenome.

When you're ready, `cd ../Step4` to generate the interactive plot!
