# Agentic AI Slidev Deck

Fork 自 [riverscn/intro-agentic-ai](https://github.com/riverscn/intro-agentic-ai)（MIT License），主题为「Agentic AI 入门与业务落地」的分享演示文档。

**[在线演示](https://intro-agentic-ai.pages.dev/)** · [演讲录像](https://www.bilibili.com/video/BV1v6o5BrEiT/) · [部署指南](docs/deploy-guide.md)

## 运行

```bash
pnpm install
pnpm dev
```

默认地址：`http://localhost:3030`

## 构建

```bash
pnpm build
```

## 部署

项目部署在 Cloudflare Pages，push 到 main 分支会自动构建部署。详见 [部署指南](docs/deploy-guide.md)。

如需导出 PDF 或 PPTX，可在安装浏览器导出依赖后执行：

```bash
pnpm add -D playwright-chromium
pnpm export
```
