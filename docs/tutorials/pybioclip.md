# pybioclip

> Classify focal species and generate image embeddings programmatically without complex ML infrastructure.

## Learning Objectives

By the end of this tutorial, you will be able to:

1. Use `pybioclip` to get both **embeddings** and **taxonomic predictions** from images.
2. Subset or otherwise geographically restrict taxonomic label options for **use-case-specific filtering**.
3. Create Grad-CAM visualizations for evaluating model attention (i.e., what part of the image is the model relying on for its determination?). 
4. Implement a **lightweight classifier for downstream applications** beyond taxonomic label prediction for use-case specificity *without fine-tuning*.

## Prerequisites

- **Python version:** >= 3.10 (versions compatible with [PyTorch](https://pytorch.org/get-started/locally/#linux-python))
- **Data:** All data used will be accessed through Hugging Face download from public repositories or provided in Cyverse.
- **Prior Knowledge:** Basic comfort with a terminal. No machine learning background required; concepts will be explained as they are mentioned.


## Setup

First, install [`uv`](https://docs.astral.sh/uv/) for efficient environment management:

```bash
# Cyverse install
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
```

Next, create and activate a virtual environment:

```bash
uv venv --python 3.12
source .venv/bin/activate
```

Finally, install `pybioclip`:

```bash
uv pip install pybioclip
```

## Background

_TBD_

## Steps

_TBD_

## Summary

_TBD_
