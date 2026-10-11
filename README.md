# clash-rules

自己托管的 Clash / FlClash 规则集。改 `rules/` 里的文件，push 到 `main` 后，订阅会按 `interval` 自动拉新版本。

CDN 地址：

```text
https://cdn.jsdelivr.net/gh/xiaoliduck/clash-rules@main/rules/<文件名>
```

## 日常怎么改

```bash
cd ~/Documents/GitHub/clash-rules
# 编辑 rules/ai.list 或 rules/custom-proxy.list 等
git add rules/
git commit -m "update ai.list"
git push
```

立刻刷新 jsDelivr 缓存（改完想马上生效时打开一次）：

```text
https://purge.jsdelivr.net/gh/xiaoliduck/clash-rules@main/rules/ai.list
```

然后在 FlClash 里对该规则集点更新，或等配置里的 `interval`。

规则写法（Surge `.list`，不要在行尾写策略名）：

```text
DOMAIN,chat.openai.com
DOMAIN-SUFFIX,grok.com
DOMAIN-KEYWORD,telegram
IP-CIDR,91.108.4.0/22,no-resolve
PROCESS-NAME,Telegram
```

## 文件说明

| 文件 | 用途 |
|------|------|
| `rules/ai.list` | Grok / Cursor / Claude / 常用 AI |
| `rules/custom-proxy.list` | 强制走代理，自己加 |
| `rules/custom-direct.list` | 强制直连，自己加（优先级更高） |
| `rules/telegram.list` 等 | 从 blackmatrix7 Surge 列表拷来的底稿，可直接改 |
| `clash-template.yaml` | 机场 Clash 订阅模板，已指向本仓库 |

GFW / 国内 IP 段等大文件仍走 [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)，体积大、会自动更新，不必拷进本仓库。

## 机场模板里怎么引用

```yaml
rule-providers:
  ai:
    type: http
    behavior: classical
    format: text
    interval: 3600
    path: ./ruleset/ai.list
    url: https://cdn.jsdelivr.net/gh/xiaoliduck/clash-rules@main/rules/ai.list

rules:
  - RULE-SET,custom-direct,DIRECT
  - RULE-SET,custom-proxy,🔰国外流量
  - RULE-SET,ai,🔰国外流量
```

仓库必须公开，jsDelivr 和 FlClash 才能拉到。不要改成私有仓库。
