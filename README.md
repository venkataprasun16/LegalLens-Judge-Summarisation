# LegalLens: Legal Judgment Summarisation

**CS787 course project** exploring rhetorical-role information for summarising Indian legal judgments. The project uses OpenNyAI's pretrained tagger to label judgment sentences, then compares LED summarisation with and without those labels.

## Repository

```text
Base Papers/                    Research papers and reference material
Rhetorical Role Classification/ Sentence-level role tagging notebooks, scripts, and results
Summarisation/                  LED training and evaluation notebooks and results
README.md
```

## Workflow

1. **Tag judgments:** notebooks in `Rhetorical Role Classification/` run OpenNyAI's pretrained rhetorical-role component and save predicted sentence labels. These are model-generated labels, not manually verified ground truth; the tag counts are not accuracy or F1 scores.
2. **Train and compare summaries:** notebooks in `Summarisation/` fine-tune LED (`allenai/led-base-16384`) on plain judgments for a baseline and on judgments augmented with role tags. The tagged run uses LoRA; the baseline fine-tunes LED directly.
3. **Evaluate:** the summarisation notebooks report ROUGE and BERTScore on their configured held-out judgments. See each folder's README and `Results/` for run details and saved results.

LED is a long-document encoder–decoder Transformer. Its encoder uses local sliding-window attention with global attention for selected tokens, and its decoder generates the summary. See the [Longformer paper](https://arxiv.org/abs/2004.05150).

## Running the notebooks

The notebooks are designed for Kaggle and/or the local OpenNyAI environment described in `Rhetorical Role Classification/README.md`. Attach the required IN-Abs data and tagged files as inputs, check the path/configuration cells, and run cells in order. LED fine-tuning is best run with a Kaggle GPU. The corpus and trained model artifacts are not included in this repository; Kaggle's `/kaggle/working` files are temporary, so download needed outputs before the session ends.

## Data and result notes

The experiments use IN-Abs judgment–summary pairs, with OpenNyAI-predicted rhetorical labels for the tagged summarisation run. Dataset access and use must follow the corpus terms. Results depend on the exact test IDs, preprocessing, and notebook settings; consult the per-folder READMEs before comparing reported scores.

## References

See [`Base Papers/`](Base%20Papers/) and [`Base Papers/paper_links.json`](Base%20Papers/paper_links.json) for the papers and project reference materials used in this work.
