# SST Segmentation

> Segment traits from specimen images and apply automatic landmarking.

## Learning Objectives

By the end of this tutorial, you will be able to:

1. Understand how SST segments traits in a set of specimen images given just one annotated example.
2. Use the provided SST tutorial notebook to segment traits in butterfly images or your own images.
3. Use X-AnyLabeling to annotate a specimen image with the help of Segment Anything Model 2 (SAM 2), a powerful AI model.

## Prerequisites

- No local setup is required for the hands-on portion of SST. We provide a ready-to-use [tutorial notebook](https://colab.research.google.com/drive/1s3ncIFOlrUm3v0mBD4eCL2NFvE3EYIpk?usp=sharing). You can further explore SST in its [official repository](https://github.com/Imageomics/SST) if interested.
- You are welcome to bring your own specimen images of interest. To try SST on your own images, [X-AnyLabeling](https://github.com/CVHub520/X-AnyLabeling) is required for annotation. An installation guide is provided below in the **Setup** section. If you do not have your own images but would still like to try X-AnyLabeling, please install it as well. We provide some butterfly images [here](https://drive.google.com/drive/folders/1GHWp1gazg2I1PJVTpq-JquK2B9uMBXlj?usp=sharing) to annotate.

## Setup

You can either install X-AnyLabeling in CyVerse or on your local machine.

### 1. Using CyVerse

Follow the steps below to install SST and X-AnyLabeling in CyVerse.

**Step 0:** In the [CyVerse Discovery Environment](https://de.cyverse.org/dashboard), launch **Jupyter Lab PyTorch GPU**. Click "Go to Analysis" and wait for "Launching VICE app: jupyter-lab-pytorch-gpu" to complete.

**Step 1:** Click **Terminal** to open a new terminal session. This should place you in `~/data-store`. Create a working directory for fast local I/O:

```bash
mkdir this-session
cd this-session
```

**Step 2:** Install `uv` for fast environment setup:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
```

**Step 3:** Create and activate a virtual environment:

```bash
uv venv --python 3.12
source .venv/bin/activate
```

**Step 4:** Clone and install SST:

```bash
git clone https://github.com/Imageomics/SST
cd SST
export CUDA_HOME=/usr/local/cuda
uv pip install -e .
cd ..
```

**Step 5:** Clone and install SAM 2:

```bash
git clone https://github.com/CVHub520/segment-anything-2
cd segment-anything-2
uv pip install -e .
cd ..
```

**Step 6:** Clone and install X-AnyLabeling:

```bash
git clone https://github.com/CVHub520/X-AnyLabeling
cd X-AnyLabeling
uv pip install -e ".[gpu]"
cd ..
```

**Step 7:** Set up noVNC for browser access:

```bash
git clone https://github.com/novnc/noVNC
```

**Step 8:** Launch X-AnyLabeling:

```bash
QT_QPA_PLATFORM="vnc:size=1920x1080" xanylabeling &
sleep 2
cd noVNC
./utils/novnc_proxy --listen 6080 --vnc localhost:5900
```

**Step 9:** Open in your browser: take the prefix from your JupyterLab URL (e.g., `a2e70e809` from `https://a2e70e809.cyverse.run/lab`) and navigate to:

```bash
https://<your-prefix>.cyverse.run/proxy/6080/vnc.html
```

**Step 10:** In the noVNC side panel on the left, select `Gear icon -> Scaling mode -> Local scaling` to adjust the resolution.

### 2. Installing locally

Follow the steps below to install X-AnyLabeling on your local machine.

#### Option 1: Install from source (for GPU-enabled machine)

**Step 0:** If you do not have Conda, download and install Miniconda from the [official website](https://docs.anaconda.com/miniconda/).

**Step 1:** Choose a working directory and open the terminal. Create a new Conda environment with Python version `3.12`, and activate it:

```bash
conda create -n x-anylabeling-sam2 python=3.12 -y
conda activate x-anylabeling-sam2
```

**Step 2:** Follow the instructions [here](https://pytorch.org/get-started/locally/) to install PyTorch and TorchVision dependencies.

**Step 3:** Install SAM 2 on a GPU-enabled machine:

```bash
git clone https://github.com/CVHub520/segment-anything-2
cd segment-anything-2
pip install -e .
```

**Step 4:** Clone the repository of X-AnyLabeling:

```bash
cd ..
git clone https://github.com/CVHub520/X-AnyLabeling
cd X-AnyLabeling
```

**Step 5:** Install the dependencies for X-AnyLabeling (choose the version that fits for your machine):

```bash
pip install -U uv

# CPU [Windows/Linux/macOS]
uv pip install -e .[cpu]

# CUDA 12.x is the default GPU option [Windows/Linux]
uv pip install -e .[gpu]

# CUDA 11.x [Windows/Linux]
uv pip install -e .[gpu-cu11]
```

**Step 6:** After installation, you can verify it by running the following command:

```bash
xanylabeling checks   # Display system and version information
```

**Step 7:** After verification, you can run the application directly:

```bash
xanylabeling
```

#### Option 2: Download released package

Download a released package from [GitHub Releases](https://github.com/CVHub520/X-AnyLabeling/releases) and run it directly. It is CPU-only and may be slower when running.

## Background

SST achieves high-quality trait segmentation across specimen images using only one labeled image per species. Standard deep learning methods require extensive labeled images to train effective segmentation models. SST concatenates specimen images into a "pseudo-video" and reframes trait segmentation as a tracking task. By doing so, SST requires only one labeled image as the first frame and leverages video segmentation models such as SAM 2 to propagate segments to subsequent frames.

## Steps

### SST

The following steps are also detailed in the [tutorial notebook](https://colab.research.google.com/drive/1s3ncIFOlrUm3v0mBD4eCL2NFvE3EYIpk?usp=sharing).

1. Clone the SST repository and set up the environment.

2. Download the SAM 2 checkpoint.

3. Use provided butterfly images or upload your own images.

4. Define the support image, support mask, and query images, then run SST trait segmentation to segment the query images.

5. Visualize the results.

### X-AnyLabeling

1. Open X-AnyLabeling (see **Setup**).

2. In the left sidebar, click **Open Dir** (the first icon) and select the directory containing the images you want to label.

3. In the left sidebar, click **Auto Labeling** (the "AI" icon at the bottom). A **No Model** button will then appear on the top toolbar. Click it and select "Segment Anything 2.1 (Large)". *Note: If your machine has limited hardware resources, you may experience lag or freezing.* In such cases, please select a smaller Segment Anything 2.1 model, such as Base, Small, or Tiny.

4. Use **Point (Q)** to include an area, **Point (E)** to exclude an area, and **+Rect** to select an area within a bounding box. You can use **Clear (B)** to remove all current annotations. Combine these tools to precisely label your target areas. Once you finish an area, use **Finish (F)** to save your annotation. Each time you click **Finish (F)**, the annotation is automatically saved to a JSON file in the same directory as the image you are currently annotating. The JSON file will share the same filename as the image.

5. You can also click the **Move and edit the selected polygons** icon in the middle of the left sidebar to manually adjust the area points using the mouse.

Note: X-AnyLabeling saves model weights on your local machine. If you wish to delete them later, you can find them at the following default paths:

- Windows: `C:\Users\<Your User Name>\xanylabeling_data\models\`
- macOS: `/Users/<Your User Name>/xanylabeling_data/models/`
- Linux: `/home/<Your User Name>/xanylabeling_data/models/`
