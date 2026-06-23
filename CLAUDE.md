# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

University assignment (UFSC, *Sistemas Inteligentes*, Prof. Nathalia da Cruz Alves) — **Trabalho Prático 2 — Redes Neurais**. The full brief lives only inside `TrabalhoPrático2-RedesNeurais.docx`; the key requirements are distilled below so you don't have to re-extract it.

Goal: classify images from **CIFAR-10** (32×32 RGB, 10 classes). The graded deliverable is a **single Jupyter notebook (`.ipynb`) with all cell outputs saved** (plots, metrics, tables), plus a ≤10-minute explanatory video (not produced here). Suggested stack: **TensorFlow + Keras** run on **Google Colab**. Deadline: **2026-06-24**. Group of up to 2.

CIFAR-10 may be swapped for another dataset only with the professor's prior approval, and it must have ≥10 classes.

## The notebook is the architecture

The grade is driven by four sequential parts that must appear in the notebook **in this order**, each with its own analysis written out (in **Portuguese** — the course and the audience are Portuguese-speaking):

1. **Simple MLP** — fully-connected only, *no convolutional layers*. Train, then plot train/val **loss and accuracy per epoch**. The written analysis must explain how loss vs. accuracy are computed and why the two curves are not necessarily proportional, and judge whether the result generalizes.
2. **CNN + data augmentation** — convolutional model with augmentation. Same per-epoch train/val loss & accuracy plots. The analysis must address what augmentation implies about the amount of data per class, plus generalization.
3. **Hyperparameter search** — a procedure that sweeps combinations over defined ranges (e.g. number of layers, neurons, learning rate). Output a **comparative table** of combinations → results, report the best values found, and explain *which* parameters mattered most and why.
4. **Final test-set evaluation** — take the single best model from part 3, report final test accuracy & loss, and discuss generalization to unseen data.

When adding code, preserve this four-part structure and keep each part's plots/tables/written analysis together with its model. Re-run cells so outputs are saved before considering a part done — empty outputs fail the deliverable.

## Environment & running

- This machine has **Python 3.14.6 but no TensorFlow/Keras/PyTorch and no Jupyter runtime** (only `jupyter_core`). Don't assume `import tensorflow` works locally. Python 3.14 is also newer than TF's typical support window — verify wheel availability before installing locally.
- Primary intended runtime is **Google Colab** (free GPU, datasets/libraries preinstalled). Treat the notebook as Colab-first.
- To run a notebook headlessly once a runtime exists: `jupyter nbconvert --to notebook --execute --inplace <file>.ipynb` (requires `nbconvert` + `ipykernel`, currently not installed).
- The repo currently has no committed code and no commits yet.

## Academic constraints (these are graded, not optional)

- **Plagiarism → grade 0 for everyone involved.** Any reused theory, code, or repository must be cited. If a generative-AI tool is used, cite which tool and for what purpose.
