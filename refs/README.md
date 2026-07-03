# Reference postures

The four built-in postures in the app were derived from these front-facing,
full-body reference images. Each image was generated front-on (so its 2D joint
angles map onto a facing user), visually vetted, then run through the same
MediaPipe pose pipeline the app uses to extract a joint-angle fingerprint, which
is baked into `samplePostures()` in `index.html`.

- `embrace_tree.png`  → "Settle · hands at the dan tien"
- `heavens.png`       → "Hold up the heavens"
- `openarms.png`      → "Open · arms wide"
- `embrace_deep.png`  → "Horse stance · hold the ball"

Named descriptively by what the figure does — not claimed as authenticated
tai-chi canon. Add authentic ones any time via "Capture new" in the app, or by
dropping a front-facing photo through the same extraction step.
