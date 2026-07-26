# J-space on Apple Silicon — Jacobian Lens 從頭復現

用 MLX 在 Apple Silicon 上,從頭把 Anthropic 的 **Jacobian lens / J-space** 方法
(論文 *Verbalizable Representations Form a Global Workspace in Language Models*)跑一遍。
模型用 **Qwen3.6-27B-4bit**——[neuronpedia 互動版](https://www.neuronpedia.org/qwen3.6-27b/jlens) 用的同一顆。

整個 repo 精簡到只有一份東西要看:

## 📓 [`jacobian_space_from_scratch_qwen36-27b.ipynb`](jacobian_space_from_scratch_qwen36-27b.ipynb) — 讀這個就夠

一份自足的 notebook,把方法每一塊都**明碼**寫出來(沒有藏在任何 package 裡):

> residual stream → logit lens → 用 `mx.vjp` 從零算 Jacobian $J_\ell$ →
> 載入公開 lens → readout(logit vs Jacobian)→ J-space 稀疏分解 → coordinate swap 因果測試

關鍵發現:語言模型的中間層裡,答案往往**比輸出層更早、更清楚地成形**——logit lens 看不到,Jacobian lens 看得到(§6 裡 Jacobian lens 早 logit lens 約 2–3 層讀到答案:`The capital of France is` 在 L55 就給 `Paris` 0.90 的信心,logit lens 這時還在 0.09)。

## 🔗 互動版 / 完整實驗 — neuronpedia

不想在本地跑 27B?完整互動版在 neuronpedia,可直接玩跨語言、兩跳推理、以及「模型想的跟說的不一樣」的例子:

**<https://www.neuronpedia.org/qwen3.6-27b/jlens>**

## 📑 [`assets/jspace_report.pdf`](assets/jspace_report.pdf) — 簡報

Anthropic 那篇公開文章的中譯簡報,19 頁,附原文的圖。notebook 是實作,這份是先把方法跟結論講清楚。

## 本地跑 notebook

需要兩個大檔(都不進 git),放到 `models/Qwen3.6-27B-4bit/` 底下:

1. **模型**:`Qwen3.6-27B-4bit` 的 MLX 權重(例如 [`mlx-community/Qwen3.6-27B-4bit`](https://huggingface.co/mlx-community/Qwen3.6-27B-4bit))。
2. **lens**:`jlens.npz`——格式是 `{J_<layer>: (d,d) fp16}` 加上 `__source_layers__ / __n_prompts__ / __d_model__` metadata。lens 綁 architecture、不綁 quant(全精度擬合的 lens 套在 int4 activation 上照樣工作)。來源見下方 References。

然後:

```bash
uv sync                 # 只需要 mlx / mlx-vlm / numpy
# 用 Jupyter 或 VS Code 開 jacobian_space_from_scratch_qwen36-27b.ipynb 執行即可
```

第一次跑約幾分鐘(載入 27B 權重 + §4 的 VJP 最慢 ~20s)。notebook 載入時會自動套上 mlx-vlm 的 #1548 RMSNorm 修正(否則 qwen3_5 輸出會是亂碼)。

## References

**論文 / 官方**
- 論文 *Verbalizable Representations Form a Global Workspace in Language Models* — <https://transformer-circuits.pub/2026/workspace/index.html>
- 官方 code(PyTorch, Apache-2.0)— <https://github.com/anthropics/jacobian-lens>
  readout `softmax(W_U·norm(J_ℓ·h_ℓ))`;fit 跳開頭 16 + 最後 token,penultimate→final,no whitening

**Pre-fitted 27B lens**(取得 `jlens.npz` 用)
- `neuronpedia/jacobian-lens`(`.pt`, MIT, 含 Qwen3.6-27B n=1000)— <https://huggingface.co/neuronpedia/jacobian-lens>
- `WeZZard/jlens-qwen36`(現成的 MLX `.npz` + 手寫 Metal GDN backward)— <https://github.com/WeZZard/jlens-qwen36>

**解讀與獨立復現**
- David Louapre, *J-Space: Yet Another LLM Mind Reader?* — <https://huggingface.co/blog/dlouapre/j-space>
- Neel Nanda 團隊, *A Review of Anthropic's Global Workspace Paper*(Qwen3.6-27B 獨立重現與批評)— <https://www.lesswrong.com/posts/zFJ3ZdQwrTWE9jT5S/a-review-of-anthropic-s-global-workspace-paper>
- nostalgebraist, *Interpreting GPT: the logit lens*(logit lens 原始貼文)— <https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens>

**MLX autograd 筆記**
- `mx.vjp` 對 activation 可微(含 4-bit `QuantizedLinear`);GatedDeltaNet 可微路徑 =
  linear block `training=True` 的 `gated_delta_chunked` 純 ops scan + fit 時把 `mx.async_eval` no-op(見 notebook §4)。
