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

### CyVerse Session

Launch a Cloud Shell session on CyVerse (click the badge): <a href="https://de.cyverse.org/apps/de/5f2f1824-57b3-11ec-8180-008cfa5ae621/launch?saved-launch-id=46c0c69f-0990-439f-929d-d5d44a3c36fb" target="_blank" rel="noopener noreferrer"><img src="https://de.cyverse.org/Powered-By-CyVerse-blue.svg"></a>

Once you are authenticated, this will take you to the CloudShell session configuration. 

- On the page for "Step 1: Analysis Info", click "Next →".
- On the page for "Step 2: Analysis Parameters", do the following:
  - In the "Input Folder" field, click the "Browse" button.
  - In the "Path" field, paste the following:
    ```
    /iplant/home/thompsonmj/imageomics-conference-2026
    ``` 
    and click Enter. 
  - Check the box next to `tutorials-data/`.
  - Click the "OK" button.
- Back on "Step 2: Analysis Parameters", click "Next →".
- On "Step 3: Advanced Settings (optional)", click "Next →".
- On "Step 4: Launch or Save", click "▶️ Launch Analysis".
- On the page that shows "Submitted", wait for it to say "Running".
- Click "Go to Analysis".

You are now running a CyVerse CloudShell session.

Create a directory to work in:
```bash
mkdir this-session && cd this-session
```

### Environment and Install

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

### Example Images

And copy images to test with from shared storage into your local container storage:
```bash
cp -R ~/data-store/data/input/imageomics-conference-2026/tutorials-data/pybioclip \
  ~/data-store/this-session
```
Ensure permissions are set appropriately so we can work with the data:
```bash
chmod --recursive u+rw ~/data-store/this-session
```

This will place images into the following location for us to practice with:
```bash
~/data-store/this-session/pybioclip/images
```
with contents:
```bash
ls -1 ~/data-store/this-session/pybioclip/images/
Actinostola-abyssorum.png
Amanita-muscaria.jpeg
Carnegiea-gigantea.png
coral-snake.jpeg
Felis-catus.jpeg
milk-snake.png
Onoclea-hintonii.jpg
Onoclea-sensibilis.jpg
Phoca-vitulina.png
Sarcoscypha-coccinea.jpeg
Ursus-arctos.jpeg
```


## Background

A foundation model is a large model trained on broad data that can be adapted to many downstream tasks without task-specific training. BioCLIP is a foundation model for biology: trained on millions of organism images paired with taxonomic labels, it learns general visual representations of the living world that can be applied to classification, embedding, and retrieval out of the box.

## CLI Examples

### Classify Focal Species in Images

`pybioclip` allows us to use BioCLIP models efficiently to classify the focal species:

- in an individual image
- in a group of images
- view prediction outputs in the terminal
- save those predictions to a file

The first time it is executed, it retrieves the necessary files for prediction:
```bash
bioclip predict pybioclip/images/Phoca-vitulina.png
```

