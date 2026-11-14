# Legal Judgment Summarisation

This module compares **LED-base-16384** on Indian legal judgments in two settings: a baseline trained on plain judgments, and a rhetorical version trained on judgments tagged with OpenNyAI sentence roles.

## Model and approach

LED is an encoder–decoder Transformer designed for long documents. Its encoder combines local sliding-window attention with global attention; the decoder generates the summary autoregressively. The baseline fine-tunes LED directly. The rhetorical run uses LoRA adapters and adds predicted rhetorical-role tags to the judgment text. Long inputs are chunked with overlap during rhetorical evaluation; baseline inputs are truncated to the configured source length.

Architecture reference: [Longformer: The Long-Document Transformer](https://arxiv.org/abs/2004.05150).

## Notebooks

Run the notebooks in `src/` on Kaggle with a GPU accelerator:

- `LED_Baseline.ipynb` — fine-tunes LED on plain judgment–summary pairs.
- `LED_Rhetorical.ipynb` — fine-tunes LED with OpenNyAI rhetorical tags using LoRA.

The notebooks install/check their Python dependencies. Attach the required data as Kaggle inputs and follow the path/configuration cell near the beginning of each notebook.

## Inputs and outputs

Both runs need judgment–summary pairs. The rhetorical run additionally needs the tagged judgments (`tagged.jsonl` and, where used, `sentence_roles.csv`) aligned with the original judgments and reference summaries. The dataset itself is not stored in this repository.

Each notebook writes generated summaries, evaluation metrics, and model artifacts to `/kaggle/working`. Kaggle working files are temporary; download the outputs or save a notebook version with outputs before the session ends. The rhetorical model artifact is a LoRA adapter and needs the LED base model to load.

## Reported test results

Scores below are from the saved 100-judgment evaluations. ROUGE and BERTScore are reported as F1 except BERTScore, which lists precision, recall, and F1 separately.

| Run | ROUGE-1 | ROUGE-2 | ROUGE-L | BERTScore P | BERTScore R | BERTScore F1 |
|---|---:|---:|---:|---:|---:|---:|
| LED baseline | 0.3280 | 0.1664 | 0.1920 | 0.8608 | 0.8303 | 0.8451 |
| LED with rhetorical tags | 0.4001 | 0.2065 | 0.2006 | 0.8556 | 0.8295 | 0.8421 |

See `Results/` for the saved score files and notebook screenshots. The runs use 100 test judgments; ensure the same case IDs and evaluation settings are used before making a strict head-to-head comparison.
