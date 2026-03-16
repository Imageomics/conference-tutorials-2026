# conference-tutorials-2026

Tools tutorials for the first Imageomics Conference workshop: **Designing for Discovery: Shaping Imageomics Tools for Biologists**.

**Dates:** April 16–17, 2026 | **Location:** The Ohio State University, Columbus, OH

## Tutorials

| Tutorial | Tool(s) |
|----------|---------|
| [pybioclip](docs/tutorials/pybioclip.md) | Programmatic species classification & embedding generation |
| [Embedding Explorer](docs/tutorials/embedding-explorer.md) | Interactive embedding space exploration |
| [BioCLIP Search Lite](docs/tutorials/bioclip-search-lite.md) | Nearest-neighbor image search across TreeofLife |
| [SST Segmentation](docs/tutorials/sst.md) | Specimen trait segmentation & automatic landmarking |
| [CAM Trait Extraction](docs/tutorials/cam-trait-extraction.md) | Prompt-CAM & Finer-CAM for species trait distinctions |
| [SAE](docs/tutorials/sae.md) | Common trait discovery across image collections |

## Tutorial Designers

See [`TUTORIAL_TEMPLATE.md`](TUTORIAL_TEMPLATE.md) for the tutorial authoring guide. Place your tutorial markdown in `docs/tutorials/`.

For inspiration: [Explore NEON biodiversity data using ecocomDP](https://www.neonscience.org/resources/learning-hub/tutorials/neon-biodiversity-ecocomdp-cyverse).

## Development

Requires Python >= 3.10. Install with [uv](https://docs.astral.sh/uv/) (recommended) or pip:

```bash
# With uv
uv venv
source .venv/bin/activate
uv pip install -e .

# Or with pip
python -m venv .venv
source .venv/bin/activate
pip install -e .
```

Preview the site locally at `localhost:8000`:

```bash
zensical serve
```

## Acknowledgments

The Imageomics Institute is supported by the National Science Foundation under Award No. 2118240.
