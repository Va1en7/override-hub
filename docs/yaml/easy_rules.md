# easy_rules（YAML）

- 源文件：[`yaml/easy_rules.yaml`](../../yaml/easy_rules.yaml)
- 配套列表：[`Rules/`](../../Rules/)（需按 path 对齐文件名）

## 用途

用本地三个 classical 规则文件做前置分流：拒绝 → 代理 → 直连。适合个人维护少量 list，而不是整包 ACL。

## 内容结构

```yaml
+rules:
  - RULE-SET,reject_rules,REJECT-DROP
  - RULE-SET,proxy_rules,GLOBAL
  - RULE-SET,direct_rules,DIRECT

rule-providers:
  reject_rules:  path: rules/reject_rule.list
  proxy_rules:   path: rules/proxy_rule.list
  direct_rules:  path: rules/direct_rule.list
```

要点：

- `+rules`：在原有规则**前**插入（具体合并语义以客户端为准）。
- provider 均为 `type: file`、`behavior: classical`、`format: text`。

## 与仓库 Rules/ 的关系

仓库里目前是：

- `Rules/MyDirectRules.list`
- `Rules/MyProxyRules.list`

而 `easy_rules.yaml` 默认 path 为：

- `rules/reject_rule.list`
- `rules/proxy_rule.list`
- `rules/direct_rule.list`

使用前请二选一：

1. 按 yaml 里的名字建 `rules/` 目录并放三个 list；或  
2. 改 yaml 的 `path` 指向现有 `Rules/My*.list`，并补 reject 列表。

## 修改提示

- 拒绝用 `REJECT-DROP` 还是 `REJECT`：看客户端是否支持 DROP。
- `proxy_rules` 出口写的是 `GLOBAL`，若你配置里没有 GLOBAL 组，改成实际代理组名（如 `PROXY` / `🚀 节点选择`）。
