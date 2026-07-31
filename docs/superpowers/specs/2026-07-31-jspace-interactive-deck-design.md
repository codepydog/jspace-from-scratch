# J-space interactive deck

## Goal

Turn `assets/jspace_report.pdf` into a standalone HTML presentation that is easy to share and understandable without a presenter.

Reader outcome:

> After reading, the reader can explain what J-space is, which experiments support the global-workspace interpretation, and why the work does not establish that Claude is conscious.

## Format

- One standalone HTML file.
- Twelve full-screen scenes.
- Arrow keys, swipe, mouse wheel, and on-screen controls change scenes.
- A compact progress indicator shows the current scene.
- Responsive from phone to desktop.
- No build step and no network dependency.
- Motion is restrained and respects `prefers-reduced-motion`.

## Story

1. The question: what happens inside a model before words appear?
2. Global workspace in one concrete human example.
3. J-space: concepts visible internally but absent from output.
4. The five tests Anthropic used.
5. Readout is not enough: swap soccer for rugby.
6. Silent reasoning: spider becomes ant, 8 becomes 6.
7. One concept reused across several tasks: France becomes China.
8. Selectivity: explicit reasoning changes, automatic continuation does not.
9. How J-lens reads a model's intermediate state.
10. Monitoring hidden intent before the output appears.
11. What the experiments support, and what remains different from a brain.
12. The careful answer to “Is AI conscious?” plus links to the paper and notebook.

## Writing

- Rewrite every sentence; do not reproduce the PDF paragraph by paragraph.
- Use Traditional Chinese with established ML terms left in English.
- Write as a technically informed colleague explaining the paper at a whiteboard.
- Prefer short concrete sentences and direct claims.
- Remove rhetorical grandeur, vague authority, repeated transitions, slogan-like conclusions, and decorative bolding.
- Each scene has one headline, one short explanation, and at most one supporting note.
- Distinguish correlation, causal intervention, functional similarity, and phenomenal consciousness.

## Visual direction

- Editorial presentation, not a dashboard.
- Warm paper-like surfaces with dark ink and one cool accent.
- Large type, generous whitespace, and a visible but quiet slide number.
- Reuse selected figures from the PDF only when they carry evidence.
- Rebuild simple ideas as native HTML/CSS diagrams so they stay legible on phones.
- Cite every reused figure next to the figure.

## Source handling

- Preserve links to Anthropic's article, the research paper, external commentary, Neuronpedia, and this repository.
- Do not copy the paper's bibliography into the presentation.
- Keep claims inside the bounds of the supplied report. Any added factual claim needs a primary source.

## Verification

- Validate HTML, anchors, keyboard controls, swipe, and slide count.
- Test at desktop and mobile widths.
- Check every scene for overflow, clipped text, broken images, and unreadable contrast.
- Confirm the presentation works offline.
