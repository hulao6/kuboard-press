# AGENTS.md — Kuboard Press

## 项目概述

Kuboard 官方文档站，基于 **VuePress 1.x** 构建。Kuboard 是一款免费的 Kubernetes 管理界面。

## 技术栈

| 层面 | 技术 |
|---|---|
| 框架 | VuePress 1.8.2 (Vue 2) |
| 主题 | 继承 `@vuepress/theme-default`，自定义覆盖主题组件 |
| 包管理 | pnpm |
| 容器 | Docker + nginx 部署 |
| 构建 | `vuepress build .` → 输出到 `docs/` 目录 |

## 目录结构

```
.vuepress/               # VuePress 配置与自定义代码
├── config.js            # 主配置：标题、插件、导航、主题配置
├── config-sidebar.js    # 侧边栏配置（核心导航结构）
├── config-sidebar-guide.js  # 开发环境额外侧边栏
├── enhanceApp.js        # Vue 插件注册（BootstrapVue、自定义组件、AOS、Cookies等）
├── components/          # 全局 Vue 组件（首页、广告、安装检查、评论等）
├── theme/               # 自定义主题
│   ├── index.js         # 继承默认主题
│   ├── layouts/         # 自定义布局（404.vue）
│   └── components/      # 覆盖默认主题组件（Navbar、Sidebar、Page等）
├── comp/                # 自定义 Vue 全局插件（课程、版本显示、KuboardDemo 等）
├── grid/                # 自定义响应式网格布局组件
├── styles/              # 全局样式覆盖（index.styl, palette.styl）
└── public/              # 静态资源（favicon、CNAME、robots.txt 等）

learning/                # Kubernetes 教程（入门/进阶/高级/实战）
guide/                   # Kuboard 使用指南（集群管理、应用管理、套件等）
install/                 # 安装文档（Kuboard v3/v2、K8S 安装、FAQ 等）
overview/                # 产品概述、概念、对比
v4/                      # Kuboard v4 文档
support/                 # 授权支持、更新日志
glossary/                # 术语表
t/                       # 考试/题目（CKA 每日一题）
docker/                  # Docker 部署配置（nginx.conf）
```

## 关键命令

```bash
pnpm install           # 安装依赖
pnpm docs:dev          # 启动开发服务器（localhost:8000）
pnpm docs:build        # 构建静态文件到 docs/
```

## 内容编写规范

1. **文档格式**: Markdown，支持 VuePress 扩展语法
2. **Frontmatter**: 每篇文档需包含 `layout`、`description` 等字段
3. **侧边栏**: 在 `.vuepress/config-sidebar.js` 中配置，路径相对于项目根目录
4. **图片资源**: 放在文档同级的 `.assets/` 目录下，使用相对路径引用
5. **外部链接**: 自动添加 `target="_blank"` 和 `nofollow`

## 自定义组件

位于 `.vuepress/components/` 的组件会自动注册为全局组件：

| 组件 | 用途 |
|---|---|
| `SpecialHomePage` | 首页自定义布局 |
| `SpecialSupportPage` | 支持页面自定义布局 |
| `StarCount` / `StarCountDockerPulls` | GitHub 星星 / Docker Pulls 计数 |
| `InstallBanner` / `InstallEnvCheck` | 安装引导和检查 |
| `PageVssue` | 基于 GitHub Issues 的评论区 |
| `FancyImage` | 图片增强展示 |
| `FrequentQuestion` | 常见问题折叠面板 |
| `AdSense*` | 广告组件 |

## 开发注意事项

- **Vue 2 + VuePress 1.x**: 组件使用 Options API，不支持 Composition API
- **BootstrapVue**: 全局注册，可在 Markdown 和组件中直接使用
- **Element UI**: 通过 babel-plugin-component 按需加载
- **代理配置**: `config.js` 中配置 `/uc-api/` 代理到 `http://kb:8080`
- **构建输出**: 默认输出到 `docs/` 目录（被 `.gitignore` 忽略）
- **关于 VuePress 本身**: 本项目使用 VuePress 1.x，不是 VuePress 2.x。VuePress 1.x 基于 Vue 2，使用 Webpack 4 构建，配置方式为 `module.exports` 导出对象。所有插件、主题配置均遵循 VuePress 1.x 规范。