# Evolution Log

Use this file only when persistent evolution is requested.

Append lightweight observations. Do not promote single-run observations directly into `SKILL.md`.

```yaml
# Example
date: YYYY-MM-DD
task_type: "cover_generation"
article_type: ""
mode: "prompt_mode"
style_used: ""
title_used: ""
judge_weakness: ""
improvement_action: ""
final_direction: ""
keep:
  - ""
avoid:
  - ""
feedback_source: "self"
confidence: "low"
```

```yaml
date: 2026-05-26
task_type: "cover_generation"
article_type: "亲子赛事/活动攻略"
mode: "image_mode"
style_used: "大字人像攻略封面"
title_used: "雨后斯巴达避坑"
judge_weakness: "用户只提供正文时直接生成图片，未先询问人脸、现场素材或风格参考图。"
improvement_action: "在 image_mode / production_mode 中加入参考图确认步骤；用户明确无参考图后才生成无参考图版本。"
final_direction: "先问参考图，再生成。"
keep:
  - "只给正文且请求出图时，先询问参考图能提高人物一致性、现场真实感和风格对齐。"
avoid:
  - "不要在用户未确认参考图需求时直接调用图片生成。"
feedback_source: "human"
confidence: "high"
```

```yaml
date: 2026-05-26
task_type: "cover_generation"
article_type: "小红书封面通用"
mode: "image_mode"
style_used: "any"
title_used: ""
judge_weakness: "只在提示词中写竖版 3:4，不足以保证图片工具实际输出符合小红书封面比例。"
improvement_action: "加入默认 1080x1440 和生成后 3:4 画幅校验；比例错误时优先重生成，其次裁切/排版成 1080x1440。"
final_direction: "默认 3:4 必须成为生成后校验规则。"
keep:
  - "小红书封面默认竖版 3:4，目标尺寸 1080x1440。"
  - "出图后要检查实际画幅，不符合就修正后再交付。"
avoid:
  - "不要只靠提示词声明 3:4 就直接交付。"
feedback_source: "human"
confidence: "high"
```

```yaml
date: 2026-05-28
task_type: "example_analysis"
article_type: "小红书封面样图归纳"
mode: "prompt_mode"
style_used: "batch_reference_analysis"
title_used: ""
judge_weakness: "样图风格分散，如果只记单张特征，会丢失可复用的封面结构规律。"
improvement_action: "按版式、主体处理、字体、配色、装饰和适用场景收拢成风格候选规则。"
final_direction: "把多张样图稳定出现的贴纸抠图、巨大标题、旅行拼贴、暖调生活方式、海报框架、拼图留白等模式写入候选规则。"
keep:
  - "人物或主物白色描边能显著提高手机端识别度，并制造贴纸拼贴感。"
  - "4-10 个字的大标题比长句更适合封面首屏，标题通常占画面 40%-60%。"
  - "背景需要弱化处理：虚化、分区拼贴、框架包裹或留白，避免和标题/人物抢第一注意力。"
  - "旅行和城市内容适合地标、美食、交通、路线箭头、虚线和手绘标注拼贴。"
  - "生活方式内容适合暖调光影、顶部细节小图、右侧人物白描边和左侧手写标题。"
avoid:
  - "不要让背景、人物、标题、小素材同时满强度抢视觉中心。"
  - "不要把长中文标题交给图片模型直接生成；生产可用时应后期排版。"
  - "不要把样图里的具体人物、城市素材或标题直接复刻。"
feedback_source: "human"
confidence: "medium"
```
