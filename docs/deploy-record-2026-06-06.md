# 部署记录：Cloudflare Pages

> 日期：2026-06-06
> 项目：Agentic AI Slidev Deck
> 仓库：David-qiuwenhui/intro-agentic-ai（fork 自 riverscn/intro-agentic-ai）

## 背景

将 Slidev 演示文稿部署到线上，方便通过链接分享给同事和朋友演示。

## 方案选型

| 方案 | 结论 |
|------|------|
| Cloudflare Workers | 项目已配置 `wrangler.jsonc`，但需要每次手动 `build + deploy` |
| **Cloudflare Pages** | **最终选择** — 连接 GitHub 后 push 自动部署，省心 |
| Vercel / Netlify / GitHub Pages | 备选方案，未采用 |

选择 Pages 的理由：后续维护只需 `git push`，无需手动构建部署。

## 实际操作步骤

### 1. 注册 Cloudflare 账号

- 地址：https://dash.cloudflare.com/sign-up
- 使用 GitHub 账号直接授权登录，免费计划

### 2. 创建 Pages 项目

- Dashboard → Workers & Pages → Create → Pages → Connect to Git

### 3. 连接 GitHub 仓库

- 授权 Cloudflare 访问 GitHub（仅授权当前仓库）
- 选择 intro-agentic-ai 仓库 → Begin setup

### 4. 构建配置

| 配置项 | 值 |
|--------|-----|
| Project name | `intro-agentic-ai` |
| Production branch | `main` |
| Framework preset | `None` |
| Build command | `pnpm build` |
| Build output directory | `dist` |

### 5. 部署结果

- 线上地址：https://intro-agentic-ai.pages.dev/
- HTTP 200，Slidev 52.14.2 构建正常
- 资源（CSS/JS/字体/图片）加载正常

## 配合部署完成的代码变更

| Commit | 内容 |
|--------|------|
| `61985a7` | 新增 `docs/deploy-guide.md` 部署指南文档 |
| `0f542ca` | 更新 README：标注 fork 来源、替换在线链接为 Pages 地址、精简部署说明 |

## 后续维护方式

```bash
# 修改内容后提交推送，Cloudflare 自动构建部署（约 1-2 分钟）
git add .
git commit -m "update: xxx"
git push
```

无需手动构建或手动部署。

## 待办（可选）

- [ ] 绑定自定义域名
- [ ] 添加 MIT LICENSE 文件
- [ ] 替换 slides.md 内容为自己的演示主题
