# Eric Tong — Personal Website

[![Deploy Hugo site to Pages](https://github.com/erictonger/erictonger.github.io/actions/workflows/deploy-site.yaml/badge.svg)](https://github.com/erictonger/erictonger.github.io/actions/workflows/deploy-site.yaml)

Eric Tong 的个人主页与技术博客，基于 [Hugo](https://gohugo.io/) 和 [Toha v4](https://github.com/hugo-toha/toha) 构建，并通过 GitHub Actions 自动发布到 [https://erictonger.github.io](https://erictonger.github.io)。

## 技术栈

- Hugo Extended 0.127.0
- Toha v4.5.0（Hugo Module）
- Node.js / npm（前端依赖）
- GitHub Actions + GitHub Pages（CI/CD）

## 项目结构

```text
.
├── assets/                  # Hugo 处理的图片与自定义 SCSS
├── content/                 # 博客文章
├── data/en/                 # 英文站点及首页板块数据
├── layouts/                 # 对 Toha 局部模板的本地覆盖
├── static/                  # 原样发布的静态资源（证书徽章等）
├── .github/workflows/       # GitHub Pages 自动发布流程
├── hugo.yaml                # Hugo 站点配置
├── go.mod                   # Toha 模块版本
└── package.json             # 前端构建依赖
```

## 本地开发

### 1. 安装环境

macOS（Homebrew）：

```bash
brew install go node
hugo version
go version
node --version
```

本项目的 Toha v4.5.0 需要 **Hugo Extended 0.127.x**。Homebrew 当前安装的最新版
Hugo 0.166 与此主题不兼容，请从
[Hugo v0.127.0 Releases](https://github.com/gohugoio/hugo/releases/tag/v0.127.0)
下载与你的系统对应的 Extended 版本，并确保 `hugo version` 输出中同时包含
`v0.127` 和 `extended`。

### 2. 安装依赖并启动

```bash
git clone git@github.com:erictonger/erictonger.github.io.git
cd erictonger.github.io
npm ci
hugo mod tidy
hugo server --buildDrafts --disableFastRender
```

浏览器访问 <http://localhost:1313>。Hugo 会监听文件变化并自动刷新页面。

### 3. 执行生产构建

```bash
hugo --gc --minify --cleanDestinationDir
```

生成结果位于 `public/`。该目录由 Hugo 和 CI 自动生成，已加入 `.gitignore`，不应提交到 Git。

## 内容维护

- About 与证书：`data/en/sections/about.yaml`
- Experiences：`data/en/sections/experiences.yaml`
- 自定义样式：`assets/styles/override.scss`
- 证书图片：`static/images/certifications/`
- 博客文章：`content/posts/`

证书徽章保存在站内，避免 Credly 图片防盗链或 URL 变更导致图标无法加载；点击徽章仍会进入 Credly 官方验证页面。

## 自动发布（CI/CD）

工作流位于 `.github/workflows/deploy-site.yaml`。向 `eric-blog-toha-style` 分支 push 后，GitHub Actions 会自动：

1. 安装 Hugo、Go、Node.js 和 Dart Sass。
2. 安装 npm 依赖并构建生产站点。
3. 上传 `public/` 构建产物。
4. 发布到 GitHub Pages。

首次启用时，进入仓库 **Settings → Pages → Build and deployment → Source**，选择 **GitHub Actions**。之后可在 **Actions** 页面查看构建日志，也可手动运行该工作流。

## 提交与发布

```bash
git status
git add README.md .github/workflows/deploy-site.yaml .gitignore hugo.yaml \
  data/en/sections/about.yaml data/en/sections/experiences.yaml \
  assets/styles/override.scss layouts/partials/misc/badge.html \
  static/images/certifications
git commit -m "feat: improve portfolio content and deployment"
git push origin eric-blog-toha-style
```

工作流成功后访问 [https://erictonger.github.io](https://erictonger.github.io)。

## License

本站内容与个人素材版权归作者所有；Toha 主题遵循其上游开源许可证。
