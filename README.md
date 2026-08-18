# Apple WLOC 自用备份

本仓库保存 WLOC 模块、脚本和自部署 Worker。订阅地址及 Worker 均由本仓库维护，不依赖上游仓库继续存在。

## 固定地址

- Worker：<https://wloc-spoofer.wloc-backup.workers.dev/>
- 快捷指令解析接口：<https://wloc-spoofer.wloc-backup.workers.dev/api/parse>

代理工具订阅：

- Surge / Egern：<https://raw.githubusercontent.com/tyiag13/wloc-backup/refs/heads/main/modules/wloc.sgmodule>
- Quantumult X：<https://raw.githubusercontent.com/tyiag13/wloc-backup/refs/heads/main/modules/wloc.conf>
- Loon：<https://raw.githubusercontent.com/tyiag13/wloc-backup/refs/heads/main/modules/wloc.lpx>
- Stash：<https://raw.githubusercontent.com/tyiag13/wloc-backup/refs/heads/main/modules/wloc.stoverride>
- Shadowrocket：<https://raw.githubusercontent.com/tyiag13/wloc-backup/refs/heads/main/modules/wloc.module>

## 手机端设置

一次性设置：

1. 在代理工具中订阅对应模块并启用。
2. 安装并完全信任代理工具的 CA 证书。
3. 确认 MITM 主机名包含 `gs-loc.apple.com` 和 `gs-loc-cn.apple.com`。
4. 让 `workers.dev` 经过代理，不要对它进行 MITM/HTTPS 解密。例如：

   ```ini
   DOMAIN-SUFFIX,workers.dev,你的代理策略组
   ```

5. 「wloc 设置地理位置」快捷指令中的请求地址应以以下内容开头：

   ```text
   https://wloc-spoofer.wloc-backup.workers.dev/api/parse?format=json&u=
   ```

日常使用：

- 设置位置：在苹果地图或高德地图选点，点“共享”，运行「wloc 设置地理位置」。
- 恢复位置：运行「wloc 清理恢复位置」。
- 切换后没有立即生效：重新开关定位服务；高版本 iOS 仍无效时重启手机。

## 快捷指令备份

GitHub 仓库只保存模块、脚本和 Worker 源码，**不包含手机里已经安装的快捷指令本体**。

原作者删除 iCloud 分享链接，不会删除手机里已经安装的快捷指令。但以后无法再从原链接安装，因此应分别打开两条快捷指令，通过“共享 → 选项 → 文件 → 存储到文件”导出 `.shortcut` 文件；也可以由自己的快捷指令副本创建新的 iCloud 分享链接。

导出前检查快捷指令内不再出现以下上游地址：

```text
wloc-spoofer.wloc.workers.dev
wloc-pages.pages.dev
raw.githubusercontent.com/Yu9191/wloc
```

## 故障排查

- 提示 TLS/安全连接失败：确认 `workers.dev` 经过代理，并使用代理端 DNS；开关飞行模式后重试。
- Worker 首页能打开但快捷指令失败：检查快捷指令中的域名和 `EncURL` 变量是否仍在 `u=` 后面。
- 提示储存失败：检查模块、MITM、CA 证书和 VPN 是否都已启用。
- 定位未变化：查看代理日志是否命中 `/clls/wloc`，然后重新开关定位服务或重启手机。

## 仓库内容

- `modules/`：各代理工具的订阅文件。
- `dist/`：手机代理工具实际调用的脚本。
- `worker/`：地图链接解析接口和选点页面源码。
- `LICENSE`：AGPL-3.0 许可证，必须保留。

Worker 重新部署：

```bash
cd worker
npm install
npm test
npm run deploy
```

Worker 不使用数据库或 KV，解析请求不写持久化存储，`observability` 已关闭。

## 来源与许可证

本仓库基于 [Yu9191/wloc](https://github.com/Yu9191/wloc) 保存并修改，保留完整 Git 历史。代码按 [AGPL-3.0](LICENSE) 许可证使用。
