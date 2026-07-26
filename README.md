# J-space on Apple Silicon

用 MLX 在本地復現 Anthropic 的 Jacobian lens,model 是 Qwen3.6-27B-4bit。
論文:[Verbalizable Representations Form a Global Workspace in Language Models](https://transformer-circuits.pub/2026/workspace/index.html)

- 簡報,19 頁中譯:[`assets/jspace_report.pdf`](assets/jspace_report.pdf)
- Notebook,含輸出:[`jacobian_space_from_scratch_qwen36-27b.ipynb`](jacobian_space_from_scratch_qwen36-27b.ipynb)([PDF](assets/jspace_notebook.pdf))
- 互動版,同一顆 model:<https://www.neuronpedia.org/qwen3.6-27b/jlens>

## 自己跑

`models/Qwen3.6-27B-4bit/` 底下要放權重([`mlx-community/Qwen3.6-27B-4bit`](https://huggingface.co/mlx-community/Qwen3.6-27B-4bit))和 lens `jlens.npz`([`neuronpedia/jacobian-lens`](https://huggingface.co/neuronpedia/jacobian-lens))。然後 `uv sync`,開 notebook。
