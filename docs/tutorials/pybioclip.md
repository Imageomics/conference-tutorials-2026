# pybioclip

> Classify focal species and generate image embeddings programmatically without complex ML infrastructure.

## Learning Objectives

By the end of this tutorial, you will be able to:

1. Use `pybioclip` to get both **embeddings** and **taxonomic predictions** from images.
2. Subset or otherwise geographically restrict taxonomic label options for **use-case-specific filtering**.
3. Create Grad-CAM visualizations for evaluating model attention (i.e., what part of the image is the model relying on for its determination?). 
4. [Bonus] Implement a **lightweight classifier for downstream applications** beyond taxonomic label prediction for use-case specificity *without fine-tuning*.

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

_TBD_ : What is a Foundation Model?

## Steps

### Classify Focal Species in Images

`pybioclip` allows us to efficiently classify the focal species in multiple images with BioCLIP models and save those predictions to a file of our choice.

<!--
Follow here:
https://imageomics.github.io/pybioclip/command-line-tutorial/#predict-species-for-multiple-images-saving-to-a-file
-->

### Embed Images
<!--
Follow here:
https://imageomics.github.io/pybioclip/command-line-tutorial/#create-embeddings
-->

In the next section, we'll talk more about what embeddings are and what you can do with them.

### Classify Focal Species from a Targeted List
<!--
Follow here:
https://imageomics.github.io/pybioclip/command-line-tutorial/#predict-from-a-list-of-classes
-->

### Classify Focal Species from a Subset of TreeOfLife Taxa

First, one may ask what taxa are available:

```bash
bioclip list-tol-taxa > BioCLIP-2-taxa.txt
```

!!! tip

    You can specify the model of interest, e.g., for the original BioCLIP model with `--model hf-hub:imageomics/bioclip` before the carat.

<!--
Follow here (use Colab):
https://imageomics.github.io/pybioclip/python-tutorial/#predict-using-a-subset-of-the-treeoflife
-->

### Interpretability Methods
<!--
Follow here (use Colab):
https://imageomics.github.io/pybioclip/python-tutorial/#experiment-with-grad-cam
-->
- note on more advanced/more finely discriminative methods later


### Advanced Topics

Building on the topics covered today, ...
<!--
Follow here (Colab notebooks):
If there's time/Next steps/Learn more
https://imageomics.github.io/pybioclip/python-tutorial/#lightweight-classifiers
-->

_TBD_

## Summary

_TBD_
