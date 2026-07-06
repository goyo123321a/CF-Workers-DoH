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
https://doh.cmliussss.hidns.co/dns-query
```

- 还可使用 Cloudflare 回源端口 `2053`、`2083`、`2087`、`2096`、`8443`，例如
```url
https://doh.cmliussss.hidns.co:2053/dns-query
```

- 如您设置了`TOKEN`变量为 **CMLiussss**，则
```url
https://doh.cmliussss.hidns.co/CMLiussss
```
### 2️⃣ 附加功能 IP信息查询

#### 🔍 查询当前IP信息
```url
https://doh.cmliussss.hidns.co/ip-info
```

- 如您设置了`TOKEN`变量为 **CMLiussss**，则
```url
https://doh.cmliussss.hidns.co/ip-info?token=CMLiussss
```

#### 🔍 查询指定IP信息
```url
https://doh.cmliussss.hidns.co/ip-info?ip=8.8.8.8
```

- 如您设置了`TOKEN`变量为 **CMLiussss**，则

```url
https://doh.cmliussss.hidns.co/ip-info?ip=8.8.8.8&token=CMLiussss
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
  "query": "8.8.8.8"
}
```

> [!NOTE]
> 请将示例中的 `doh.cmliussss.hidns.co` 替换为你实际部署的域名

## 🔧 变量说明

### 📋 环境变量

| 变量名 | 是否必须 | 默认值 | 说明 |
|--------|----------|--------|------|
| DOH_PATH | 否 | dns-query |  服务端点路径（优先级最高） dns-query my-dns, query, doh |
| TOKEN | 否 | dns-query | 备选 DoH 路径 |
| ADMIN_USER | 否 | admin | 管理员登录用户名 admin |
| ADMIN_PASS | 否 | 123321 | 管理员登录密码 123321 |

### KV变量(名称随意)
DOH_CONFIG

### Curl使用示例
# GET 请求 - A记录 (IPv4)
```
curl -H "accept: application/dns-json" \
  "https://cfdoh.xxas.xx.kg/cf-doh?name=google.com&type=A"
```

# GET 请求带 Accept 头- AAA记录 (ipv6)
```
curl -H "accept: application/dns-json" \
  "https://cfdoh.xxas.xx.kg/cf-doh?name=google.com&type=AAA"
```

# GET 请求 – Wire Format（?dns=）
# 查询 google.com A 记录（Base64URL 编码示例）
```
curl -H "accept: application/dns-message" \
  "https://cfdoh.xxas.xx.kg/cf-doh?dns=AAABAAABAAAAAAAAB2V4YW1wbGUDY29tAAABAAE"
```
预期：返回二进制 DNS 数据（终端会显示乱码，这是正常的）。
验证响应头：content-type: application/dns-message

# POST 请求 - JSON格式 (A记录)
```
curl -X POST -H "Content-Type: application/dns-json" \
  -d '{"name":"google.com","type":"A"}' \
  "https://cfdoh.xxas.xx.kg/cf-doh"
```

# POST 请求 - 表单格式 (A记录)
```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" \
  -d "name=google.com&type=A" \
  "https://cfdoh.xxas.xx.kg/cf-doh"
```

# POST 请求 – Wire Format（原始二进制）
```bash
echo -n "AAABAAABAAAAAAAAB2V4YW1wbGUDY29tAAABAAE" | base64 -d > query.bin
curl -X POST -H "Content-Type: application/dns-message" --data-binary @query.bin \
  "https://cfdoh.xxas.xx.kg/cf-doh"
```

# 浏览器访问 (直接显示JSON)
https://cfdoh.xxas.xx.kg/cf-doh?name=google.com&type=A

# 浏览器配置 DoH
Chrome/Edge: 设置 → 隐私和安全 → 安全 → 使用安全 DNS → 自定义
填入: https://cfdoh.xxas.xx.kg/cf-doh

## ⭐ Star 星星走起
[![Stargazers over time](https://starchart.cc/cmliu/CF-Workers-DoH.svg?variant=adaptive)](https://starchart.cc/cmliu/CF-Workers-DoH)

## 💡 技术特性
- 基于 Cloudflare Workers 无服务器架构
- 使用原生 JavaScript 实现

## 📝 许可证
本项目开源使用，欢迎自由部署和修改！

## 🙏 鸣谢
[tina-hello](https://github.com/tina-hello/doh-cf-workers)、[ip-api](https://ip-api.com/)、Cloudflare、GPT
