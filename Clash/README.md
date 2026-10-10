# Clash / mihomo 规则集

本目录是 `My-rules` 仓库**根目录 egern 源文件**的 Clash 转换产物，
供 mihomo / Clash.Meta 的 `rule-providers` 以 `type: http` 直接拉取。

| 文件 | egern 源 | behavior | format |
|---|---|---|---|
| `futu.yaml` | `../futu` | classical | yaml |
| `antigravity.yaml` | `../antigravity` | classical | yaml |
| `Direct.yaml` | `../Direct` | classical | yaml |
| `jav.yaml` | `../jav` | classical | yaml |

## raw 地址

```
https://raw.githubusercontent.com/TatisZ/My-rules/main/Clash/futu.yaml
https://raw.githubusercontent.com/TatisZ/My-rules/main/Clash/antigravity.yaml
https://raw.githubusercontent.com/TatisZ/My-rules/main/Clash/Direct.yaml
https://raw.githubusercontent.com/TatisZ/My-rules/main/Clash/jav.yaml
```

## mihomo 引用示例

```yaml
rule-providers:
  futu:
    type: http
    behavior: classical
    format: yaml
    url: "https://raw.githubusercontent.com/TatisZ/My-rules/main/Clash/futu.yaml"
    path: ./ruleset/gh/futu.yaml
    interval: 86400
```

## 重新生成

egern 的 `*_set` 语法（`domain_suffix_set:` 等）mihomo 解析不了，必须转换：

```bash
python3 ~/.config/mihomo/egern2clash.py <egern源文件> \
    -o Clash/<名字>.yaml --name <NAME> --source <源URL>
```

转换规则：

- `domain_set` → `DOMAIN,`
- `domain_suffix_set` → `DOMAIN-SUFFIX,`
- `domain_keyword_set` → `DOMAIN-KEYWORD,`
- `domain_wildcard_set` → `DOMAIN-WILDCARD,`
- `ip_cidr_set` → `IP-CIDR,`（源文件若声明 `no_resolve: true`，则整集追加 `,no-resolve`）

`no_resolve` 由 egern 源文件顶部的声明决定，脚本默认值为 `false`：

| 文件 | 源声明 | 产物中 `,no-resolve` 条数 |
|---|---|---|
| `futu.yaml` | `no_resolve: true` | 64 |
| `antigravity.yaml` | `no_resolve: false` | 0 |
| `Direct.yaml` | 无声明（= false） | 0 |
| `jav.yaml` | 无声明（= false） | 0 |

即只有 `ip_cidr_set` 会受 `no_resolve` 影响；域名类规则不受影响。