```console
open_clip_config.json: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 534/534 [00:00<00:00, 2.49MB/s]
open_clip_model.safetensors: 100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 1.71G/1.71G [00:05<00:00, 314MB/s]
embeddings/txt_emb_species.npy: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 2.66G/2.66G [00:12<00:00, 214MB/s]
embeddings/txt_emb_species.json: 100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 91.6M/91.6M [00:01<00:00, 74.7MB/s]
100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 1/1 [00:06<00:00,  6.22s/images]
```
And this yields the prediction:
```bash
file_name,kingdom,phylum,class,order,family,genus,species_epithet,species,common_name,score
pybioclip/images/Phoca-vitulina.png,Animalia,Chordata,Mammalia,Carnivora,Phocidae,Phoca,vitulina,Phoca vitulina,Harbour Seal,0.8855793476104736
pybioclip/images/Phoca-vitulina.png,Animalia,Chordata,Mammalia,Carnivora,Phocidae,Phoca,largha,Phoca largha,Spotted seal,0.04246024042367935
pybioclip/images/Phoca-vitulina.png,Animalia,Chordata,Mammalia,Carnivora,Phocidae,Ommatophoca,rossii,Ommatophoca rossii,Ross seal,0.040906891226768494
pybioclip/images/Phoca-vitulina.png,Animalia,Chordata,Mammalia,Carnivora,Phocidae,Pusa,hispida,Pusa hispida,Ringed seal,0.012681455351412296
pybioclip/images/Phoca-vitulina.png,Animalia,Chordata,Mammalia,Carnivora,Phocidae,Halichoerus,grypus,Halichoerus grypus,Grey Seal,0.00508195860311389
```
Subsequent predictions are comparatively quicker:
```bash
bioclip predict pybioclip/images/Sarcoscypha-coccinea.jpeg 
Warning: You are sending unauthenticated requests to the HF Hub. Please set a HF_TOKEN to enable higher rate limits and faster downloads.
WARNING:huggingface_hub.utils._http:Warning: You are sending unauthenticated requests to the HF Hub. Please set a HF_TOKEN to enable higher rate limits and faster downloads.
100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 1/1 [00:04<00:00,  4.59s/images]
file_name,kingdom,phylum,class,order,family,genus,species_epithet,species,common_name,score
pybioclip/images/Sarcoscypha-coccinea.jpeg,Fungi,Ascomycota,Pezizomycetes,Pezizales,Sarcoscyphaceae,Sarcoscypha,coccinea,Sarcoscypha coccinea,skarlagen-pragtbæger,0.3211705982685089
pybioclip/images/Sarcoscypha-coccinea.jpeg,Fungi,Ascomycota,Pezizomycetes,Pezizales,Sarcoscyphaceae,Sarcoscypha,macaronesica,Sarcoscypha macaronesica,,0.24325676262378693
pybioclip/images/Sarcoscypha-coccinea.jpeg,Fungi,Ascomycota,Pezizomycetes,Pezizales,Sarcoscyphaceae,Sarcoscypha,excelsa,Sarcoscypha excelsa,,0.1801837533712387
pybioclip/images/Sarcoscypha-coccinea.jpeg,Fungi,Ascomycota,Pezizomycetes,Pezizales,Sarcoscyphaceae,Sarcoscypha,jurana,Sarcoscypha jurana,,0.1774240881204605
pybioclip/images/Sarcoscypha-coccinea.jpeg,Fungi,Ascomycota,Pezizomycetes,Pezizales,Sarcoscyphaceae,Sarcoscypha,humberiana,Sarcoscypha humberiana,,0.015541158616542816
```
The warning that occurs is from a cache validation check. All of the necessary files should be stored locally, so this is just noise that isn't needed. We can suppress it by setting:
```bash
export HF_HUB_OFFLINE=1
```
Now, predictions should be quieter for the rest of the session.

We can specify individual images we would like predictions for from this list:
```bash
bioclip predict \
  pybioclip/images/Sarcoscypha-coccinea.jpeg pybioclip/images/Ursus-arctos.jpeg
```
Or we can submit all of the images present for prediction. Let's also place the predicted outputs into a file called `predictions.csv`:
```bash
bioclip predict --output predictions.csv pybioclip/images/*
```

You man inspect the outputs with:
```bash
cat predictions.csv
```

It's too much to fit in the screen. Let's try again with only the top prediction:
```bash
bioclip predict --output predictions.csv --k 1 pybioclip/images/*
```

This yields a more manageable list for today:
```bash
cat predictions.csv 
file_name,kingdom,phylum,class,order,family,genus,species_epithet,species,common_name,score
pybioclip/images/Actinostola-abyssorum.png,Animalia,Cnidaria,Anthozoa,Actiniaria,Actiniidae,Cribrinopsis,similis,Cribrinopsis similis,,0.2903341054916382
pybioclip/images/Amanita-muscaria.jpeg,Fungi,Basidiomycota,Agaricomycetes,Agaricales,Amanitaceae,Amanita,muscaria,Amanita muscaria,Fly agaric,0.8807751536369324
pybioclip/images/Carnegiea-gigantea.png,Plantae,Tracheophyta,Magnoliopsida,Caryophyllales,Cactaceae,Carnegiea,gigantea,Carnegiea gigantea,Saguaro,0.8612735271453857
pybioclip/images/coral-snake.jpeg,Animalia,Chordata,Squamata,,Elapidae,Micrurus,hemprichii,Micrurus hemprichii,Hemprich's coral snake,0.08613336831331253
pybioclip/images/Felis-catus.jpeg,Animalia,Chordata,Mammalia,Carnivora,Felidae,Felis,silvestris,Felis silvestris,Wildcat,0.26516351103782654
pybioclip/images/milk-snake.png,Animalia,Chordata,Squamata,,Colubridae,Lampropeltis,triangulum,Lampropeltis triangulum,Eastern milksnake,0.532394528388977
pybioclip/images/Onoclea-hintonii.jpg,Plantae,Tracheophyta,Magnoliopsida,Asterales,Asteraceae,Polymnia,laevigata,Polymnia laevigata,,0.07937052100896835
pybioclip/images/Onoclea-sensibilis.jpg,Animalia,Arthropoda,Insecta,Diptera,Anthomyiidae,Chirosia,gleniensis,Chirosia gleniensis,,0.03540492802858353
pybioclip/images/Phoca-vitulina.png,Animalia,Chordata,Mammalia,Carnivora,Phocidae,Phoca,vitulina,Phoca vitulina,Harbour Seal,0.8855838775634766
pybioclip/images/Sarcoscypha-coccinea.jpeg,Fungi,Ascomycota,Pezizomycetes,Pezizales,Sarcoscyphaceae,Sarcoscypha,coccinea,Sarcoscypha coccinea,skarlagen-pragtbæger,0.32118508219718933
pybioclip/images/Ursus-arctos.jpeg,Animalia,Chordata,Mammalia,Carnivora,Ursidae,Ursus,arctos,Ursus arctos,Brown Bear,0.8381649851799011
```

