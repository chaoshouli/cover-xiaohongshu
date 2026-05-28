# Generation Modes

## prompt_mode

Use when the user wants a prompt, wants to paste into another image model, or asks to optimize a cover prompt.

Output:

```text
## 封面提示词

[final prompt]

## 生成建议

- 画幅：竖版 3:4
- 重点：[1-2 things to inspect]

## 本轮记忆

- 保留：[rule]
- 避免：[rule]
```

## image_mode

Use when the user asks to generate the cover image directly.

Workflow:

1. Extract Target.
2. Fill missing critical inputs.
3. If the user provided only article text, ask whether they want to upload reference images before generation.
4. Continue without references only after the user says no references are available or asks to proceed directly.
5. Generate 3 candidate directions internally.
6. Run Judge Improver and revise the draft.
7. Produce the final image prompt.
8. In Codex, call the image generation capability with the final prompt.
9. Verify the generated image is vertical 3:4.
10. If it is not 3:4, regenerate with stricter ratio instructions or crop/layout to 1080x1440.
11. Return the generated image and a short note with the title and style used.

Prompt requirements:

- Include platform and aspect ratio: vertical 3:4.
- Default target size: 1080x1440.
- Describe subject, composition, foreground, middle ground, background, lighting, color, typography area, and safe margins.
- If Chinese text must appear, keep it short and clearly quoted, but warn internally that image models may render Chinese incorrectly.

Aspect ratio rules:

- Default output must be vertical 3:4.
- Do not rely only on prompt wording.
- After generation, inspect the actual image dimensions when possible.
- If the ratio is wrong, fix before final delivery: regenerate first, crop/layout second.

## production_mode

Use when the user needs a publishable cover or explicitly cares about readable Chinese text.

Preferred workflow:

1. Generate a no-text or weak-text background image prompt.
2. Reserve clear title space in the prompt.
3. In Codex, generate the background image.
4. Add Chinese title and subtitle using deterministic layout tools when available.
5. Verify text readability, margins, contrast, composition, and vertical 3:4 ratio.

Production prompt rules:

- Do not ask the image model to render long Chinese text.
- Use phrases like "blank title area", "clean empty space for Chinese title", or "large readable typography area".
- Keep the final title outside the generated image when deterministic post-layout is possible.
- Use high contrast between title and background.
- Keep critical text away from face, eyes, product details, and edge margins.

## When Reference Images Exist

- Image 1 is always the face reference.
- Image 2+ are product/UI/material references.
- Do not describe every pixel of a reference image. Use its role: face identity, product screenshot, UI card, logo, object, or texture.
- Do not copy an example cover. Learn composition and hierarchy only.

## Failure Handling

If Codex image generation is unavailable:

- Complete `prompt_mode`.
- Tell the user the prompt is ready for image generation.
- If `production_mode` was requested, include the no-text background prompt and the exact title layout instruction.

## Codex Notes

- Use Codex image generation for `image_mode`.
- For `production_mode`, prefer generating a clean background without Chinese title text, then add readable Chinese title with deterministic layout when tools are available.
- Do not claim an image was generated unless the image generation call actually succeeds.
