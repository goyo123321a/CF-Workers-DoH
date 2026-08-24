# 📶 CF-Workers-DoH
![img](./img.png)

CF-Workers-DoH 是一个基于 Cloudflare Workers 构建的 DNS over HTTPS (DoH) 解析服务。它允许你通过 HTTPS 协议进行 DNS 查询，提高查询的安全性和隐私保护。

> [!CAUTION]
> **doh.cmliussss.hidns.co 已被GFW阻断，需自行部署使用。**

## 🚀 部署方式

- **Workers** 部署：复制 [_worker.js](https://github.com/goyo123321a/CF-Workers-DoH/blob/main/_worker.js) 代码，`保存并部署`即可
- **Pages** 部署：`Fork` 后 `连接GitHub` 一键部署即可

## 📖 使用方法

假设你已部署成功，你的服务域名为：`doh.cmliussss.hidns.co`

### 1️⃣ DNS解析服务 (DoH)

将以下地址添加到支持DoH的设备或软件中：

```url
https://doh.goyo123.work.gd/cf-doh
```

- 还可使用 Cloudflare 回源端口 `2053`、`2083`、`2087`、`2096`、`8443`，例如
```url
https://doh.goyo123.work.gd:2053/cf-doh
```

- 还可以通过后台更改路径
```url
https://doh.goyo123.work.gd/cf-doh
```
### 2️⃣ 附加功能 IP信息查询

#### 🔍 查询当前IP信息
```url
https://doh.goyo123.work.gd/ip-info
```
- 带TOKEN
```url
https://doh.goyo123.work.gd/ip-info?token=123456
```
#### 🔍 查询指定IP信息
```url
https://doh.goyo123.work.gd/ip-info?ip=8.8.8.8
```
- 带TOKEN
```url
https://doh.goyo123.work.gd/ip-info?ip=8.8.8.8&token=123456
```
#### 📝 **返回信息示例**
```json
{
  "status": "success",
  "country": "美国",
  "countryCode": "US",
  "region": "VA",
  "regionName": "弗吉尼亚州",
  "city": "Ashburn",
  "zip": "20149",
  "lat": 39.03,
  "lon": -77.5,
  "timezone": "America/New_York",
  "isp": "Google LLC",
  "org": "Google Public DNS",
  "as": "AS15169 Google LLC",
  "query": "8.8.8.8",
  "timestamp": "2026-07-18T13:27:25.017Z"
}
```

#### 🔍 查询多个指定IP信息
```url
https://doh.goyo123.work.gd/ip-info?ips=8.8.8.8,1.1.1.1
```
- 带TOKEN
```url
https://doh.goyo123.work.gd/ip-info?ips=8.8.8.8,1.1.1.1&token=123456
```
#### 📝 **返回信息示例**
```json
{
  "8.8.8.8": {
    "status": "success",
    "country": "美国",
    "countryCode": "US",
    "region": "VA",
    "regionName": "弗吉尼亚州",
    "city": "Ashburn",
    "zip": "20149",
    "lat": 39.03,
    "lon": -77.5,
    "timezone": "America/New_York",
    "isp": "Google LLC",
    "org": "Google Public DNS",
    "as": "AS15169 Google LLC",
    "query": "8.8.8.8",
    "timestamp": "2026-08-19T12:44:24.096Z"
  },
  "1.1.1.1": {
    "status": "success",
    "country": "澳大利亞",
    "countryCode": "AU",
    "region": "QLD",
    "regionName": "Queensland",
    "city": "South Brisbane",
    "zip": "4101",
    "lat": -27.4766,
    "lon": 153.0166,
    "timezone": "Australia/Brisbane",
    "isp": "Cloudflare, Inc",
    "org": "APNIC and Cloudflare DNS Resolver project",
    "as": "AS13335 Cloudflare, Inc.",
    "query": "1.1.1.1",
    "timestamp": "2026-08-19T12:44:37.906Z"
  }
}
```


> [!NOTE]
> 请将示例中的 `doh.goyo123.work.gd` 替换为你实际部署的域名

## 🔧 变量说明

### 📋 环境变量

| 变量名 | 是否必须 | 默认值 | 说明 |
|--------|----------|--------|------|
| ADMIN_USER | 否 | admin | 管理员登录用户名 admin |
| ADMIN_PASS | 否 | 123321 | 管理员登录密码 123321 |
| SESSION_TTL | 否 | 3600 | 管理会话有效期（秒） 3600 |
| DOH_PATH | 否 | dns-query | 端点路径(如 my-doh) |
| TOKEN | 否 | 空 | 保护 /ip-info 接口的令牌（随机字符串） 空（不设则接口开放） |

### KV绑定用(创建KV名称随意)
DOH_CONFIG

### Curl使用示例
# GET 请求 - A记录 (IPv4)
```
curl "https://doh.goyo123.work.gd/cf-doh?name=google.com&type=A"
```

# GET 请求带 Accept 头- AAAA记录 (ipv6)
```
curl -H "accept: application/dns-json" \
  "https://doh.goyo123.work.gd/cf-doh?name=google.com&type=AAAA"
```

# GET 请求 – Wire Format（?dns=）
# 查询 google.com A 记录（Base64URL 编码示例）
```
curl -H "accept: application/dns-message" \
  "https://doh.goyo123.work.gd/cf-doh?dns=AAABAAABAAAAAAAAB2V4YW1wbGUDY29tAAABAAE"
```
预期：返回二进制 DNS 数据（终端会显示乱码，这是正常的）。
验证响应头：content-type: application/dns-message

# POST 请求 - JSON格式 (A记录)
```
curl -X POST -H "Content-Type: application/dns-json" \
  -d '{"name":"google.com","type":"A"}' \
  "https://doh.goyo123.work.gd/cf-doh"
```

# POST 请求 - 表单格式 (A记录)
```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" \
  -d "name=google.com&type=A" \
  "https://doh.goyo123.work.gd/cf-doh"
```

# POST 请求 – Wire Format（原始二进制）
```bash
echo -n "AAABAAABAAAAAAAAB2V4YW1wbGUDY29tAAABAAE" | base64 -d > query.bin
curl -X POST -H "Content-Type: application/dns-message" --data-binary @query.bin \
  "https://doh.goyo123.work.gd/cf-doh"
```

# 浏览器访问 (直接显示JSON)
https://doh.goyo123.work.gd/cf-doh?name=google.com&type=A

# 浏览器配置 DoH
Chrome/Edge: 设置 → 隐私和安全 → 安全 → 使用安全 DNS → 自定义
填入: https://doh.goyo123.work.gd/cf-doh

# 第一次向上游请求（MISS）
```bash
curl -i "https://doh.goyo123.work.gd/cf-doh?name=google.com&type=A"
```
# 第二次命中缓存（HIT）
```bash
curl -i "https://doh.goyo123.work.gd/cf-doh?name=google.com&type=A"
```
### 📦 DoH 缓存机制说明

本文档详细说明当前 DoH（DNS-over-HTTPS）代理服务的缓存策略、键生成规则、过期机制以及跨格式、跨地域的行为，帮助您理解并高效利用缓存。

---

1. 缓存类型

服务维护两种独立的缓存：

缓存类型 用途 存储内容 缓存键前缀
JSON 缓存 存储 JSON 格式的 DNS 响应 完整的 JSON 对象（含 Status、Answer 等） dns:
二进制缓存 存储 application/dns-message 格式的原始 DNS 响应 ArrayBuffer（二进制数据） binary:

两种缓存互不干扰，但会互相填充（见第 5 节）。

---

2. 缓存键生成规则

2.1 JSON 缓存键

```javascript
dnsCacheKey(domain, type)
```

· 格式：dns:域名:类型标识
· 域名统一转为 小写。
· 类型标识规则：
  · 若 type 为严格的大写字符串 "ALL"，则标识为 "ALL"。
  · 其他类型（如 "A"、"AAAA"、"NS"、"all" 等）均通过 normalizeType() 转为标准数字：
    · A → 1
    · AAAA → 28
    · NS → 2
    · all（小写）→ 0
    · ALL（大写）→ 特殊处理为 "ALL"（不是数字 0）

2.2 二进制缓存键

```javascript
binaryCacheKey(domain, type)
```

· 格式：binary:域名:类型数字
· 域名统一转为小写。
· 类型必须为数字（例如 1、28、2），不区分大小写（因为二进制查询解析出的类型就是数字）。

---

3. 缓存过期策略（TTL）

· TTL 来源：从上游 DNS 响应中提取所有记录（Answer）的最小 TTL 值。
· 兜底 TTL：若响应无记录，则使用默认值 60 秒。
· 强制范围：最终 TTL 限制在 60 ~ 86400 秒（1 分钟至 24 小时）。
· 缓存头设置：通过 Cache-Control: max-age=${ttl} 控制缓存有效期。

注意：不同边缘节点的缓存独立，TTL 从各自缓存写入时刻开始倒计时，因此同一时刻不同节点可能返回不同的剩余 TTL。

---

4. 大小写敏感（ALL vs all）

· type=ALL（大写）：
  · JSON 缓存键为 dns:域名:ALL，独立于小写 all。
  · 该分支会向上游请求 type=255（所有记录），返回完整 DNS 记录（含 MX、TXT 等）。
  · 不会拆分 A/AAAA 缓存，仅独立缓存完整响应。
· type=all（小写）：
  · JSON 缓存键为 dns:域名:0（数字 0）。
  · 该分支并发请求 A、AAAA、NS 三种类型，只返回这三种记录。
  · 会拆分 A 和 AAAA 分别存入单类型缓存（JSON + 二进制），供后续单独查询使用。
· 目的：满足不同客户端的差异化需求，同时避免缓存污染。

---

5. 跨格式缓存共享（双向填充）

为实现缓存最大化利用，服务采用了双向转换机制：

· JSON → 二进制：当 JSON 查询命中或从上游获取后，会调用 jsonResponseToWire() 转换为二进制响应，并通过 setBinaryCache() 存入二进制缓存。

· 二进制 → JSON：当二进制查询命中或从上游获取后，会调用 dnsWireToJson() 转换为 JSON 响应，并通过 setDnsCache() 存入 JSON 缓存。

因此，无论客户端使用哪种格式（application/dns-json 或 application/dns-message），后续相同域名和类型的任意格式请求都能命中缓存。

---

6. 单类型缓存与 type=all 的关系

· type=all（小写） 查询后，会从合并结果中提取 A 和 AAAA 记录，分别存入单类型缓存（键为 dns:域名:1 和 dns:域名:28）。
· 单独 type=A 或 type=AAAA 查询 会首先查找自己的单类型缓存，若未命中则尝试从 all 缓存中提取对应记录（回退逻辑），从而保证一致性。
· type=ALL（大写） 不参与拆分，也不影响上述缓存。

---

7. 地理节点与缓存独立性

· Cloudflare Workers 的 caches.default 在每个边缘节点（PoP）上独立存储，不跨节点共享。
· 不同地区的客户端 可能被分配到不同的边缘节点，因此：
  · 每个节点的缓存内容可能不同（因为上游 DoH 会根据出口 IP 返回就近 IP）。
  · 同一地区内的所有请求共享该节点的缓存，实现快速响应。
· 跨节点缓存独立是必要的，以保证地域化解析结果（地理负载均衡），避免用户拿到远端 IP 导致访问延迟。

---

8. 缓存命中标识

响应头中包含以下字段，方便调试：

字段 说明
X-Cache HIT 或 MISS，表示当前请求是否命中缓存。
X-Cache-Key 显示本次请求使用的缓存键，便于定位缓存条目。
X-Cache-Status 由 setDnsCache / setBinaryCache 添加，标记写入缓存成功。

---

9. 管理建议

· 监控缓存命中率：可通过 X-Cache 头或 Cloudflare 分析面板观察。
· 调整 TTL 范围：若需修改最小/最大 TTL 限制，可修改 extractMinTtlFromBinary 和缓存写入逻辑中的边界值。
· 清空缓存：缓存会自动过期，也可通过修改缓存键（如变更 doh_path）间接失效，但无需手动干预。

---

10. 常见问题

Q1：为什么 curl 和面板返回的 TTL 不同？

A：即使在同一节点，不同时刻请求也会因 TTL 倒计时导致剩余时间不同。若来自不同节点，则缓存独立，差异更明显。

Q2：type=ALL 和 type=all 的缓存能否共用？

A：不能，因为缓存键不同（ALL 与 0），且返回内容不同（完整 vs 仅 A/AAAA/NS）。

Q3：二进制和 JSON 缓存是否共享？

A：不共享存储空间，但数据会互相填充，因此命中率极高。

Q4：缓存是否会影响上游 DoH 的地理负载均衡？

A：不会，每个节点独立缓存来自上游的响应，只是加速重复查询，不改变上游返回的原始 IP。

---

如需进一步调整缓存行为，可修改代码中的相关函数（setDnsCache、setBinaryCache、dnsCacheKey、binaryCacheKey），但建议保持现有设计以充分利用边缘缓存优势。

## ⭐ Star 星星走起
[![Stargazers over time](https://starchart.cc/cmliu/CF-Workers-DoH.svg?variant=adaptive)](https://starchart.cc/cmliu/CF-Workers-DoH)

## 💡 技术特性
- 基于 Cloudflare Workers 无服务器架构
- 使用原生 JavaScript 实现

## 📝 许可证
本项目开源使用，欢迎自由部署和修改！

## 🙏 鸣谢
[tina-hello](https://github.com/tina-hello/doh-cf-workers)、[ip-api](https://ip-api.com/)、Cloudflare、GPT
