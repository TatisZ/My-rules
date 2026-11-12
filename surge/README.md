# Surge 规则集

由 `260927.yaml`（Egern）转换到 Surge 时，**无法直接复用现成规则**的那几份，在这里重新写成 Surge 原生格式。

格式：**Surge `RULE-SET`** —— 纯文本、每行一条经典规则、**不带 policy 列**。
引用方式（在主配置 `[Rule]` 里）：

```
RULE-SET,https://raw.githubusercontent.com/TatisZ/My-rules/main/surge/Direct.list,DIRECT
```

> 本目录里的 `.list` 都是「规则模式」文件，policy 由引用它的那一行决定，所以同一份列表可以被不同策略复用。
> 本仓库根目录下的 `Direct` / `jav` / `futu` / `antigravity` 是 **Egern YAML**（`domain_suffix_set:` 等），
> `Clash/` 下的是 Clash rule-provider（`payload:`）。两者 Surge 都不认，请不要在 Surge 配置里直接引用它们。

## 文件清单

| 文件 | 来源 | 转换说明 | 建议 policy |
| --- | --- | --- | --- |
| `Direct.list` | 根目录 `Direct`（Egern `domain_suffix_set`，13 条 → 13 条） | 逐条转 `DOMAIN-SUFFIX`；补 `20111020.xyz`（见 `Clash/Direct.yaml`，egern 版漏了） | `DIRECT` |
| `Jav.list` | 根目录 `jav`（Egern `domain_suffix_set`，5 条） | 逐条转 `DOMAIN-SUFFIX`；去掉 `javdb.com ` 的尾随空格 | 任意代理（原配置：`dmit-anytls`） |
| `LAN.list` | `260927.yaml` 里 `or: name=lan` 组合规则 | 拆成 9 条平铺规则（RULE-SET 逐行即 OR 语义）；`domain_keyword`→`DOMAIN-KEYWORD`，`.local`→`DOMAIN-SUFFIX,local`，IP 段加 `no-resolve` | `DIRECT` |
| `HK_Broker.list` | [LingJingMaster/Shadowrocket-Rules `HK_Broker.list`](https://github.com/LingJingMaster/Shadowrocket-Rules/blob/main/HK_Broker.list) | 原文件本身已是经典格式，但 egern 引用的是 GitHub blob 页面（非 raw）且不在 blackmatrix7 仓库里，故自托管并补标准头部；204 行规则原样保留 | `DIRECT` |

## 直接用现成的（本仓库不含）

大部分分流规则直接用了 [blackmatrix7/ios_rule_script/rule/Surge](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule) 的现成 `.list`：
`AppleMusic` / `Gemini` / `OpenAI` / `Disney` / `Telegram` / `WeChat` / `ChinaMax` / `ChinaMax_Domain`（DNS 分流用）。

注意仓库里的文件夹大小写：`WeChat`（不是 `Wechat`）、`ChinaMax`、`AppleMusic`；
`_Domain.list` 是 **DOMAIN-SET** 格式（裸域名，带前导点表示后缀匹配），引用时要用 `DOMAIN-SET,`，不能当 `RULE-SET` 用。

## 相关

- 主配置与逐项映射说明见本地 `Surge.conf` / `SURGE_CONVERSION_NOTES.md`（含节点密钥，不上传）。
- Surge 官方规则集格式：https://manual.nssurge.com/rules/ruleset.html
