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

This tells you what percentage of reads were **not** assigned to any taxon in the k2_viral database. For this sample, approximately 91% of reads are unclassified. That is entirely expected — this is a metatranscriptomic wastewater sample sequencing all RNA present, and the k2_viral database only contains viral genomes. Most reads originate from bacteria, bacteriophage hosts, and other organisms not represented in this database.

## What's in this sample?

Look at the family-level entries in the report and sort by abundance:

`sort -k1 -rn sample.report | head -30`

You should see:
- **Caudoviricetes** (Jouyvirus, Tequatrovirus, Peduovirus) — dsDNA bacteriophages; consistently the most abundant classified group in any wastewater sample
- **Tobamovirus / Virgaviridae** — this includes [Tomato brown rugose fruit virus (ToBRFV)](https://en.wikipedia.org/wiki/Tomato_brown_rugose_fruit_virus) and [Pepper Mild Mottle Virus (PMMoV)](https://doi.org/10.1128/AEM.01012-16), RNA plant viruses shed in human feces. The CASPER study uses PMMoV and ToBRFV as normalization markers to account for variation in fecal load across samples
- **Lenarviricota / Leviviricetes** — single-stranded RNA phages (such as MS2), another classic wastewater indicator
- **Naldaviricetes / Baculoviridae** — insect baculoviruses (you may see *Choristoneura fumiferana* granulovirus, used as a biopesticide). Their presence in the k2_viral report is a reminder that this database contains genomes from **all** viral kingdoms, not just human pathogens

Now filter to look specifically at **vertebrate-infecting virus** families:

`grep -E "Caliciviridae|Norovirus|Parvoviridae|Picornaviridae|Hantavir|Bunyaviral|Peribunyavir" sample.report`

This sample was collected on **December 23, 2025** in NYC — peak norovirus season — from hospital sewage where pathogen concentrations are far higher than in diluted municipal influent. The CASPER paper ([Justen et al. 2026](https://doi.org/10.64898/2026.03.05.26345726)) specifically notes that NYC hospital sites diverged from municipal patterns with elevated Picornaviridae. You should find:

- **Norovirus GII** (Caliciviridae) — ~40 reads; the leading cause of acute gastroenteritis worldwide, and one of the primary targets of wastewater surveillance. This is the most abundant human-infecting virus in this sample — consistent with December peak norovirus season and the concentrated hospital sewage source
- **Parvoviridae** — small ssDNA viruses that include human bocavirus and parvovirus B19, commonly shed in feces
- **Picornaviridae / Aichivirus** — a kobuvirus linked to shellfish-associated gastroenteritis outbreaks globally
- **Hantaviridae / Bunyavirales** — rodent-associated RNA viruses; their presence in a hospital setting may reflect local rodent activity, diet (rodent-derived food), or cross-contamination — a reminder that wastewater surveillance captures signals from many sources beyond active human infection, as discussed in Justen et al. 2026

This sample has among the highest norovirus fractions in the CASPER dataset for its sample size, making the signal clearly detectable even at 50,000 reads. As noted in Justen et al. 2026, vertebrate-infecting viruses typically represent only ~0.01% of all wastewater reads, but hospital grab samples — closer to the patient source and much less diluted — can show 10–100× higher fractions.

When you're ready, `cd ../Step3` to convert this report into a format that Krona can read!
