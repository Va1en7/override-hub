# ACL4SSR_Online_Full（YAML）

- 源文件：[`yaml/ACL4SSR_Online_Full.yaml`](../../yaml/ACL4SSR_Online_Full.yaml)
- 带图标 + DNS 版：[`ACL4SSR_Online_Full_WithIcon.yaml`](../../yaml/ACL4SSR_Online_Full_WithIcon.yaml)

## 用途

基于 ACL4SSR Online Full 思路的完整分流覆写：多业务策略组、广告拦截、流媒体/微软/苹果/游戏等规则集。本文件**不含 DNS 段**。

## 主要策略组

| 组名 | 说明 |
|------|------|
| 🚀 节点选择 | 总选择（自动/各地区/手动/直连） |
| 🚀 手动切换 | include-all 手动挑节点 |
| ♻️ 自动选择 | url-test，interval 300，tolerance 50 |
| 🇭🇰🇯🇵🇺🇲🇨🇳🇸🇬🇰🇷 地区节点 | 地区 url-test/select |
| 📲 电报消息 | Telegram |
| 💬 AIGC | OpenAI + Google Antigravity + Gemini（Antigravity 走本地 `Rules/antigravity.list`，Gemini 走 blackmatrix7 CDN 规则集） |
| 📘 Meta | Facebook / Instagram / WhatsApp / Threads / Messenger / Quest（Oculus）/ Meta AI，规则集为 MetaCubeX `geosite/meta` 全量（含 Meta 开发者与开源域名）；`muse.ai` 不在此组，见下 |
| 📹 油管视频 | YouTube |
| 🎥 奈飞视频 / 🎥 奈飞节点 | Netflix |
| 📺 巴哈姆特 / 📺 哔哩哔哩 | 动漫/国内视频相关 |
| 🌍 国外媒体 / 🌏 国内媒体 | 媒体大类 |
| 📢 谷歌FCM | 推送 |
| Ⓜ️ 微软 Bing / 云盘 / 服务 | 微软拆分 |
| 🍎 苹果服务 | Apple |
| 🎮 游戏平台 | Epic/Origin/Sony/Steam/Nintendo |
| 🎶 网易音乐 | 网易云 |
| 🎯 全球直连 | 直连出口 |
| 🛑 广告拦截 / 🍃 应用净化 | 广告相关 |
| 🐟 漏网之鱼 | MATCH 兜底 |

组定义顺序：节点类（选择/手动/自动）→ 地区组 → 业务分流组 → 奈飞节点。
## 规则流向（摘要）

顺序大致为：

1. 局域网 / UnBan → 直连  
2. 广告 / 应用净化 → 拦截组；`muse.ai` → 💬 AIGC、`RULE-SET,meta` → 📘 Meta  
3. FCM、GoogleCN、SteamCN  
4. Bing / OneDrive / Microsoft / Apple  
5. Telegram / AIGC（OpenAI + Antigravity + Gemini）/ 网易云 / 游戏平台  
6. YouTube / Netflix / 巴哈 / B 站  
7. 国内媒体 / 国外媒体  
8. ProxyGFWlist → 节点选择  
9. 国内域名/公司 IP / Download → 直连  
10. `GEOIP,CN` → 直连  
11. `MATCH` → 漏网之鱼  

rule-providers 多来自 `ACL4SSR/ACL4SSR` 仓库 list，经 jsDelivr 拉取，`interval: 86400`；其中 `Antigravity` 指向本仓库的 [`Rules/antigravity.list`](../../Rules/antigravity.list)（自建 list，走 **GitHub raw**，不走 CDN），`Gemini` 指向 blackmatrix7 的 `Clash/Gemini/Gemini.yaml`（第三方 list，走 **testingcf CDN**）。

`meta` 是本地新增的 provider：`https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@meta/geo/geosite/meta.yaml`（`behavior: domain` + `format: yaml`，**testingcf CDN**，展开后 556 条），覆盖 Facebook / Instagram / WhatsApp / Threads / Messenger / Quest（Oculus）/ Meta AI / Meta 开发者与开源域名，以及大量仿冒域名。**上游已把 `muse.ai` 归入该分类**，所以本地在 `RULE-SET,meta` 之前单独写了 `- "DOMAIN-SUFFIX,muse.ai,💬 AIGC"` 把它抢回 AIGC 组。

> 为什么必须排在 `ProxyGFWlist` 之前：Meta 域名（`meta.com`、`facebook.com`、`instagram.com`、`whatsapp.com`、`threads.com` 等）都在上游 `ProxyGFWlist` 里，该规则在规则表尾部指向 `🚀 节点选择`。`RULE-SET,meta` 若排到它后面会被抢先命中，新组形同虚设。同时它排在 `BanAD` / `BanProgramAD` 之后，`atdmt.com` 这类广告追踪域名仍由广告拦截优先处理。

> 本地追加：`- "RULE-SET,Antigravity,💬 AIGC"` 排在 `RULE-SET,OpenAi` 之前，域名清单在 [`Rules/antigravity.list`](../../Rules/antigravity.list)，由 `rule-providers.Antigravity`（`type: http` + `behavior: classical`）经 **GitHub raw** 拉取，list 内不写出站。同时把出站组 `💬 OpenAi` 改名为 `💬 AIGC`。
>
> 原因：ACL4SSR 的 `Google.list` 不含这些域名，不追加就会落到 `🌍 国外媒体` / 兜底组。清单为**窄口径**（只收 Antigravity 专有域名），来自 mihomo-party 日志对 `agy.exe` 的实测归因（2026-09-24 ~ 10-09，2137 条连接），维护说明见 [`docs/rules/自建规则.md`](../rules/自建规则.md)。
>
> 追加 Gemini：`- "RULE-SET,Gemini,💬 AIGC"` 插在 `RULE-SET,Antigravity` 之后、`RULE-SET,OpenAi` 之前；规则集来自 blackmatrix7 `Clash/Gemini/Gemini.yaml`（`rule-providers.Gemini`，`type: http` + `behavior: classical`，经 **testingcf CDN** 拉取），覆盖 Gemini 网页 / AI Studio / Gemini API 等域名。注意该规则集**不含** Antigravity 的 `cloudcode-pa.googleapis.com` 系域名，两者互补。

## 与 WithIcon 版差异

| 项目 | Full | Full_WithIcon |
|------|------|----------------|
| DNS | 无 | 有完整 fake-ip / DoH |
| 图标 | 无/少 | 策略组带 icon |
| 分流骨架 | 同系 ACL4SSR | 同系，组名可能略简 |

## 修改提示

- 只想改兜底：改最后的 `MATCH` 指向组。
- 某业务强制某地区：改对应 `proxy-groups` 的 `proxies` 顺序或默认第一项。
- 规则集 URL 挂了就换镜像或本地 path。
