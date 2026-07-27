# J-space from scratch - Qwen3.6-27B-4bit

用 MLX 在 Apple Silicon 上從頭復現 Anthropic 的 Jacobian lens。
Notebook 含輸出:[`jacobian_lens.ipynb`](jacobian_lens.ipynb) · [PDF](assets/jspace_notebook.pdf)

前三節是十分鐘的 demo:架構、原理、用「法國的首都」算一次。附錄 A 到 I 是實作細節和
論文五個 workspace 面向的完整實驗,都能重跑。

## Source

- 論文:[Verbalizable Representations Form a Global Workspace in Language Models](https://transformer-circuits.pub/2026/workspace/index.html)
- Blog:[A global workspace in language models](https://www.anthropic.com/research/global-workspace) · [中譯簡報](assets/jspace_report.pdf)
- 官方 code:[anthropics/jacobian-lens](https://github.com/anthropics/jacobian-lens)
- Demo:<https://www.neuronpedia.org/qwen3.6-27b/jlens>

## How to run local

### 配備

- Apple Silicon,統一記憶體 32 GB(實測 M4 / 32 GB / macOS 26)
- 磁碟 18 GB:權重 15 GB + lens 3.1 GB
- Python 3.13、mlx 0.32、mlx-vlm 0.6.4

### Model download

```bash
uv sync
D=models/Qwen3.6-27B-4bit

uv run hf download mlx-community/Qwen3.6-27B-4bit --local-dir $D

gh release download v0.2-fulldepth --repo WeZZard/jlens-qwen36 \
  --pattern '*.npz.part-*' --dir $D
cat $D/*.npz.part-* > $D/jlens.npz && rm $D/*.part-*
```

notebook 讀的 lens 格式是 `{J_<layer>: (d,d) fp16}` 加上 `__source_layers__` / `__n_prompts__` metadata,別的來源要自己對一下 key。

### Run

開 [`jacobian_lens.ipynb`](jacobian_lens.ipynb),Run All。
