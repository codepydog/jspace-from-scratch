# Jacobian Lens Demo：架構章設計

## 範圍

只修改 `jacobian_lens_demo.ipynb` 的第 1 章。保留封面，不修改原本的
`jacobian_lens.ipynb`。

## Cell

以一個 Markdown cell 完成：

1. 標題：`## 1. Qwen3.6-27B VLM 架構`
2. 圖片：`assets/qwen3_6_27b_vlm_flow.png`
3. 三段短說明：
   - 影像與文字先轉成 5120 維向量，再合併成同一條 sequence。
   - Decoder 共 64 層；3 層 GatedDeltaNet 接 1 層 Attention，重複 16 次。
   - `Final RMSNorm → lm_head (5120 → 248,320 logits) → softmax → 下一個 token 的機率`。

`lm_head` 不包含 softmax。圖維持輸出 `next-token logits`，文字補充 softmax 才將
logits 轉為機率。

## 驗證

- Notebook JSON 可解析。
- Markdown 圖片路徑存在。
- 第 1 章只有一張圖、三段說明，沒有 code cell。
