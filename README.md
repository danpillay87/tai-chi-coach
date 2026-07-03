# STILL — *move your ghost*

A solo **tai-chi / posture visual trainer**. PoC. Point a phone/webcam at yourself,
pick a posture, and STILL guides you into it: a glowing **target skeleton to copy**,
a live neon skeleton of you, the **one thing to fix** as a spoken cue, and a hold
timer that celebrates when you nail it.

Forked from **GHOST** (the rowing coach) — it reuses GHOST's proven, fully
on-device MediaPipe pose pipeline and numbers-only privacy model.

**Art direction: "Ink Flow + Ki-Surge burst".** Painterly-anime calm while you
practise — a cream, brush-outlined figure with a jade glow (gold when you're on
target), on a deep-jade ground with slow chi ink-ribbons drifting behind. Then,
the moment you *complete* a hold, a Dragon-Ball-Z-style energy burst fires (gold
+ electric-blue core flash, expanding rings, rising sparks). Calm to practise,
with a payoff at the reward moment. (Chosen from a 4-way mock-up pitch — see the
art-direction artifact.) The new part is a **posture-hold engine** (instead of
rowing stroke segmentation) and a **full-body frontal skeleton** (instead of the
side-on 6-joint view).

## Run it

```
cd ~/tai-chi-coach
python -m http.server 8000
```
Open **http://localhost:8000** (localhost or https is required — camera is blocked
on `file://`). On a phone, serve it and hit the machine's LAN address, or push to
GitHub Pages like GHOST.

## What it does

- **Whole-body pose** via MediaPipe PoseLandmarker (on-device, pinned `0.10.20`).
- **Posture fingerprint** = a set of rotation/scale/position-invariant **joint
  angles** (elbows, shoulders, hips, knees + torso lean). This is what's compared,
  so where you stand and your body size don't matter.
- **Match %** 0–100 in the hero ring. Blends the weighted mean of all joints with
  the *worst* joint, so a single badly-off limb caps the score — you can't "hold" a
  sloppy pose.
- **Target ghost** — the reference posture drawn (dashed cream/jade) aligned onto
  your live body: the cartoon you copy. Your live skeleton flares **warm** on the
  limb that's most off.
- **One cue at a time** — the worst joint becomes a plain instruction ("straighten
  your left arm", "sink lower", "bring your back upright"), shown and spoken (Web
  Speech API). Anatomically correct left/right (subject's own body).
- **Hold to complete** — when you're still *and* above the match threshold for 3s,
  a chime + "posture held".
- **Capture your own** — get into a pose, hit *Capture new*, 3-2-1 countdown, it
  stores that pose's fingerprint as a reference.
- **Four built-in reference postures** ship (Settle / Hold up the heavens / Open /
  Horse stance), each **read from a real front-facing photo** via the same pose
  pipeline — see `refs/`. Named by what the figure does, not claimed as tai-chi
  canon. Add authentic ones any time via *Capture new*.

## Privacy (inherited from GHOST, non-negotiable)

Frames are read for pose and **discarded every frame**. Nothing is recorded or
uploaded. Only derived **numbers** (joint angles + normalised reference postures)
persist in `localStorage`, clearable in the menu.

## Known limits (it's a PoC)

- **Front-facing, slow, solo only.** This is the sweet spot for a single 2D camera.
  Turn side-on, reach toward/away from the camera, or rotate the torso and 2D angles
  get ambiguous (foreshortening) and accuracy drops.
- **Not biomechanically precise** — good enough to coach a facing-camera hold, not to
  judge fine internal rotation.
- Tuning constants (`CFG` at the top of `index.html`) — hold threshold, stillness,
  per-angle falloff — are set by feel and want a real body to dial in.

## Next steps (parked)

- **Flow mode**: follow a whole tai-chi form as a sequence (dynamic time warping
  against a reference trajectory) rather than discrete held postures.
- **Self-calibration** of the hold/stillness thresholds (GHOST already does this for
  rowing — same pattern transfers).
- **BJJ solo drills** (shrimping, bridging, breakfalls) — feasible but harder (floor,
  non-upright poses degrade the tracker). Partner/live rolling is out of scope.
