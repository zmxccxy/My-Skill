# My-Skill

生活和工作中沉淀下来的 skill 合集，供 TRAE 等 AI 编程助手复用。

## 什么是 Skill

Skill 是一份带元信息的 Markdown 说明书（`SKILL.md`），用于教会 AI 助手完成特定任务，可附带模板、脚本等资源文件。

## 目录结构

```
My-Skill/
├── README.md
├── .gitignore
└── html-ppt/              # 深色科技风格 HTML 幻灯片生成器
    ├── SKILL.md           # 技能定义（色彩、排版、动画、导航等规范）
    └── assets/
        └── template.html  # 完整的 2 页 HTML 模板
```

## 技能列表

| 技能 | 说明 |
| --- | --- |
| [html-ppt](./html-ppt/SKILL.md) | 生成深色科技风格的单文件 HTML 演示文稿（培训课件 / 演讲 PPT），内联 CSS/JS，含入场动画、键盘/触摸导航、激光笔效果 |

## 使用方式

将目标技能目录复制到项目的 `.trae/skills/` 下即可：

```bash
cp -r html-ppt /path/to/your-project/.trae/skills/
```

之后在 TRAE 中描述需求（如"帮我生成一份关于 XX 的 HTML 演示文稿"），AI 会自动加载对应 skill。

## 贡献规范

- 每个技能一个独立目录，根目录下必须包含 `SKILL.md`
- `SKILL.md` 头部需包含 `name` 与 `description` 元信息
- 模板、示例等资源文件统一放在技能目录下的 `assets/` 中
- 新增技能后，请同步更新本文件的"技能列表"
