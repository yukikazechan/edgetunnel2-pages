# edgetunnel2 Pages

单 UUID 管理页面，与 `yukikazechan/edgetunnel2` 的新版上游核心和链式代理 API 配套使用。

页面基于 `EDT-Pages/EDT-Pages.github.io@e344c8dff25fe4e2482115a70caf95710edb0d98`，补充链式代理设置界面；登录页使用相同上游版本。已移除旧的多用户界面。

将本仓库作为静态 Cloudflare Pages 发布（无需构建），然后在 Worker 中设置可选变量 `PAGES_URL=https://edgetunnel2-pages.pages.dev`。
用户通过 **Worker 的 `/admin`** 登录和管理；直接打开 Pages 域名只会展示模板，Pages 不保存账号或代理凭据。API 请求使用 Worker 域名下的 `/admin/chain-proxy.json`，由 Worker 验证登录并存入原有 KV。

链式代理需要同时部署修复后的 Worker。未设置 `PAGES_URL` 时，Worker 默认使用上游页面并注入同一界面，因此 Worker 可先行更新。

扩展源码和测试位于 Worker 仓库 `extensions/chain-proxy-ui.js`。修改界面后，在 Worker 仓库运行：

```sh
node scripts/build-pages.mjs --out ../edgetunnel2-pages
```

前提是 Worker 仓库的 `admin/index.html` 已同步到所需的上游页面版本。使用 `--admin <模板文件>`、`--login <登录模板文件>` 可指定新的上游模板。
