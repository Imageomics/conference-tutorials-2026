# Prompt-CAM

> Explore fine-grained trait distinctions between different specified species.

## Learning Objectives

By the end of this tutorial, you will be able to:

1. Understand how Prompt-CAM uses class-specific prompts injected into Vision Transformers to generate attention maps that reveal what a model focuses on for each species.
2. Use the provided notebook or demo to generate and compare attention maps across different species classes.
3. Assess whether the highlighted image regions correspond to biologically meaningful traits and understand how the model perceives shared or distinctive features between classes.

## Prerequisites

- No local setup is required for the hands-on portion of this tutorial. We will provide a ready-to-use Google Colab notebook so participants can focus on exploring the tool. We will provide some sample images to see the highlighted traits.
- Participants are welcome to bring organism images of interest. Some familiarity with species labels and biologically relevant traits will be helpful for interpreting results. 
- For those who would like to run the method locally after the workshop, installation instructions will also be provided (Python >= 3.10, conda recommended).

## Setup

- For the tutorial, participants may bring organism images of interest with species labels and upload them to Google Drive for use in Colab.
- We will begin with pretrained models and example datasets (e.g., CUB-200 birds) prepared by us so that attendees can immediately explore what Prompt-CAM produces and how to interpret the attention maps. After that, we will discuss how to adapt the method to their own data. Participants who want to explore further can follow the instructions in the [Prompt-CAM repository](https://github.com/Imageomics/Prompt_CAM).

## Background

Prompt-CAM is an fast interpretable transformer method published at CVPR 2025. It injects learnable class-specific prompts into pre-trained Vision Transformers (such as DINO and DINOv2) without modifying the underlying model architecture. These prompts cause the model's attention mechanism to highlight the image regions most relevant to each specified class, producing visual attention maps that make the model's reasoning transparent.

Unlike general saliency methods, Prompt-CAM is designed for fine-grained visual analysis: it can reveal which traits are shared between classes and which are distinctive, and it allows side-by-side comparison of how the same image is perceived differently depending on the target class. This makes it especially useful for biodiversity tasks where species look very similar and subtle morphological differences drive classification.

## Steps

1. Start with the provided Prompt-CAM Colab demo using a pretrained model (DINO or DINOv2) and an example dataset such as CUB-200 birds.
2. Select an organism image and one or more species classes to query. Observe how the attention maps shift depending on which class prompt is active.
3. Compare attention maps across two similar species to see which image regions the model treats as shared traits versus distinguishing traits.
4. Inspect the highlighted regions and ask whether they align with known field traits, such as bill shape, plumage pattern, or wing markings.
5. After seeing the provided workflow, learn what is needed to apply Prompt-CAM to new datasets, including preparing labeled images and running training with your own configuration.

## Summary

Prompt-CAM is an interpretable fine-grained visual analysis tool that requires no architectural changes to strong pre-trained Vision Transformers. By injecting class-specific prompts, it generates attention maps that show what the model focuses on for each species or category. The workflow is accessible through a Colab notebook and scales to custom datasets, making it a practical tool for understanding and validating model reasoning in biodiversity and ecological research.

