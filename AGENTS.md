# AGENTS.md — override-hub

给编码代理（Pi / Claude Code 等）的项目说明。人读总览见 [README.md](./README.md) 与 [docs/README.md](./docs/README.md)。

## 项目是什么

自用 **Clash / Mihomo** 覆写规则仓库，不是应用代码库。

- **产物形态**：YAML 覆写、JavaScript `main(config)` 脚本、classical `*.list` 规则。
- **消费者**：Mihomo / Clash Verge / Mihomo Party 等客户端的「覆写 / Override / 脚本」。
- **文档**：`docs/` 说明用途与字段；**以源文件为准**，文档不整份粘贴大配置。

## 目录约定

```text
override-hub/
├── yaml/           # YAML 覆写（策略组 / rule-providers / rules / 可选 dns）
├── javascript/     # JS 覆写，必须 function main(config) { ...; return config }
├── Rules/          # 自建 classical 列表（直连 / 代理等）
├── docs/           # 与源文件对应的 Markdown 说明
│   ├── README.md
│   ├── yaml/
│   ├── javascript/
│   └── rules/
├── README.md       # 仓库入口索引 + raw 链接
└── AGENTS.md       # 本文件
```

| 路径 | 可改？ | 说明 |
|------|--------|------|
| `yaml/*` | 是 | 覆写正文 |
| `javascript/*` | 是 | 脚本覆写正文 |
| `Rules/*` | 是 | 个人 list |
| `docs/*` | 是 | 随源文件同步更新 |
| `README.md` | 是 | 增删源文件时更新索引 |
| `.orca/` | 否（默认） | 本地/工具状态，除非用户点名 |

## 语言与回复

- 与用户沟通默认 **中文**。
- 配置键名、规则类型、组名保持源文件原样（可中英混合，如 `PROXY`、`🚀 节点选择`）。
- Commit / PR 说明若用户未指定，用简洁中文或中英均可；不要擅自改 git 历史。

## 改文件前必做

1. 读目标源文件，不要只凭 `docs/` 臆造内容。
2. 若改的是「布丁狗」逻辑，同时核对：
   - `yaml/布丁狗的订阅转换.yaml`
   - `javascript/布丁狗的订阅转换.js`
   两者应对齐；只改一侧时在回复里明确说明并建议同步。
3. 若改 ACL4SSR 系，区分：
   - `ACL4SSR_Online_Full.yaml`（无 DNS）
   - `ACL4SSR_Online_Full_WithIcon.yaml`（图标 + DNS）
   不要把 DNS 段误拷进 Full 版，除非用户要求。
4. 改 `Rules/*.list` 或 `easy_rules.yaml` 时，核对 **path / 文件名** 是否一致（见下方已知债）。

## 编辑规范

### YAML

- `+rules`：表示向原配置 **追加/前置** 规则（合并语义以客户端为准），不要无故改成整表 `rules:` 覆盖，除非用户要整包替换。
- `rule-providers`：保留 `type` / `behavior` / `format` / `interval` / `url` 或 `path` 字段完整。
- 规则书写优先与仓库现有风格一致（部分文件对 RULE-SET 使用带引号字符串）。
- 出站名必须是最终配置里存在的组（`DIRECT` / `REJECT` / `PROXY` / 自定义中文组名等）。
- CDN 现多用 `testingcf.jsdelivr.net`；替换镜像时同一文件内保持一致。

### JavaScript

- 入口必须是：`function main(config) { ...; return config }`。
- 区分 **整表替换** 与 **合并**：
  - 布丁狗脚本会重写 `proxy-groups`、`rules`；合并 `rule-providers` 用 `Object.assign`。
  - 防 DNS 泄露、图标脚本应尽量非破坏：缺字段先建、有则改。
- 多个 JS 叠加时注意顺序：后执行的整表 `rules = [...]` 会冲掉先插入的规则。文档/回复里要提醒。
- 不要引入 Node 专用 API（`fs`、`require` 等），除非用户确认客户端支持。
- 不要在脚本里写订阅 token、代理密码等密钥。

### Rules（`*.list`）

- classical 一行一条；**list 内一般不写出站**，出站在 `RULE-SET,provider,出站` 上指定。
- 常用类型：`DOMAIN-SUFFIX`、`DOMAIN`、`PROCESS-NAME`、`IP-CIDR` 等。
- 游戏/客户端优先考虑 `PROCESS-NAME`。
- 保持 UTF-8；避免无意义空行堆积（文件末单行换行可保留）。

### 文档（`docs/`）

源文件变更后 **同步** 对应文档（无对应篇则补一篇并挂到 `docs/README.md`）：

