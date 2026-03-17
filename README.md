# conference-tutorials-2026

Tools tutorials for the first Imageomics Conference workshop: **Designing for Discovery: Shaping Imageomics Tools for Biologists**.

**Dates:** April 16–17, 2026 | **Location:** The Ohio State University, Columbus, OH

## Contributing a Tutorial

Tutorial markdown files live in `docs/tutorials/`. See [`TUTORIAL_TEMPLATE.md`](TUTORIAL_TEMPLATE.md) for the authoring guide.

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
