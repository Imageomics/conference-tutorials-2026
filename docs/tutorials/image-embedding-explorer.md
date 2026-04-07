# Image Embedding Explorer

> Explore image embeddings interactively—try different models, projections, and see which images cluster together or apart.

## Learning Objectives

By the end of this tutorial, you will be able to:

1. _TBD_
2. _TBD_
3. _TBD_

## Prerequisites

- Python >= 3.10
- _TBD_

## Setup

In the [CyVerse Discovery Environmnet](https://de.cyverse.org/dashboard), launch "Jupyter Lab PyTorch GPU".

Wait for "Launching VICE app: jupyter-lab-pytorch-gpu" to complete.

Click "Terminal" to start a new terminal session. This should place you in `~/data-store`.

Clone the repository for the embedding explorer application:
```bash
git clone https://github.com/Imageomics/emb-explorer.git
```

Enter the repository directory:
```bash
cd emb-explorer
```

Install `uv` for fast environment setup:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Reload your shell environment:
```bash
source $HOME/.local/bin/env
```

Create a virtual environment with an appropriate Python version:
```
uv venv --python 3.12
```

Activate the virtual environment:
```bash
source .venv/bin/activate
```

Install the package using CUDA 13.x (available on CyVerse):
```bash
uv pip install -e ".[gpu-cu13]"
```

Launch the app:
```bash
streamlit run apps/embed_explore/app.py \
  --server.headless true \
  --server.enableCORS false \
  --server.enableXsrfProtection false
```
or
```bash
streamlit run apps/precalculated/app.py \
  --server.headless true \
  --server.enableCORS false \
  --server.enableXsrfProtection false
```

In your brower's URL bar, you will see something like "https://a12345abc.cyverse.run/lab". Note the prefix "a12345abc".

In another browser tab, paste "https://placeholder.cyverse.run/proxy/8501/" (it will be 404 Not Found for now).

Replace the proxy URL prefix "placeholder" with that of your Jupyter Lab session (i.e. "https://a12345abc.cyverse.run/proxy/8501/" from the example).

The app UI should now be visible in that browser tab. Set paths relative to the repository root you cloned to.


## Background

_TBD_

## Steps

_TBD_

## Summary

_TBD_

