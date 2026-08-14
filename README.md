# override-hub

自用 Clash / Mihomo 覆写规则集合。

## 文档

完整说明、选型与字段摘要见：**[docs/README.md](./docs/README.md)**

代理（AI）协作约定见：**[AGENTS.md](./AGENTS.md)**

## 源文件索引

### YAML

| 文件 | 说明 |
|------|------|
| [yaml/布丁狗的订阅转换.yaml](./yaml/布丁狗的订阅转换.yaml) | 轻量策略组 + AIGC/TG/Google |
| [yaml/ACL4SSR_Online_Full.yaml](./yaml/ACL4SSR_Online_Full.yaml) | ACL4SSR 全量分流 |
| [yaml/ACL4SSR_Online_Full_WithIcon.yaml](./yaml/ACL4SSR_Online_Full_WithIcon.yaml) | 全量 + 图标 + DNS |
| [yaml/easy_rules.yaml](./yaml/easy_rules.yaml) | 本地 reject/proxy/direct 三列表 |
| [yaml/添加直连规则.yaml](./yaml/添加直连规则.yaml) | `+rules` 直连示例 |

### JavaScript

| 文件 | 说明 |
|------|------|
| [javascript/布丁狗的订阅转换.js](./javascript/布丁狗的订阅转换.js) | 布丁狗逻辑脚本版 |
| [javascript/防止dns泄露(雾).js](./javascript/防止dns泄露(雾).js) | 防泄露规则 + fake-ip |
| [javascript/正则匹配设置代理组图标.js](./javascript/正则匹配设置代理组图标.js) | 组名正则补图标 |

### Rules

| 文件 | 说明 |
|------|------|
| [Rules/MyDirectRules.list](./Rules/MyDirectRules.list) | 自用直连 |
| [Rules/MyProxyRules.list](./Rules/MyProxyRules.list) | 自用代理 |

## 远程 raw（GitHub）

若仓库推送到 `mihomo-party-org/override-hub`，可用 raw 链接引用（按实际 owner/branch 修改）：

### YAML

- [布丁狗的订阅转换.yaml](https://raw.githubusercontent.com/mihomo-party-org/override-hub/main/yaml/%E5%B8%83%E4%B8%81%E7%8B%97%E7%9A%84%E8%AE%A2%E9%98%85%E8%BD%AC%E6%8D%A2.yaml)
- [ACL4SSR_Online_Full.yaml](https://raw.githubusercontent.com/mihomo-party-org/override-hub/main/yaml/ACL4SSR_Online_Full.yaml)
- [ACL4SSR_Online_Full_WithIcon.yaml](https://raw.githubusercontent.com/mihomo-party-org/override-hub/main/yaml/ACL4SSR_Online_Full_WithIcon.yaml)
- [添加直连规则.yaml](https://raw.githubusercontent.com/mihomo-party-org/override-hub/main/yaml/%E6%B7%BB%E5%8A%A0%E7%9B%B4%E8%BF%9E%E8%A7%84%E5%88%99.yaml)

### JavaScript

- [布丁狗的订阅转换.js](https://raw.githubusercontent.com/mihomo-party-org/override-hub/main/javascript/%E5%B8%83%E4%B8%81%E7%8B%97%E7%9A%84%E8%AE%A2%E9%98%85%E8%BD%AC%E6%8D%A2.js)
- [防止dns泄露(雾).js](https://raw.githubusercontent.com/mihomo-party-org/override-hub/main/javascript/%E9%98%B2%E6%AD%A2dns%E6%B3%84%E9%9C%B2(%E9%9B%BE).js)
