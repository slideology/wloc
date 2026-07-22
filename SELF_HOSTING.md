# 自有部署与使用

## 自有资产

- GitHub 仓库：https://github.com/slideology/wloc
- Cloudflare Worker：https://wloc-spoofer.slideology0816.workers.dev/
- 地图解析 API：https://wloc-spoofer.slideology0816.workers.dev/api/parse

代理模块、代理脚本、图标、选点页面和地图解析 API 均由以上自有资产提供。

## 首次安装

只安装你所使用代理工具对应的一项：

- Surge：https://raw.githubusercontent.com/slideology/wloc/refs/heads/main/modules/wloc.sgmodule
- Quantumult X：https://raw.githubusercontent.com/slideology/wloc/refs/heads/main/modules/wloc.conf
- Loon：https://raw.githubusercontent.com/slideology/wloc/refs/heads/main/modules/wloc.lpx
- Stash：https://raw.githubusercontent.com/slideology/wloc/refs/heads/main/modules/wloc.stoverride
- Shadowrocket：https://raw.githubusercontent.com/slideology/wloc/refs/heads/main/modules/wloc.module

安装后：

1. 启用模块、脚本与 HTTPS 解密（MITM）。
2. 安装并信任代理工具生成的 CA 证书。
3. 确认 MITM 主机名包含 `gs-loc.apple.com` 和 `gs-loc-cn.apple.com`。
4. 用 Safari 打开自建选点页面，选择位置后点击“储存到设备”。
5. 在代理工具日志中确认 WLOC 响应已被修改，再打开地图验证。

## 日常操作

- 设置或切换位置：打开自建选点页面，选点并储存。
- 恢复真实位置：在选点页面清除已保存坐标，或直接关闭模块。
- iOS 26 及以上版本：切换或清除后若仍显示旧坐标，需要重启设备清除 `locationd` 缓存。

## 可选快捷指令

快捷指令应保存到自己的 iCloud。编辑“设置地理位置”快捷指令时，将“获取 URL 内容”使用的接口设为：

`https://wloc-spoofer.slideology0816.workers.dev/api/parse`

设置完成后，从苹果地图或高德地图分享地点到该快捷指令即可。清理快捷指令不依赖地图解析 API，只需请求选点页使用的清理动作。

## 后续更新

同步上游代码时，先拉取并检查差异，避免覆盖自有 URL：

```bash
git fetch upstream
git log --oneline main..upstream/main
git diff main...upstream/main
```

确认合并后，重新检查运行时地址并发布：

```bash
rg -n "Yu9191/wloc|wloc-pages\.pages\.dev|wloc-spoofer\.wloc\.workers\.dev" README.md modules worker
cd worker
npm install
npm run deploy
```

Cloudflare 发布后，应回读首页和解析 API，确认 HTTP 200，再在手机端刷新模块。
