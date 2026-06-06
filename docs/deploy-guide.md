# Slidev 项目部署指南：Cloudflare Pages

> 本文档介绍如何将 Slidev 演示文稿部署到 Cloudflare Pages，通过在线链接分享给他人访问。

## 前置条件

- 一个 [Cloudflare](https://dash.cloudflare.com/sign-up) 账号（免费计划即可）
- 项目代码已推送到 GitHub 仓库
- 本地已安装 Node.js 18+ 和 pnpm

## 第一步：注册 Cloudflare 账号

1. 打开 [Cloudflare 注册页面](https://dash.cloudflare.com/sign-up)
2. 推荐使用 **GitHub 账号**直接登录（后续连接仓库更方便）
3. 也可以用邮箱注册，免费计划无需绑定信用卡

## 第二步：创建 Pages 项目

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 左侧导航栏选择 **Workers & Pages**
3. 点击 **Create** 按钮
4. 选择 **Pages** 标签页
5. 点击 **Connect to Git**

## 第三步：连接 GitHub 仓库

1. 首次使用需要授权 Cloudflare 访问你的 GitHub
2. 建议选择 **Only select repositories**，仅授权当前项目仓库
3. 选择你的 `slidev-ppt-starter` 仓库（或你 fork 后的仓库名）

## 第四步：配置构建设置

在构建配置页面填写以下内容：

| 配置项 | 值 |
|--------|-----|
| **Production branch** | `main` |
| **Framework preset** | `None` |
| **Build command** | `pnpm build` |
| **Build output directory** | `dist` |

点击 **Save and Deploy**，Cloudflare 会自动开始首次构建。

## 第五步：获取访问地址

构建完成后（通常 1-2 分钟），Cloudflare 会分配一个默认域名：

```
https://<项目名>.pages.dev
```

你也可以在项目设置中自定义子域名。

## 后续更新流程

配置完成后，后续更新内容只需：

```bash
# 1. 修改 slides.md 或其他文件
# 2. 提交并推送到 main 分支
git add .
git commit -m "update: 演示内容"
git push

# Cloudflare 会自动检测到 push 并触发重新构建部署
```

每次 push 到 main 分支，Cloudflare 会自动执行 `pnpm build` 并部署新版本。整个过程通常 1-2 分钟完成。

## 可选配置

### 自定义域名

如果你有自己的域名：

1. 在 Cloudflare Pages 项目设置中选择 **Custom domains**
2. 点击 **Set up a custom domain**
3. 输入你的域名并按提示配置 DNS（如果域名已在 Cloudflare 管理，会自动配置 CNAME 记录）

### 预览部署

每个非 main 分支的 push 或 Pull Request 都会生成一个独立的预览地址：

```
https://<commit-hash>.<项目名>.pages.dev
```

适合在合并前预览效果。

### 环境变量

如果构建过程需要环境变量（如 API key），在项目设置 **Environment variables** 中添加。当前项目无需额外环境变量。

## 故障排查

| 问题 | 解决方案 |
|------|---------|
| 构建失败：`pnpm: command not found` | 在项目设置 **Environment variables** 中添加 `NPM_FLAGS` = `--version`，并将 **Build command** 改为 `npm install -g pnpm && pnpm build` |
| 页面 404 | 确认 Build output directory 填的是 `dist`，不是 `dist/` |
| 样式或图片加载失败 | 检查 `public/` 目录下的资源路径是否正确 |
| 推送后没有触发构建 | 检查 GitHub 仓库的 Webhook 设置是否正常 |

## 参考链接

- [Cloudflare Pages 官方文档](https://developers.cloudflare.com/pages/)
- [Slidev 官方部署指南](https://sli.dev/guide/hosting)
