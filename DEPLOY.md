# huohua.11s.space 部署说明（Vercel）

跟你的 `download.11s.space` 同一套做法：Vercel 导入 GitHub 仓库 + 加自定义域名。
Vercel 会自动在 Cloudflare 里指引 DNS，并自动签发 HTTPS 证书。

## 一次性配置（约 2 分钟）

1. 打开 https://vercel.com/new
2. 选 **Import Git Repository** → 找到 `TomShi11/huohua-11s-space` → **Import**
3. Framework Preset 保持 **Other**，其余全部默认 → **Deploy**
4. 部署完成后进项目 → **Settings → Domains** → 输入 `huohua.11s.space` → **Add**
5. Vercel 会给出一条 CNAME 记录，到 Cloudflare → `11s.space` → DNS 添加：

   | 字段 | 值 |
   |---|---|
   | Type | `CNAME` |
   | Name | `huohua` |
   | Target | Vercel 页面上显示的那个地址（形如 `xxxx.vercel-dns-017.com`） |
   | Proxy | **DNS only（灰云）** |

   > 灰云是必须的。开橙云代理会和 Vercel 的证书签发冲突。
   > 你的 `download.11s.space` 现在也是这么配的（解析到 `f8c459f9908c3a32.vercel-dns-017.com`）。

6. 回 Vercel 的 Domains 页面等状态变成 **Valid Configuration**，HTTPS 自动就绪。

## 之后就不用管了

`main` 分支每次 push，Vercel 自动重新部署。

**扩展发新版本时本站无需任何改动** —— 下载按钮指向
`releases/latest/download/douyin-spark-keeper.zip`，GitHub 自动解析到最新 release。
只要保证新 release 的附件名仍是 `douyin-spark-keeper.zip`，且不是 draft / prerelease。

## 附带的短链接

`vercel.json` 里配了两条，方便分享：

| 地址 | 跳转到 |
|---|---|
| `huohua.11s.space/dl` | 直接下载最新版 zip |
| `huohua.11s.space/github` | 仓库主页 |

## 扩展发版流程

```powershell
# 1. 改 outputs/douyin-spark-keeper/manifest.json 的 version
# 2. 重新打包（会生成带顶层文件夹的 zip）
pwsh -NoProfile -File work\pack.ps1
# 3. 提交并发布
cd outputs\douyin-spark-keeper
git add -A; git commit -m "release: vX.Y.Z"; git push
gh release create vX.Y.Z ..\douyin-spark-keeper.zip --title "vX.Y.Z" --notes "更新说明"
```