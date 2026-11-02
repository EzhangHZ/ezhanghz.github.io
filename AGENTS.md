# 工作区记忆：ezhanghz.github.io

## 本地预览（Docker，不推 GitHub）

Jekyll 4 站点，用 `jekyll/jekyll:4` 容器预览，2026-11-02 验证可用：

```bash
# 启动（容器名 jekyll-preview，监听宿主机 0.0.0.0:4000）
docker run -d --name jekyll-preview \
  -p 4000:4000 \
  -v /home/eric/ezhanghz.github.io:/srv/jekyll \
  jekyll/jekyll:4 \
  jekyll serve --host 0.0.0.0 --port 4000 --force_polling

# 访问
#   VS Code Remote-SSH：自动转发，浏览器打开 http://localhost:4000
#   纯 SSH：ssh -L 4000:localhost:4000 user@server

# 日志
docker logs -f jekyll-preview

# 停止并删除（镜像保留，下次秒起）
docker stop jekyll-preview && docker rm jekyll-preview
```

注意：
- 首次启动自动 `bundle install`，约 6 秒可用；改文件后自动增量重建，刷新浏览器即可。
- 日志中 Sass `@import` 的 deprecation warning 来自 Minimal Light 主题，不影响构建，忽略即可。
- `_site`、`.jekyll-cache` 已在 `.gitignore`，构建不污染 git 工作区。
- 本地 Jekyll 4 与线上 GitHub Pages 构建（github-pages gem，Jekyll 3.9）可能有细微差异；需完全一致可在 Gemfile 加 `gem "github-pages", group: :jekyll_plugins`。
