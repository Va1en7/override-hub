# ACL4SSR_Online_Full_WithIcon（YAML）

- 源文件：[`yaml/ACL4SSR_Online_Full_WithIcon.yaml`](../../yaml/ACL4SSR_Online_Full_WithIcon.yaml)
- 无 DNS 对照：[`ACL4SSR_Online_Full.yaml`](../../yaml/ACL4SSR_Online_Full.yaml)

## 用途

ACL4SSR 全量分流 + 策略组图标 + 内置 DNS（fake-ip）。适合 Mihomo 系客户端直接当整包覆写。

## DNS 要点

| 项 | 值 |
|----|-----|
| enable | true |
| listen | `0.0.0.0:1053` |
| enhanced-mode | fake-ip |
| fake-ip-range | `198.18.0.1/16` |
| ipv6 | false |
| default-nameserver | 223.5.5.5 / 119.29.29.29 |
| nameserver | Cloudflare DoH |
| geosite:cn 策略 | 阿里 / 腾讯 DoH |
| proxy/direct nameserver | 阿里 / 腾讯 DoH |
| respect-rules | true |

`fake-ip-filter` 含局域网、local、Tailscale、微软连通性检测、Apple 等，避免假 IP 搞坏内网或连通性探测。

## 策略组与规则

整体与 `ACL4SSR_Online_Full.yaml` 同系：节点选择、电报、OpenAI、流媒体、微软、苹果、游戏、广告、漏网之鱼等。  
区别主要是：

- 组名更偏简洁（部分无 emoji 前缀，以源文件为准）
- 每组带 `icon`（Qure / 同类图标集）
- 文件头部多了完整 `dns:`


### 落地节点与地区组（反向排除）

- `落地节点` 组：`include-all` + `filter: (?i)落地`，把订阅里名字带「落地」的节点全部收进该组并 url-test 自动测速。
- 六个地区组（香港 / 日本 / 美国 / 台湾 / 狮城 / 韩国）的 `filter` 均以 `(?i)^(?!.*落地)` 开头，负向前瞻排除名字含「落地」的节点，保证地区组只含普通/中转线路，落地节点只留在 `落地节点` 组。
- 此写法依赖客户端支持 lookahead（Mihomo 的 filter 基于 regexp2，支持；旧版 Clash core 不支持）。
- 若订阅新增其他地区且命名含「落地」，需同步给对应地区组加上同样的 `^(?!.*落地)` 排除。
- 组定义顺序：节点类（选择 / 手动 / 自动 / 落地 / 中转）→ 地区组 → 业务分流组 → 奈飞节点 → GLOBAL。
## 修改提示

- 本机已有全局 DNS 覆写时，注意与客户端 DNS 设置冲突；可删掉本文件 `dns:` 段只保留分组规则。
- 改 DoH：同时检查 `nameserver`、`nameserver-policy`、`proxy-server-nameserver`、`direct-nameserver`。
- 图标 CDN 失效时批量替换 jsDelivr 域名即可。
