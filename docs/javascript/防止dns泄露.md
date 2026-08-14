# 防止 DNS 泄露（雾）（JavaScript）

- 源文件：[`javascript/防止dns泄露(雾).js`](../../javascript/防止dns泄露(雾).js)

## 用途

1. 增加 `prevent_dns_leak` 规则集（domain list）。  
2. 把该 RULE-SET 插到规则表尽量靠前，出站跟最终 `MATCH` 同一组。  
3. 强制 `dns.enhanced-mode = fake-ip`。

## 逻辑摘要

```text
若无 rule-providers → 建空对象
写入 prevent_dns_leak (http, domain, text, 日更)
找到 rules 里 MATCH 那条 → 取出出站名
rules.unshift(RULE-SET,prevent_dns_leak,<MATCH出站>)
dns.enhanced-mode = fake-ip
```

规则集 URL：

`https://raw.githubusercontent.com/xishang0128/rules/main/clash%20or%20stash/prevent_dns_leak/prevent_dns_leak_domain.list`

## 注意

- 配置里**没有 MATCH 规则**时，脚本不会插入 RULE-SET（`name` 为 null）。
- 与已有 fake-ip DNS 可叠加；若你故意用 redir-host，此脚本会改回 fake-ip。
- GitHub raw 不稳时可改 mirror 或下载到本地 file provider。
- 「雾」表示启发式防护，不能替代系统级 DNS 管控，也不保证所有泄露场景。

## 推荐组合

先挂分流覆写（ACL / 布丁狗），再挂本脚本，避免本脚本插入的规则被后续整表 `rules = [...]` 冲掉。  
若分流 JS 会重写整个 `rules`，应把防泄露逻辑合并进同一脚本末尾，或放在最后执行的覆写层。
