# easy_rules（YAML）

- 源文件：[`yaml/easy_rules.yaml`](../../yaml/easy_rules.yaml)
- 配套列表：[`Rules/`](../../Rules/)（`path` 已与仓库文件对齐）

## 用途

用本地三个 classical 规则文件做前置分流：拒绝 → 代理 → 直连。适合个人维护少量 list，而不是整包 ACL。

## 内容结构

```yaml
+rules:
  - RULE-SET,reject_rules,REJECT-DROP
  - RULE-SET,proxy_rules,PROXY
  - RULE-SET,direct_rules,DIRECT

rule-providers:
  reject_rules:  path: Rules/reject_rule.list
  proxy_rules:   path: Rules/MyProxyRules.list
  direct_rules:  path: Rules/MyDirectRules.list
```

要点：

- `+rules`：在原有规则**前**插入（具体合并语义以客户端为准）。
- provider 均为 `type: file`、`behavior: classical`、`format: text`。

## 与仓库 Rules/ 的关系

`easy_rules.yaml` 的 `path` 已与仓库实际文件一一对齐：

| provider | `path` | 仓库文件 |
|----------|--------|----------|
| `reject_rules` | `Rules/reject_rule.list` | `Rules/reject_rule.list`（空壳，待填） |
| `proxy_rules` | `Rules/MyProxyRules.list` | `Rules/MyProxyRules.list` |
| `direct_rules` | `Rules/MyDirectRules.list` | `Rules/MyDirectRules.list` |

注意 `path` 相对**客户端工作目录**（profile 目录），不是仓库根目录：把仓库挂成工作目录，或改用绝对路径。

历史债务：早期 yaml 写的是 `rules/reject_rule.list` / `rules/proxy_rule.list` / `rules/direct_rule.list` 三个仓库里不存在的名字（且 `Rules` 大小写在 Linux 上也不匹配），已修复。

## 修改提示

- 拒绝用 `REJECT-DROP` 还是 `REJECT`：看客户端是否支持 DROP。
- `proxy_rules` 出口是 `PROXY`；若你配置里没有 `PROXY` 组，改成实际代理组名（如 `🚀 节点选择`）。