### Embed Images
<!--
Follow here:
https://imageomics.github.io/pybioclip/command-line-tutorial/#create-embeddings
-->
An embedding is a numeric vector in high-dimensional space. It can be thought of as a model's conception of an image, such as it encodes the model's perception of the image's features based on what it has learned during training.

Calculate the BioCLIP embeddings for each of the images at-hand:
```bash
bioclip embed --output embeddings.json pybioclip/images/*
```

Once this completes, you will have a JSON file with an embedding vector for each image:
```bash
{
  "model": "hf-hub:imageomics/bioclip-2",
  "embeddings": {
    "pybioclip/images/Actinostola-abyssorum.png": [
      3.226287364959717,
      "...768 total"
    ],
    "pybioclip/images/Amanita-muscaria.jpeg": [
      2.0568881034851074,
      "...768 total"
    ],
   # ...
    "pybioclip/images/Sarcoscypha-coccinea.jpeg": [
      1.7511709928512573,
      "...768 total"
    ],
    "pybioclip/images/Ursus-arctos.jpeg": [
      4.4644927978515625,
      "...768 total"
    ]
  }
}
```

Embeddings are extremely useful for many downstream tasks.

In the next deep dive, we will talk more about what embeddings are and what you can do with them.

### Classify Focal Species from a Targeted List
<!--
Follow here:
https://imageomics.github.io/pybioclip/command-line-tutorial/#predict-from-a-list-of-classes
-->
You may also specify a list of classes (labels) that you would like the model to select among:

For example, you will likely see improved accuracy by selecting a subset of options that you believe is most relevant to the image contents rather than the entire tree of life. The classes can be any text label:
```bash
bioclip predict --cls cat,bird,bear pybioclip/images/Ursus-arctos.jpeg
```

### Classify Focal Species from a Subset of TreeOfLife Taxa

To find what taxa are available for open-ended classification:

```bash
bioclip list-tol-taxa > BioCLIP-2-taxa.txt
```

!!! tip

    You can specify the model of interest, e.g., for the original BioCLIP model with `--model hf-hub:imageomics/bioclip` before the carat.

## Python API examples
To demonstrate use of the Python API in a notebook environment, follow the link below to open the pybioclip documentation, and then the TOL-Subsetting.ipynb notebook.

<https://imageomics.github.io/pybioclip/python-tutorial/#predict-using-a-subset-of-the-treeoflife>

### Interpretability Methods
Interpretability in AI is a huge field of research, and there are many approaches. Some of which will be covered in detail in coming sessions here which are capable of more advanced and detailed identification of discriminative traits.

For now though, we will introduce the topic with Grad-CAM in the GradCamExperiment.ipynb notebook in the pybioclip documentation here:

<https://imageomics.github.io/pybioclip/python-tutorial/#experiment-with-grad-cam>

### Advanced Topics

You may notice examples where pybioclip gets the prediction wrong. Occasionally, way wrong. A number of methods exist to improve prediction accuracy, and a simple first step for this is to train a lightweight classifier on the embeddings that you may generate with pybioclip.

To introduce this topic, open the FewShotSVM.ipynb notebook in the documentation location below to see how a Support Vector Machine can be used to improve classification results.

<https://imageomics.github.io/pybioclip/python-tutorial/#lightweight-classifiers>

## Summary

pybioclip is a fast way to get started with a strong set of models: species classification, embeddings, and interpretability, all without writing any model code. As we saw in the notebooks, even a simple SVM on top of these embeddings can substantially improve results for specific use cases.
When you need more control, the natural next step is working directly with PyTorch and the model internals. But pybioclip is a great place to gain momentum quickly, and, depending on the application, possibly serve as a complete solution.