| 源 | 文档 |
|----|------|
| `yaml/布丁狗的订阅转换.yaml` | `docs/yaml/布丁狗的订阅转换.md` |
| `yaml/ACL4SSR_Online_Full.yaml` | `docs/yaml/ACL4SSR_Online_Full.md` |
| `yaml/ACL4SSR_Online_Full_WithIcon.yaml` | `docs/yaml/ACL4SSR_Online_Full_WithIcon.md` |
| `yaml/easy_rules.yaml` | `docs/yaml/easy_rules.md` |
| `yaml/添加直连规则.yaml` | `docs/yaml/添加直连规则.md` |
| `javascript/布丁狗的订阅转换.js` | `docs/javascript/布丁狗的订阅转换.md` |
| `javascript/防止dns泄露(雾).js` | `docs/javascript/防止dns泄露.md` |
| `javascript/正则匹配设置代理组图标.js` | `docs/javascript/正则匹配设置代理组图标.md` |
| `Rules/*` | `docs/rules/自建规则.md` |

文档只写：用途、关键组/规则流向、修改注意点、与相关文件差异。  
**禁止**把整份 ACL 大 YAML 贴进 Markdown。

增删源文件时更新：

- `README.md` 索引与 raw 链接（如适用）
- `docs/README.md` 快速索引表

## 已知技术债（改前先看）

1. **`easy_rules.yaml` path 与仓库文件名不一致**
   - yaml 默认：`rules/reject_rule.list`、`proxy_rule.list`、`direct_rule.list`
   - 仓库实际：`Rules/MyDirectRules.list`、`Rules/MyProxyRules.list`（无 reject 列表）
   - 修改任一侧时要么对齐 path/命名，要么在文档与回复中写清仍不兼容。
2. **防 DNS 泄露脚本依赖配置中存在 `MATCH` 规则**；无 MATCH 时不会插入 RULE-SET。
3. **图标 / 规则集依赖外网 CDN 或 GitHub raw**；用户环境不稳时应允许改 mirror 或 `type: file`。

## 任务类型怎么做

| 用户意图 | 建议动作 |
|----------|----------|
| 加个人直连/代理域名或进程 | 改 `Rules/*.list`，必要时改引用该 list 的 yaml；更新 `docs/rules/自建规则.md` |
| 改 AIGC/TG/Google 分流 | 改布丁狗 yaml/js（尽量双端同步）+ 对应 docs |
| 改广告/流媒体/微软等大分流 | 改 ACL4SSR 对应 yaml + docs；WithIcon 的 DNS 单独评估 |
| 只加几条临时规则 | 优先 `添加直连规则.yaml` 或 `+rules`，避免复制整包 ACL |
| 补 DNS / fake-ip | 优先现有 WithIcon DNS 或 `防止dns泄露(雾).js`，避免重复冲突段 |
| 只补策略组图标 | `正则匹配设置代理组图标.js` 的 `iconMap` |
| 只整理说明 | 只动 `docs/` / `README.md`，不改源行为 |

## 验证（完成前）

本仓库无统一测试套件。完成前至少做静态检查：

1. YAML：缩进、列表 `-`、关键键是否还在；`rules` / `proxy-groups` / `rule-providers` 名称交叉引用是否存在。
2. JS：`main` 是否 `return config`；括号/引号是否平衡；对 `config.rules` 是否先判存在再 `.find` / `.unshift`（按脚本原有防护级别，增强时勿引入空引用）。
3. list：无误用全角逗号；无多余出站字段（除非故意写成完整规则）。
4. 文档与索引是否已同步。
5. 不要声称「已在客户端实测通过」，除非用户环境里真的跑过。

可用只读检查示例（按需）：

```bash
# 文件是否存在、行数是否异常缩水
wc -l yaml/* javascript/* Rules/* docs/**/*.md

# 粗查 JS 语法（有 node 时）
node --check javascript/*.js
```

## 安全与隐私

- 禁止提交或写入：订阅 token、代理密码、Cookie、API Key、私人服务器完整凭证。
- 用户粘贴的订阅 URL 若含 token，文档与回复中脱敏。
- 不要把全局 `FIRECRAWL_API_KEY` 等环境密钥写进本仓库。
- 本仓库是网络分流配置；不要添加与覆写无关的恶意规则（劫持、未说明的全局 REJECT 等）。用户明确要求的拦截/拒绝除外。

## 不要做的事

- 未经要求把整个仓库「重构」成另一套规则体系（如全部换成单一模板）。
- 删除用户 `Rules/` 里的个人条目却不说明。
- 用应用项目的 TDD/构建流程硬套本仓库（无 package.json 构建链时不要虚构 npm 脚本）。
- 在未请求时执行 `git push`、改 remote、或 force push。
- 把 `.orca/worktrees` 等本地状态当源文件提交。

## 优先级

1. 用户明确指令  
2. 不破坏现有覆写可加载性  
3. 源文件与 `docs/` / 根 README 索引一致  
4. 布丁狗 yaml/js 双端一致性  
5. 风格与仓库既有写法一致  

## 相关文档入口

- 使用与选型：[docs/README.md](./docs/README.md)
- 源文件索引：[README.md](./README.md)
