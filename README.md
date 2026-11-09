# Blenda Labs: Prompt-to-3D-Animation Prototype (Take-Home Test)

Welcome, and thanks for taking the time. This test mirrors the kind of work we do at Blenda Labs: AI generates a starting point, and a creative technologist makes it controllable, reliable and good.

## The task

**Build a prototype app where a user types a prompt (e.g. "the character waves hello, then looks surprised") and a rigged Blender humanoid performs it as a keyframed animation, with a preview or render.**

## What we provide

- An LLM API key with a spending cap. Never commit it to the repo; load it from an environment variable.
- A rigged humanoid `.blend` file (named bones, neutral T-pose or A-pose)
- Possibly: A short bone and pose reference sheet

## Core requirements

1. **Prompt input:** a simple UI (web, CLI, or Blender add-on panel, your choice).
2. **AI layer:** the LLM must return *structured data* (e.g. JSON with bone rotations, timing and easing), not freeform Python. Validate it before use.
3. **Blender layer:** a `bpy` script (headless or add-on) reads that data and inserts keyframes on the humanoid's bones, with easing and sensible limits so limbs don't break.
4. **Output:** a playable preview (viewport capture or Eevee render, 3-5 seconds).
5. **Failure handling:** bad LLM output, impossible poses and ambiguous prompts should fail gracefully.

## Stretch goals (optional)

- A pose library the LLM can reference ("wave", "shrug", "sit") instead of raw rotations
- Iterating on a result ("make it slower", "more energetic")
- Export in a format usable for an image-to-video step
- Chaining multiple prompts into one sequence

## Constraints

- **Time box:** 6-8 hours, spread over about 3-5 days. Please don’t go over; tell us what you'd do with more time instead.
- **Stack:** any language or framework for the app layer. The Blender side must be Python (`bpy`).
- **AI tools:** any AI coding tools are allowed, but you must disclose which ones you used.
- **Security:** do not execute arbitrary LLM-generated code unsandboxed. How you handle this is part of the evaluation.
- **Originality:** use only the provided rig and your own work. No existing IP or licensed characters.

## Deliverables

1. **Source code** with a README covering setup, how to run, and architecture. We should be able to run it from your instructions alone.
2. **Screen recording (2-3 min)** showing the app working on at least 5 different prompts, including 1-2 that fail or look bad.
3. **Short write-up (about half a page):** design decisions, limitations, and what you'd build next.

Send a zip or a link to a private repo.

## What we look for

- **Prompt and schema design:** structured, constrained output, and visible iteration
- **Blender integration:** clean `bpy` code, correct bone handling, reproducible setup
- **Animation quality:** natural timing and easing, believable poses, no broken deformation
- **Robustness:** validation, error handling, sensible fallbacks
- **Architecture and code quality:** readable, modular, runnable from your README
- **Product thinking:** a usable UI and sensible scope for the time box
- **Communication:** honesty about limits and clear next steps

Getting *a* result is easy. Getting a *reliably good* one is the real skill.

## Questions?

If something in the brief is unclear, ask your recruiting contact. Making a reasonable assumption and noting it in your write-up is also fine.
