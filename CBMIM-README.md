# CBMIM theme

cbm.im（南城左立方）博客专用的 **anzhiyu 主题定制版**，私有仓库，不对外发布。

## 这个仓库是什么

- **基线**：`anzhiyu-c/hexo-theme-anzhiyu` v1.7.1（上游 commit `07114b6`，dev 分支）
- **内容**：站点 `anviyu/themes/anzhiyu` 当前实际正在使用的主题状态（官方基线 + 本地改造）
- **用途**：主题的版本库与备份；以后主题改动先在这里提交，再同步到站点
- **可见性**：private

## 相对官方的本地改造

| 文件 | 改动 |
|---|---|
| `_config.yml` | 默认图格式 jpg / gif / png → **webp**（`error_img.flink`、`error_img.post_page`、`valine.bg`、`cover.default_cover`） |
| `scripts/events/merge_config.js` | 同上，默认图片路径同步改为 webp |
| `layout/includes/header/index.pug` | **修复独立页面顶部图丢失**：原写法恒用 `home_index_img_bg`，而该变量仅在首页分支赋值，导致 about / link / comments 等页面顶部图不显示 |
| `source/img/` | `404`、`comment_bg`、`default_cover`、`friend_404`、`loading` 转 webp，并删除对应的 5 个旧格式文件 |
| `source/favicon.ico`、`source/img/512.png`、`source/img/siteicon/*` | 替换为本站图标 |

除上述 21 个文件外，其余文件与官方 v1.7.1 逐字节一致。

## 与上游同步

```bash
# 上游（官方）
git remote add upstream https://github.com/anzhiyu-c/hexo-theme-anzhiyu.git

# 拉取官方更新并合并到本地定制版
git fetch upstream
git merge upstream/dev        # 官方主线是 dev 分支
```

冲突通常只会出现在上面那张表里的文件——那是本地改造所在处，按本地优先处理。

## 应用到站点

站点路径：`~/OneDrive/Backup/GitHub Page/anviyu/themes/anzhiyu`

主题改动在此仓库提交后，把变更同步到站点，提交站点仓库即可由 GitHub Actions 构建发布。
（后续可选择把站点的 `themes/anzhiyu` 改成指向本仓库的 git clone / submodule，实现单一来源。）
