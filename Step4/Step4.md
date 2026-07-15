# Step 4: Generating an Interactive Krona Plot

We're in the final analysis step! Here you'll use `ktImportText` (part of KronaTools) to convert the Krona text file into a fully interactive HTML visualization.

A pre-computed copy of `sample.krona.txt` is here in `Step4/` for reference.

## Generate the Krona HTML

Run the following command:

`ktImportText sample.krona.txt -o sample.krona.html`

Options used:
- `sample.krona.txt` — the Krona-format input file from the previous step
- `-o sample.krona.html` — the output HTML file

This should complete almost instantly. You'll see a new file, `sample.krona.html`, appear in the directory.

## Open the visualization

In GitHub Codespaces, you can open the HTML file directly in the browser using the Live Server extension:
1. Right-click `sample.krona.html` in the file explorer
2. Select **Open with Live Server**

Alternatively, download the file to your local machine and open it in any browser — it's a self-contained file that works without an internet connection.

A pre-computed copy is also available as `Step4/sample.krona.html` if you'd like to open it directly.

## Explore the plot

The Krona chart shows the full taxonomic hierarchy as a sunburst. The innermost ring is the broadest level (e.g., Viruses), and each outer ring shows the next level of classification.

Try the following:
- **Click** on any segment to zoom in on that clade
- **Hover** over a segment to see read counts and percentages
- **Click the center** to zoom back out
- Use the **search box** (top right) to find a specific taxon by name

What are the most abundant viral families in this sample? Can you find SARS-CoV-2? Are there any unexpected viruses present?

## What's next?

Now that you've explored the taxonomic composition of a single sample, head to the conclusion for ideas on how to extend this workflow.

`cd ../Step5`
