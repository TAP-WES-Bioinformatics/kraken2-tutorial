# Step 2: Interpreting the Kraken2 Report

You should now have `sample.report` and `sample.kraken` in your Step1 directory. In this step, we'll focus on the report file, which gives us a human-readable summary of the taxonomic composition of our sample.

Pre-computed copies of these files are here in `Step2/` — if Kraken2 is still running or you'd like to compare your results, `cd` here and work with those files directly.

## The report format

Open `sample.report` in the file explorer, or view it in the terminal:

`cat sample.report`

The report has six tab-separated columns with no header:

| Column | Meaning |
|--------|---------|
| 1 | Percentage of reads covered by this clade (including all descendants) |
| 2 | Number of reads covered by this clade |
| 3 | Number of reads **directly** assigned to this taxon (not children) |
| 4 | Rank code (`U` unclassified, `R` root, `D` domain, `K` kingdom, `P` phylum, `C` class, `O` order, `F` family, `G` genus, `S` species) |
| 5 | NCBI taxonomy ID |
| 6 | Scientific name (indented to reflect taxonomic depth) |

The first two rows are always:
- `U` — all unclassified reads (reads that matched nothing in the database)
- `R` — root of the classified tree (all reads that were classified)

## Explore the data

Sort the report by column 1 (abundance) to find the most prevalent taxa:

`sort -k1 -rn sample.report | head -20`

Look at the rank codes in column 4. Which ranks do you see represented? Notice that the percentage in column 1 is cumulative — a family-level entry includes all its genera and species below it.

To see only species-level hits (`S` in column 4):

`awk '$4 == "S"' sample.report | sort -k1 -rn | head -20`

What viruses are most abundant in this sample? Are there any you recognize as commonly associated with wastewater surveillance?

## Unclassified reads

Look at the very first line of the report:

`head -n 1 sample.report`

This tells you what percentage of reads were **not** assigned to any taxon in the k2_viral database. For this sample, approximately 85% of reads are unclassified. That is entirely expected — this is a metatranscriptomic wastewater sample sequencing all RNA present, and the k2_viral database only contains viral genomes. Most reads originate from bacteria, bacteriophage hosts, and other organisms not represented in this database.

## What's in this sample?

Look at the family-level entries in the report and sort by abundance:

`sort -k1 -rn sample.report | head -30`

You should see:
- **Caudoviricetes** (Jouyvirus, Peduovirus, Tequatrovirus) — dsDNA bacteriophages; consistently the most abundant classified group in any wastewater sample
- **Tobamovirus / Virgaviridae** — this includes [Tomato brown rugose fruit virus (ToBRFV)](https://en.wikipedia.org/wiki/Tomato_brown_rugose_fruit_virus) and [Pepper Mild Mottle Virus (PMMoV)](https://doi.org/10.1128/AEM.01012-16), RNA plant viruses shed in human feces. The CASPER study uses PMMoV and ToBRFV as normalization markers to account for variation in fecal load across samples
- **Lenarviricota / Leviviricetes** — single-stranded RNA phages (such as MS2), another classic wastewater indicator
- **Naldaviricetes / Baculoviridae** — insect baculoviruses. Their presence in the k2_viral report is a reminder that this database contains genomes from **all** viral kingdoms, not just human pathogens

Now filter to look specifically at **human-infecting viruses**:

`grep -E "Influenza|Orthomyxoviridae|Cytomegalovirus|Astroviridae|Rotavirus|Aichi|Picornaviridae" sample.report`

This is where the early-detection idea becomes concrete. You did not tell Kraken2 to look for influenza, CMV, or enteric viruses — you classified against a broad viral database and asked what was there. In this 50,000-read slice you should find:

- **Influenza A virus (H3N2)** (Orthomyxoviridae) — ~5 reads. A recognizable human respiratory pathogen recovered without a flu-specific assay. At full CASPER depth (~1.5 billion read pairs), this Chicago CHI-B sample carried a large H3N2 signal; even a tiny subsample is enough to flag it
- **Human cytomegalovirus (HHV-5)** — ~32 reads. A common human herpesvirus shed by infected individuals; another pathogen you would only see if you weren't limited to a fixed PCR panel
- **Astroviridae / Rotavirus A / Aichivirus** — sparse enteric hits. Typical of municipal wastewater, and a reminder that vertebrate-infecting viruses are rare relative to phages and plant viruses

Vertebrate-infecting viruses typically represent only a tiny fraction of all wastewater reads — often on the order of ~0.01%. Deep untargeted sequencing is what makes those rare signals recoverable. The point of this tutorial sample is not that influenza is the *most* abundant virus here (phages and tobamoviruses dominate), but that a pathogen-agnostic classifier can still surface a recognizable human-infecting virus you weren't specifically targeting.

When you're ready, `cd ../Step3` to convert this report into a format that Krona can read!
