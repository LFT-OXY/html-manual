github链接： https://github.com/nextlevelbuilder/ui-ux-pro-max-skill

https://ui-ux-pro-max-skill.nextlevelbuilder.io/

这是一款 AI SKILL，能为你提供设计智慧，帮你打造专业级的 UI/UX，横跨各种平台和框架都不在话下！

## 概述

UI UX Pro Max 是一个可搜索的数据库，里面涵盖了 UI 风格、配色方案、字体搭配、图表类型、产品推荐、UX 指南，还有针对不同技术栈的最佳实践。它本身是一个技能/工作流，可以集成到各种 AI 编程助手（比如 Claude Code、Codex、Cursor、Windsurf 等等）里使用。

## 功能亮点

- 57 种 UI 风格任你挑——玻璃拟态、粘土拟态、极简主义、粗野主义、新拟态、便当网格、深色模式等等，应有尽有！
- 95 套配色方案 - 专为不同行业定制，比如 SaaS、电商、医疗健康、金融科技、美妆护肤等等，直接拿来就能用。
- 56 组字体搭配 - 精心挑选的排版组合，直接导入 Google Fonts 就能用
- 24 种图表类型 - 帮你搞定仪表盘和数据分析的选图难题
- 涵盖 11 大技术栈——React、Next.js、Vue、Nuxt.js、Nuxt UI、Svelte、SwiftUI、React Native、Flutter、HTML+Tailwind、shadcn/ui，主流框架一网打尽！
- 98 条 UX 设计准则 - 全是干货：最佳实践、要避开的坑，还有无障碍设计规则

## 安装指南

### 使用 CLI（推荐）

```
# Install CLI globally
npm install -g uipro-cli

# Go to your project
cd /path/to/your/project

# Install for your AI assistant
uipro init --ai claude      # Claude Code
uipro init --ai cursor      # Cursor
uipro init --ai windsurf    # Windsurf
uipro init --ai antigravity # Antigravity (.agent + .shared)
uipro init --ai copilot     # GitHub Copilot
uipro init --ai kiro        # Kiro
uipro init --ai codex       # Codex (Skills)
uipro init --ai gemini      # Gemini CLI
uipro init --ai all         # All assistants
```

### 其他 CLI 命令

```
uipro versions              # List available versions
uipro update                # Update to latest version
uipro init --version v1.0.0 # Install specific version
```


### 手动安装指南

把对应的文件夹复制到你的项目里：

| AI 助手随时待命       | 需要复制的文件夹                                                             |
| --------------- | -------------------------------------------------------------------- |
| Claude Code     | `.claude/skills/ui-ux-pro-max/`                                      |
| Cursor          | `.cursor/commands/ui-ux-pro-max.md` + `.shared/ui-ux-pro-max/`       |
| Windsurf        | `.windsurf/workflows/ui-ux-pro-max.md` + `.shared/ui-ux-pro-max/`    |
| Antigravity     | `.agent/workflows/ui-ux-pro-max.md` + `.shared/ui-ux-pro-max/`       |
| GitHub Copilot  | `.github/prompts/ui-ux-pro-max.prompt.md` + `.shared/ui-ux-pro-max/` |
| Kiro            | `.kiro/steering/ui-ux-pro-max.md` + `.shared/ui-ux-pro-max/`         |
| Codex           | `.codex/skills/ui-ux-pro-max/`                                       |
| Gemini CLI 使用指南 | `.gemini/skills/ui-ux-pro-max/` + `.shared/ui-ux-pro-max/`           |
