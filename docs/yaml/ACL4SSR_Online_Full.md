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
| 📲 电报消息 | Telegram |
| 💬 OpenAi | OpenAI |
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
| 🇭🇰🇯🇵🇺🇲🇨🇳🇸🇬🇰🇷 地区节点 | 地区 url-test/select |

## 规则流向（摘要）

顺序大致为：

1. 局域网 / UnBan → 直连  
2. 广告 / 应用净化 → 拦截组  
3. FCM、GoogleCN、SteamCN  
4. Bing / OneDrive / Microsoft / Apple  
5. Telegram / OpenAI / 网易云 / 游戏平台  
6. YouTube / Netflix / 巴哈 / B 站  
7. 国内媒体 / 国外媒体  
8. ProxyGFWlist → 节点选择  
9. 国内域名/公司 IP / Download → 直连  
10. `GEOIP,CN` → 直连  
11. `MATCH` → 漏网之鱼  

rule-providers 多来自 `ACL4SSR/ACL4SSR` 仓库 list，经 jsDelivr 拉取，`interval: 86400`。

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
