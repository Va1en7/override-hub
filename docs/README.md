# Override Hub 文档

自用 Clash / Mihomo 覆写规则说明。源文件在仓库根目录的 `yaml/`、`javascript/`、`Rules/`。

## 目录结构

```text
override-hub/
├── yaml/           # YAML 覆写（策略组 + 规则集 + DNS）
├── javascript/     # JS 脚本覆写（运行时改 config）
├── Rules/          # 自建 classical 规则列表
└── docs/           # 本说明文档
```

## 快速索引

| 类型 | 文件 | 说明 | 文档 |
|------|------|------|------|
| YAML | [`yaml/布丁狗的订阅转换.yaml`](../yaml/布丁狗的订阅转换.yaml) | 精简策略组 + AIGC/TG/Google 分流 | [详情](./yaml/布丁狗的订阅转换.md) |
| YAML | [`yaml/ACL4SSR_Online_Full.yaml`](../yaml/ACL4SSR_Online_Full.yaml) | ACL4SSR 全量分流（无 DNS） | [详情](./yaml/ACL4SSR_Online_Full.md) |
| YAML | [`yaml/ACL4SSR_Online_Full_WithIcon.yaml`](../yaml/ACL4SSR_Online_Full_WithIcon.yaml) | ACL4SSR + 图标 + DNS | [详情](./yaml/ACL4SSR_Online_Full_WithIcon.md) |
| YAML | [`yaml/easy_rules.yaml`](../yaml/easy_rules.yaml) | 本地三文件规则集前置 | [详情](./yaml/easy_rules.md) |
| YAML | [`yaml/添加直连规则.yaml`](../yaml/添加直连规则.yaml) | `+rules` 追加直连示例 | [详情](./yaml/添加直连规则.md) |
| JS | [`javascript/布丁狗的订阅转换.js`](../javascript/布丁狗的订阅转换.js) | 与同名 YAML 逻辑等价的脚本版 | [详情](./javascript/布丁狗的订阅转换.md) |
| JS | [`javascript/防止dns泄露(雾).js`](../javascript/防止dns泄露(雾).js) | 防 DNS 泄露规则 + fake-ip | [详情](./javascript/防止dns泄露.md) |
| JS | [`javascript/正则匹配设置代理组图标.js`](../javascript/正则匹配设置代理组图标.js) | 按组名正则补图标 | [详情](./javascript/正则匹配设置代理组图标.md) |
| Rules | [`Rules/MyDirectRules.list`](../Rules/MyDirectRules.list) | 自用直连域名/进程 | [详情](./rules/自建规则.md) |
| Rules | [`Rules/MyProxyRules.list`](../Rules/MyProxyRules.list) | 自用代理域名 | [详情](./rules/自建规则.md) |

## 使用方式（Mihomo / Clash Verge / Party 等）

1. 订阅正常拉取节点配置。
2. 在客户端「覆写 / Override / 脚本」里挂对应 YAML 或 JS。
3. YAML 若使用 `+rules`，一般是**前置追加**规则，不整表替换。
4. JS 必须导出 `function main(config) { ...; return config }`。
5. 本地 `Rules/*.list` 需与 `easy_rules.yaml` 里的 `path` 对齐，或改成你机器上的实际路径。

## 选型建议

- **要完整 ACL 分流、广告拦截、流媒体分组**：用 `ACL4SSR_Online_Full*.yaml`。
- **要轻量 + AIGC 专组**：用「布丁狗的订阅转换」（YAML 或 JS）。
- **只加几条个人规则**：用 `添加直连规则.yaml` 或 `easy_rules.yaml` + `Rules/`。
- **补 DNS 安全**：在已有分流后再挂 `防止dns泄露(雾).js`。
- **只美化图标**：挂 `正则匹配设置代理组图标.js`。

## 文档约定

- 每个源文件对应一篇短文档：用途、关键字段、策略组/规则流向、修改注意点。
- 不在文档里整份粘贴大 YAML；以路径链接到源文件为准。
