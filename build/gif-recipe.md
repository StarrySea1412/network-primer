# orbitix GIF 渠道配方（已验证 2026-11-10）

## 渠道
- baseUrl: https://new.orbitix.site（OpenAI 兼容 /v1/images/generations）
- key: sk-kDaOI45ILwonvcWiNF8N0p14JrRHgaSAT8hMcyTSJclj4ZXI
- 模型: gpt-image-2.6-sunburst / gpt-image-2.6-flare（另有 2.5 两款）
- 已录入画境（db.json，name=orbitix），注意画境需重启或在 UI 刷新后才可见

## GIF 正确姿势（实测）
```
POST /v1/images/generations
{
  "model": "gpt-image-2.6-sunburst",
  "prompt": "... looping animation ...",
  "n": 1,
  "output_format": "gif"     ← 关键开关
}
```
- **绝对不要带 size**（包括 "auto" 可能同样撞）：上游 GIF 按自己的尺寸出（实测 413×412 / 437×390），钉了 size 网关直接 502 "Upstream GIF does not match the requested output"
- `response_format:"gif"` 是无效参数（被忽略，返回 PNG）
- 返回 url 直接是 .gif，实测为动画帧

## 已知行为
- 配额间歇 429 "no available image quota"——等 1–5 分钟重试，或切 2.6-flare / 2.5 系
- 生成耗时 30–90s，比静态图慢
- 尺寸约 400px 级，适合做装饰性循环动画（信鸽飞、封口蜡章闪），不适合当主视觉

## 用在 network-primer 的候选位
- hero 角落：信鸽驮信封循环飞（小尺寸放角落不抢主视觉）
- DNS 章节：问路的小纸条动画
- 仅装饰；核心动画仍由 SVG/Canvas 交互承担（GIF 不可交互、不可暂停）
