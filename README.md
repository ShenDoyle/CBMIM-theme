<p align="center">
  <a title="Hexo Version" target="_blank" href="https://hexo.io/zh-cn/"><img alt="Hexo Version" src="https://img.shields.io/badge/Hexo-%3E%3D%205.3.0-orange?style=flat"></a>
  <a title="Node Version" target="_blank" href="https://nodejs.org/zh-cn/"><img alt="Node Version" src="https://img.shields.io/badge/Node-%3E%3D%2010.13.0-yellowgreen?style=flat"></a>
  <a title="License" target="_blank" href="LICENSE"><img alt="License" src="https://img.shields.io/badge/License-GPL--3.0-blue.svg?style=flat"></a>
  <a title="Site" target="_blank" href="https://cbm.im/"><img alt="Site" src="https://img.shields.io/badge/site-cbm.im-success?style=flat"></a>
</p>

<p align="center">🇨🇳 中文简体</p>

# CBMIM theme

**cbm.im（南城左立方）专用 Hexo 主题**。独立维护、独立演进 —— 主题当前由 GPL-3.0 项目部署而来（版权归属见 [NOTICE.md](NOTICE.md)），后续将进行大量改造，逐步形成完全独立的主题。

本站仓库：<https://github.com/ShenDoyle/CBMIM-theme>　｜　站点：<https://cbm.im/>

---

## 📌 这个仓库是什么

| 项 | 内容 |
|---|---|
| 基线 | v1.7.1（源自 GPL-3.0 项目，来源与归属见 [NOTICE.md](NOTICE.md)） |
| 内容 | 站点实际运行中的主题状态 = 基线 + 本站改造 |
| 用途 | 主题的**版本库与唯一来源**：主题改动先在本仓库提交，再经 npm 依赖应用到站点 |
| 可见性 | public |
| 许可证 | **GNU GPL v3.0**（`LICENSE` 原文保留，未作改动） |

## 🧩 改造清单

| 文件 | 改动 |
|---|---|
| `_config.yml` | 默认图格式 jpg / gif / png → **webp**（`error_img.flink`、`error_img.post_page`、`valine.bg`、`cover.default_cover`）；默认品牌项（头像、收款码、徽章、AI 助手名等）中性化 |
| `scripts/events/merge_config.js` | 同上，默认图片路径同步改为 webp；品牌默认值清理 |
| `scripts/events/cdn.js` | internal CDN 包名 → `hexo-theme-cbmim` |
| `scripts/helpers/random.js` | 随机文章路由 → `/cbmim/random.js` |
| `scripts/events/init.js` | 弃用配置提示 → `_config.cbmim.yml` |
| `layout/includes/header/index.pug` | **修复独立页面顶部图丢失**：原写法恒用 `home_index_img_bg`，而该变量仅在首页分支赋值，导致 about / link / comments 等页面顶部图不显示 |
| `layout/includes/` | 模板子目录改名 `cbmim/`（partial 引用同步）；控制台打印文案改为 CBMIM |
| `source/js/` | js 子目录改名 `cbmim/`（cdn.js 引用同步） |
| `source/img/` | `404`、`comment_bg`、`default_cover`、`friend_404`、`loading` 转 webp，并删除对应的 5 个旧格式文件 |
| `source/favicon.ico`、`source/img/512.png`、`source/img/siteicon/*` | 替换为本站图标 |
| `README.md`、`NOTICE.md` | 本仓库自有文档，不影响运行时行为 |

**刻意保留的内部标识符**（改名会破坏样式与 JS，后续改造中逐步替换）：

- CSS 类名 `anzhiyufont`、`anzhiyu-icon-*` 与图标字体文件（全站图标体系）
- CSS 变量 `--anzhiyu-*`（主题色体系）
- JS 全局对象 `anzhiyu.*`（交互功能入口）与 DOM id `#anzhiyu-footer`
- 第三方依赖包 `anzhiyu-theme-static`（dark / swiper / friends vue 等静态资源）、`img2color-go` 主色调 API

> `package.json` 的 **`version` 刻意保持 `1.7.1`**。原因：`scripts/events/cdn.js` 会用该版本号拼 CDN 路径（`...@1.7.1/...`），改动会导致 CDN 资源取不到。

## 💻 安装 / 启用

### npm 安装（推荐，站点 `package.json` 声明依赖）

```bash
npm install github:ShenDoyle/CBMIM-theme#main --save
```

站点 `_config.yml`：

```yaml
theme: cbmim
```

Hexo 会自动从 `node_modules/hexo-theme-cbmim` 加载主题；主题仓更新后 `npm update hexo-theme-cbmim` 即可拉取。

> 若缺少渲染器：`npm install hexo-renderer-pug hexo-renderer-stylus --save`

### 内部 CDN 说明

`scripts/events/cdn.js` 的 internal CDN 依赖包名（现为 `hexo-theme-cbmim`）。该包**没有发布到 npm**，所以 `CDN.internal_provider` 只能用 `local`（默认）或 `custom`（用 `custom_format` 指向 jsDelivr 的 GitHub 源，如 `https://cdn.jsdelivr.net/gh/ShenDoyle/CBMIM-theme@main/source/${file}`）。

## ⚙ 覆盖配置

主题配置请放在站点根目录的 `_config.cbmim.yml`（Hexo 原生 `_config.<theme>.yml` 机制），避免更新主题时丢失。

- `_config.cbmim.yml` 中的配置**优先级高于**主题内 `_config.yml`
- 主题更新后可能新增配置项，升级时对照主题内 `_config.yml` 手动补齐
- 想把某项覆盖为空，注意**不要删掉主键**

## 🚀 应用到站点

主题以 **npm 依赖**（`hexo-theme-cbmim`，来源本仓库）安装，站点目录内**不保留主题源码**。

流程：**本仓库提交 → push → 站点 `npm update hexo-theme-cbmim` → 提交站点仓库 → 构建发布（cbm.im）**。

> ⚠️ 主题 `package.json` 的 `version` 必须保持 `1.7.1` 不动：本主题的 `scripts/events/cdn.js` 用它拼 CDN 路径，改了会导致 CDN 资源 404。

📘 **完整手册见 [`docs/使用与部署手册.md`](docs/使用与部署手册.md)** —— 仓库分工、三种发布触发方式、从零到线上的可执行命令、改主题/回滚流程、标签速查、封面规则、排错手册、验收清单。

## 📁 目录结构

```
├── _config.yml            主题默认配置（被站点 _config.cbmim.yml 覆盖）
├── layout/                页面模板（pug）
├── scripts/               Hexo 注入器 / 生成器 / 标签插件
│   ├── events/            构建期注入（含 cdn.js、merge_config.js）
│   ├── helpers/           模板助手
│   └── tag/               标签语法（note / hideToggle / tabs / tip 等）
├── source/                主题静态资源（css / js / img / font）
├── languages/             多语言文案
├── NOTICE.md              版权归属与修改声明（GPL-3.0 §5）
├── LICENSE                GNU GPL v3.0（原文，未改动）
└── README.md              本文件
```

## 📜 许可证

本项目以 **GNU General Public License v3.0** 发布。作为 GPL-3.0 派生作品，原始版权归属与逐项修改声明完整保留于 **[NOTICE.md](NOTICE.md)**，`LICENSE` 为许可证原文。
