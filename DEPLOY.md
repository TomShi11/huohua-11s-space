# huohua.11s.space 部署说明

跳转页已发布到 GitHub Pages，**只剩一步 DNS 需要你在 Cloudflare 操作**。

## 现状

| 项目 | 状态 |
|---|---|
| 仓库 | https://github.com/TomShi11/huohua-11s-space |
| Pages 构建 | ✅ built（无错误） |
| CNAME 文件 | ✅ `huohua.11s.space` |
| DNS 解析 | ❌ 未配置 → 当前访问 502 |

## 你需要做的（1 分钟）

Cloudflare → 选择 `11s.space` → **DNS** → **Add record**：

| 字段 | 值 |
|---|---|
| Type | `CNAME` |
| Name | `huohua` |
| Target | `tomshi11.github.io` |
| Proxy status | **DNS only（灰云）** |
| TTL | Auto |

> ⚠️ 必须选**灰云**（DNS only）。开橙云代理会让 GitHub 无法验证域名归属，
> HTTPS 证书签发不了，并可能出现重定向循环。
> 证书签好后想开橙云再改也可以。

## 配置完成后

1. 等 1–5 分钟生效，访问 http://huohua.11s.space 确认能打开并自动下载
2. 回到仓库 **Settings → Pages**，勾选 **Enforce HTTPS**
   （证书由 GitHub 自动签发，通常几分钟内可用；灰云是前提）

验证命令：

```powershell
Resolve-DnsName huohua.11s.space
curl.exe -sI https://huohua.11s.space | Select-Object -First 3
```

## 日常维护

**发布扩展新版本后，本站无需任何改动。**

下载按钮指向 `releases/latest/download/douyin-spark-keeper.zip`，
GitHub 会自动解析到最新 release。只要保证：

- 新 release 里的附件文件名仍是 `douyin-spark-keeper.zip`
- release 不是 draft / prerelease

发版命令（在扩展目录）：

```powershell
# 1. 改 manifest.json 里的 version
# 2. 重新打包
pwsh -NoProfile -File ..\..\work\pack.ps1
# 3. 提交并发布
git add -A; git commit -m "release: vX.Y.Z"; git push
gh release create vX.Y.Z ..\douyin-spark-keeper.zip --title "vX.Y.Z" --notes "..."
```