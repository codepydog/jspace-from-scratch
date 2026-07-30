# Jacobian Lens Demo

A compact MLX notebook for understanding Jacobian lens on Qwen3.6-27B.

[Open the notebook](jacobian_lens_demo.ipynb)

## What it covers

- One forward pass: text → token IDs → embeddings → logits → next token
- The residual stream
- Logit lens vs Jacobian lens
- A local J-lens direction computed with backpropagation
- Concept swap: `France → China`
- A two-hop intervention: `Eiffel Tower → France → Paris`

The notebook is written in Traditional Chinese.

## Requirements

- Apple Silicon
- 32 GB unified memory
- About 18 GB of free disk space
- Python 3.12+

Tested on an M4 Mac with 32 GB memory.

## Setup

Install the environment:

```bash
uv sync
```

Download the 4-bit model:

```bash
MODEL_DIR=models/Qwen3.6-27B-4bit

uv run hf download mlx-community/Qwen3.6-27B-4bit \
  --local-dir "$MODEL_DIR"
```

Download the precomputed Jacobian lens:

```bash
gh release download v0.2-fulldepth \
  --repo WeZZard/jlens-qwen36 \
  --pattern '*.npz.part-*' \
  --dir "$MODEL_DIR"

cat "$MODEL_DIR"/*.npz.part-* > "$MODEL_DIR/jlens.npz"
rm "$MODEL_DIR"/*.npz.part-*
```

Open `jacobian_lens_demo.ipynb`, select the project environment, and run the cells in order.

## Sources

- [Verbalizable Representations Form a Global Workspace in Language Models](https://transformer-circuits.pub/2026/workspace/index.html)
- [Anthropic Jacobian lens implementation](https://github.com/anthropics/jacobian-lens)
- [Precomputed Qwen3.6-27B lens](https://huggingface.co/neuronpedia/jacobian-lens)
- [Neuronpedia J-lens demo](https://www.neuronpedia.org/qwen3.6-27b/jlens)
