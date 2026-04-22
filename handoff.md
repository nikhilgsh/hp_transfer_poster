# Handoff: ICLR 2026 poster

## What this is

Landscape 190 × 90 cm poster for Ghosh, Wu, Bietti, *Understanding the Mechanisms of Fast Hyperparameter Transfer*, ICLR 2026 main conference. Nikhil presents verbally at the poster session — the poster does visual heavy lifting and scaffolds his talk. It is *not* meant to be fully self-contained in text.

## Build

```
conda run -n texlive tectonic --keep-intermediates --synctex main.tex
```

- `main.tex` + `preamble.tex` at the repo root (Overleaf-importable).
- `decomposition_equation.tex` is the user-authored standalone source for the alignment-matrix / top-$k$ equation figure. Compile separately with tectonic to produce `decomposition_equation.pdf`, which `main.tex` `\includegraphics`'s in Panel C.
- `\graphicspath` in `main.tex` resolves figure names against `HP_Transfer__slides_/{figure,screenshot,logo}/`, `HP_Transfer__arxiv_/arxiv_figures/`, and `tweet/`, plus repo root — so most `\includegraphics` calls use bare filenames.
- Preview: `pdftoppm -png -r 55 main.pdf /tmp/poster_preview` (pdftoppm prints a libtiff warning — harmless).
- **Do not pipe `conda run` through `grep`/`tail`/`head`** — the harness's `block_unbounded_recursive_scan.sh` hook blocks it. Redirect to a file and read after: `conda run ... > /tmp/x.log 2>&1; grep ... /tmp/x.log`.
- User's global CLAUDE.md applies: Unicode math in terminal output (λ, η, √, ₀₁₂), LaTeX only inside `.tex`/`.md`.

## Current layout (describes state; not a changelog)

**Title band**: title + authors + emails centered, Flatiron + NYU logos on the right at ~4 cm height. No ICLR text, no thesis strip under the title — both removed per user request.

**Column 1** (`0.48\paperwidth`):
- Panel A *Introduction to µ-Transfer*: intro paragraph spanning the column, then `mu-transfer.png` (cartoon) + `mu_transfer1.png` (real "optimum shifts" vs "optimum stable" plot) side-by-side.
- Panel B *Fast transfer*: intro line defining the three gaps; one row with four equal-size items `[a_n.png | b_n.png | c_n.png | parabola-TikZ inset]`; then useful-transfer theorem paragraph; then `slow.png` (58%) + `\bluebox` with the rate-theorem statement and follow-up line (40%).

**Column 2** (`0.48\paperwidth`):
- Panel C *The mechanism: low-rank trajectory decomposition*: intro, `decomposition_equation.pdf` (alignment matrix, top-$k$/residual split, layer+time sum), `decomp.png` cartoon, tealbox with the 3-part conjecture from `HP_Transfer__slides_/results.tex:418–427` — (1) Top-$k$ LSC, (2) Top-$k$ Invariance (φᵏₙ ≈ φᵏ∞, νᵏ*ₙ ≈ νᵏ*∞), (3) Residual Flatness (νᵏ*ₙ ≈ ν*ₙ).
- Panel D *Real-model evidence: conjecture verified*: intro + setup line, then the single three-panel figure `figure/topk-residual.png` (Total | Top-κ | Residual — directly mirrors `decomp.png`), per-panel descriptions, tealbox, final one-liner teasing what's in the paper + arXiv ID (2512.22768).

No bottom takeaway strip, no QR. Both were rejected.

## User preferences that aren't derivable from the code

- **Voice: match `tweet.pdf`** (repo root). Direct, punchy: "brutally expensive at scale", "fast *enough*", "optimal HPs should depend on network statistics that stabilize with width *faster* than the loss itself". Avoid academic opener phrasing ("scale-aware parameterizations promise that..."). Why: explicit user instruction after they shared `tweet.pdf`.
- **Verbal-support mode, not self-contained.** Captions are tagline-length. The author narrates the rest. Why: "the poster has to do the visual heavy lifting without being visually confusing and provide me with context for verbal explanations."
- **Prefer `tweet/` imagery over slides screenshots** where a clean alternative exists. `mu-transfer.png`, `decomp.png`, `slow.png`, `topk-residual.png` are the anchor tweet figures. `mu_transfer2.png` (the gears/money infographic in `HP_Transfer__slides_/screenshot/`) was explicitly called "disgusting" — do not reintroduce.
- **3-part conjecture, not 2-part.** Earlier drafts used a 2-part conjecture (top-$k$ width-stable + residual flat); user corrected this by pointing at the 3-part version in `HP_Transfer__slides_/results.tex`. Keep LSC as a separate item.
- **Figure whitespace is the recurring complaint.** When the column is wider than the image, use `width=\linewidth` *with* an explicit `height=Ncm, keepaspectratio` so LaTeX picks the more restrictive. If a figure is floating with empty space beside it, the fix is usually pairing it with another figure or a text callout, not adding more captions.
- **Don't `\cd` away from the repo root in `Bash`** — cwd persists across calls. Use subshells `(cd ... && ...)` or absolute paths.

## Known open issues

- **Column height imbalance.** Column 1 (Panel A + Panel B) ends visibly above Column 2's bottom. Last render: `/tmp/poster_preview-1.png`. Could be fixed by bumping Panel A/B figures, adding a Panel B footer bluebox, or accepting the imbalance. User has flagged whitespace repeatedly — lean toward fixing.
- **Overfull hbox warning** of ~538 pt (≈19 cm) at `main.tex:\end{frame}` on compile. PDF still renders cleanly at 190 × 90 cm. Worth investigating if visible bleed appears.
- **No QR code.** User called the earlier placeholder box "weird" and it was removed. Before printing, add a clean QR to arXiv 2512.22768 + project page. Likely lives somewhere unobtrusive (right side of title band, or a small bottom-right corner).
- **Panel title polish is ambiguous.** Current `\panelhead` renders `\LARGE\bfseries` purple title with a 2pt purple underline. User said "section titles are not polished" at one iteration; unclear whether the underline fix satisfied them. Be ready to iterate.
- **Parabola TikZ in Panel B** is smaller than the a_n/b_n/c_n cartoons beside it in the 4-wide row. Size-matching is approximate; user has flagged non-uniform sizing as a broader issue.
- **`tweet/` has unused images** that could replace or supplement current choices if a panel needs more content: `composite-profile-captioned-setup.png` (top-k trajectories + profile, 4-layer LLaMA setup baked into the image), `profile.png` (cumulative top-k vs k), `residual.png` (residual flat in LR, paper figure 9), `linearization.png` (EMA-linearized loss equivalence). Do not add without a reason — user prefers less-is-more.

## Design history captured elsewhere

The planning document at `/mnt/home/nghosh/.claude/plans/eager-seeking-dragonfly.md` records the agreed-upon panel structure, figure checklist, and design rules (verbal-support mode, "less is more"). It is the prior-art handoff — still mostly accurate, though recent iterations have diverged in details (e.g., the 3-part conjecture, cartoon moved to Panel C not Panel D, no bottom takeaway). Trust `main.tex` over the plan where they disagree.

## Recent process notes (things to avoid repeating)

- Many small sequential edits frustrate the user. When multiple complaints stack up, do a bigger structural pass rather than patching items one at a time.
- A recurring typo in my `old_string` was `\end{center>` instead of `\end{center}` — if an Edit fails to match, re-read the relevant lines before retrying.
- Claiming a fix is in place without rendering to verify has happened — always re-compile and render after structural changes.
