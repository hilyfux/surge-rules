# surge-rules

iOS Shadowrocket / macOS Surge 个人分流配置。专注于：

- **海外 AI** (OpenAI / Claude / Gemini / Grok / Perplexity / Mistral / HuggingFace 等) → 新加坡
- **中国 AI** (通义 / 文心 / 豆包 / Kimi / DeepSeek / 智谱 / 星火 / 百川 / 混元 / MiniMax / 天工) → 直连
- **中国政务/金融** (政府 / 教育 / 医疗 / 银行 / 保险 / 券商 / 支付) → 直连
- **Apple 拆中海外** (iCloud-CN/推送 直连，App Store/Apple News/TV/Music/TestFlight 走代理)
- **Google 全部走代理**

## 配置链接（Shadowrocket 远程订阅）

```
https://raw.githubusercontent.com/hilyfux/surge-rules/main/Shadowrocket/auto-router.conf
```

iPhone：Shadowrocket → 配置 → 添加（来源选择"URL"）→ 粘贴上面链接 → 下载 → 选中并连接。

## 路由策略表

| 优先级 | 类别 | 出口 |
|---|---|---|
| 1 | LAN / 局域网 / DNS Fallback | DIRECT |
| 2 | 中国政府 / 教育 / 国防 / 法院 | DIRECT |
| 3 | 中国医疗（三甲医院 / 医保 / 互联网医疗） | DIRECT |
| 4 | 中国银行（工建农中招交浦光大兴业等 + 微众/网商） | DIRECT |
| 5 | 中国保险（人保/平安/太保/泰康/友邦等） | DIRECT |
| 6 | 中国券商 + 行情资讯（华泰/中信/国君/广发/东财/同花顺等） | DIRECT |
| 7 | 中国支付（支付宝 / 微信 / 银联 / 京东 / 拼多多） | DIRECT |
| 8 | 中国 AI 工具 | DIRECT |
| 9 | 海外 AI 工具 | 新加坡 |
| 10 | Apple 海外（App Store / News / TV / Music / TestFlight） | 节点选择 |
| 10 | Apple 国内（iCloud-CN / 系统推送） | DIRECT |
| 10 | iCloud Private Relay | REJECT |
| 11 | Google（搜索 / Gmail / Voice / FCM / YouTube） | 节点选择 / 流媒体 |
| 12 | Telegram / Twitter / Discord / TikTok | 社交媒体 |
| 13 | Netflix / Spotify / Steam | 流媒体 |
| 14 | PayPal / 海外支付 | 海外支付 |
| 15 | Microsoft / Bing | 节点选择 |
| 16 | ChinaMax 大集合 + GEOIP,CN | DIRECT |
| 17 | FINAL | 自动选择 |

## Proxy Group

- **节点选择** — 顶层手动覆盖入口
- **自动选择** — 跨地区 url-test 选最快（300s 间隔，80ms 容差，过滤试用/到期等假节点）
- **故障转移** — fallback 类型，主节点掉线时按 SG → HK → JP → US 自动切换（180s 健康检查）
- **地区组** — 香港 / 台湾 / 日本 / 新加坡 / 美国（每组内 url-test）
- **服务组** — AI海外 / Google / Apple海外 / 社交媒体 / 流媒体 / 海外支付 / G2A

## 规则集来源

| 类别 | 来源 |
|---|---|
| 海外 AI 综合（含 Grok/Perplexity/Poe/Meta AI 等） | [SukkaW/Surge](https://github.com/SukkaW/Surge) |
| OpenAI / Claude / Gemini / Copilot / Apple / Google / Microsoft | [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) |
| 中国大陆域名 / IP 兜底 | blackmatrix7 ChinaMax + Shadowrocket 内置 GEOIP,CN |
| 中国 AI / 银行 / 保险 / 政府 / 医院 / 券商 | 内联（公开仓库无成熟规则集，本仓库手工维护） |

## DNS

- 主：阿里 DoH3 + 腾讯 DoH + 223.5.5.5 + 119.29.29.29
- DNS 直连域名（防 .cn 被劫持）：always-real-ip
- 国内 DNS 失败回落代理 DNS

## 致谢

- [@SukkaW](https://github.com/SukkaW) — 维护极高质量的 AI 综合规则
- [@blackmatrix7](https://github.com/blackmatrix7) — iOS 规则脚本最大集合
