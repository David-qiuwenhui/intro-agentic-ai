# Slidev + Claude Code 生成 AI PPT — 研究笔记

> 源项目：[riverscn/intro-agentic-ai](https://github.com/riverscn/intro-agentic-ai) (MIT License)
> 在线演示：https://intro-agentic-ai.lishun.me
> 演讲录像：https://www.bilibili.com/video/BV1v6o5BrEiT/

---

## 📌 技术架构总览

```
slidev-ppt-starter/
├── slides.md              ← 核心！2605 行 Markdown，52 页幻灯片
├── style.css              ← 自定义 CSS（术语 tooltip、对比表格、网格布局）
├── components/
│   └── GradientDescent3D.vue  ← Three.js 3D 可视化组件（12KB）
├── public/
│   ├── _redirects          ← Cloudflare Pages 路由
│   └── images/             ← 5 张 PNG + 1 个 MP4（共约 11MB）
├── package.json            ← Slidev 52 + theme-seriph + Three.js
├── wrangler.jsonc           ← Cloudflare Workers 部署配置
├── .agents/skills/slidev/  ← Slidev Skill（50+ 参考文件）
│   ├── SKILL.md            ← 核心 Skill 定义（8.7KB）
│   ├── README.md
│   └── references/         ← 50+ 细分功能参考
├── .claude/
│   ├── settings.json       ← Claude Code 权限配置
│   └── skills/slidev       ← symlink → .agents/skills/slidev
└── skills-lock.json        ← Skill 版本锁定
```

### 依赖说明

| 依赖 | 版本 | 作用 |
|------|------|------|
| `@slidev/cli` | 52.14.2 | Slidev 核心 CLI |
| `@slidev/theme-seriph` | 0.25.0 | 演示主题 |
| `three` | 0.184.0 | 3D 可视化（梯度下降动画） |
| `playwright-chromium` | 1.59.1 | PDF/PPTX 导出 |

---

## 🔍 原作者工作流解析

### Step 1: 安装 Slidev Skill

```bash
npx skills add slidevjs/slidev
```

这个命令会从 Slidev 官方仓库拉取 Skill 文件到 `.agents/skills/slidev/` 目录。
Skill 包含 **50+ 参考文件**，覆盖 Slidev 所有功能的 Markdown 语法指南。

### Step 2: 用 Claude Code 生成 slides.md

在安装了 Skill 的项目目录下启动 Claude Code，给出主题和要求：

```
帮我创建一个关于 XXX 的 Slidev 演示文稿，约 50 页，
面向 XXX 受众，风格为 XXX。
```

Claude Code 会根据 Skill 中的参考文件，直接在 `slides.md` 中生成符合 Slidev 语法的 Markdown 内容。

### Step 3: 预览和迭代

```bash
pnpm dev    # 启动 http://localhost:3030 实时预览
```

在浏览器中查看效果，通过 Claude Code 对特定页面进行修改和迭代。

### Step 4: 构建和部署

```bash
pnpm build                        # 构建静态 SPA
pnpm dlx wrangler deploy          # 部署到 Cloudflare Workers
# 或导出 PDF
pnpm export                       # 需要 playwright-chromium
```

---

## 🧩 slides.md 结构解析

### Headmatter（全局配置）

```yaml
---
theme: seriph              # 使用 seriph 主题
background: https://cover.sli.dev  # 封面背景
title: 认识 Agentic AI
info: |                    # SEO 信息
  ## 认识 Agentic AI
  从神经网络到智能体
class: text-center         # 默认 CSS 类
drawings:
  persist: false           # 画板不持久化
transition: slide-left     # 全局转场动画
mdc: true                  # 启用 MDC 语法扩展
---
```

### 单页 Slide 结构

```markdown
---
layout: center             # 页面布局
class: text-center
---

# 页面标题

正文内容...支持 **Markdown** 和 <span class="custom">HTML</span>

<!-- 演讲者笔记（不会显示在幻灯片上） -->
```

### 关键自定义 CSS 类

| CSS 类 | 用途 | 示例 |
|--------|------|------|
| `.term[data-zh]` | 英文术语悬浮中文翻译 | `<span class="term" data-zh="智能体">Agent</span>` |
| `.bridge-grid` | 4 列对比表格（生物 vs AI） | 4 列网格对比 |
| `.learn-grid` | 3 列学习对比 | 生物学习 vs 机器学习 |
| `.bridge-pillars` | 3 列圆角标签 | 三大支柱展示 |

### 特殊组件

- **GradientDescent3D.vue** — 使用 Three.js 渲染 3D 梯度下降可视化
- 在 slides.md 中通过 `<GradientDescent3D />` 直接引用

---

## 🚀 自定义修改指南

### 修改内容主题

1. 编辑 `slides.md` 的 headmatter（第 1-13 行）改标题、背景
2. 每页用 `---` 分隔，替换每页的标题和内容
3. 演讲者笔记放在 `<!-- -->` HTML 注释里

### 修改视觉风格

| 修改项 | 文件 | 方法 |
|--------|------|------|
| 主题色 | headmatter | 改 `theme: seriph` → 其他主题名 |
| 自定义样式 | `style.css` | 修改/新增 CSS 类 |
| 布局 | 每页 frontmatter | `layout: center/two-cols/image-right` 等 |
| 字体 | headmatter | 添加 `fonts:` 配置 |

### 添加新页面

在 `slides.md` 中找到合适位置，插入：

```markdown
---
layout: default
---

# 新页面标题

内容...

---
```

### 添加交互组件

在 `components/` 目录新建 `.vue` 文件，然后在 slides.md 中引用：

```markdown
---
layout: center
---

<MyComponent />
```

---

## 💰 Token 消耗评估

### 生成一套 50 页 PPT 的 Token 开销

| 阶段 | 输入 Token | 输出 Token | 说明 |
|------|-----------|-----------|------|
| Skill 加载 | ~8,000 | 0 | SKILL.md + references 自动注入 |
| 内容规划 | ~3,000 | ~2,000 | 生成大纲和页面结构 |
| 逐页生成 | ~5,000×5 | ~3,000×5 | 分批生成（每批约 10 页） |
| 迭代修改 | ~5,000×3 | ~3,000×3 | 约 3 轮迭代优化 |
| **合计** | ~50,000 | ~25,000 | |

### 费用估算

| 模型 | 输入价格 | 输出价格 | 总费用 |
|------|---------|---------|--------|
| Sonnet 4.6 | $3/M | $15/M | **~$0.5** |
| Opus 4.7 | $15/M | $75/M | **~$2.6** |
| Haiku 4.5 | $0.80/M | $4/M | **~$0.15** |

> 💡 实际费用会因迭代次数、内容复杂度有所浮动。建议用 Sonnet 生成，Opus 审查关键页面。

---

## 📋 常用命令速查

```bash
# 开发
pnpm dev              # 启动开发服务器 → http://localhost:3030

# 构建
pnpm build            # 构建静态 SPA → dist/

# 导出
pnpm export           # 导出 PDF（需先 pnpm approve-builds）

# 部署到 Cloudflare
pnpm dlx wrangler login   # 首次登录
pnpm build && pnpm dlx wrangler deploy

# Skill 管理
npx skills add slidevjs/slidev    # 安装 Slidev Skill
```

---

## 🔗 参考资料

- [Slidev 官方文档](https://sli.dev)
- [Slidev 主题画廊](https://sli.dev/resources/theme-gallery)
- [Slidev GitHub](https://github.com/slidevjs/slidev)
- [原作者仓库](https://github.com/riverscn/intro-agentic-ai)
- [Skill 安装来源](https://github.com/slidevjs/slidev/tree/main/.agents/skills/slidev)
