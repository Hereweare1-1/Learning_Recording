# HTTP协议

这篇笔记整理HTTP协议的特点、请求数据格式和响应数据格式。

## 1. HTTP是什么


**HTTP**全称为HyperText Transfer Protocol，中文是“超文本传输协议”。它是客户端和服务器之间进行通信的一套规则。

```text
客户端 → HTTP请求 → 服务器
客户端 ← HTTP响应 ← 服务器
```

## 2. HTTP的基本特点

### 请求—响应模型

通常由客户端先发送一次请求，服务器处理后返回一次响应。

```text
客户端发送请求 → 服务器处理请求 → 服务器返回响应
```

### 无状态

HTTP本身不会自动记住前后两次请求来自同一个用户，每次请求在协议层面都是相对独立的。

实际网站可以使用Cookie、Session或Token保存登录状态，所以“HTTP无状态”不等于“网站不能记住用户”。

### 数据格式与传输方式

- HTTP规定了请求和响应的结构，但请求体和响应体不一定只能传递文本，也可以传递图片、视频、JSON等数据。
- HTTP/1.1中的请求行和请求头是文本形式，因此学习时可以直接阅读。
- HTTP/1.1和HTTP/2通常通过TCP传输；HTTP/3使用基于UDP的QUIC协议。

> [!note] 注意版本差异
> 我可以先把HTTP理解为“基于请求—响应模型的应用层协议”，但也要知道：“HTTP全部基于文本、底层一定使用TCP”只适合初步理解HTTP/1.1，并不适用于所有HTTP版本。

## 3. HTTP请求

一个HTTP请求通常包含：

- **URL**：请求哪个地址。
- **请求方法**：要进行什么操作，例如`GET`、`POST`、`PUT`、`DELETE`。
- **Headers（请求头）**：携带身份认证、数据类型等额外信息。
- **Body（请求体）**：发送给服务器的数据，API中经常使用JSON格式。

### HTTP请求报文的结构

下面是一段简化后的HTTP请求：

```http
POST /api/courses HTTP/1.1
Host: localhost:90
Accept: application/json
Content-Type: application/json
Authorization: Bearer example-token

{"name": "Python", "status": 1}
```

它由三部分组成：

```text
请求行
请求头
空行
请求体（可以没有）
```

**请求行**

请求行位于第一行，由请求方法、资源路径和HTTP版本组成：

```text
POST /api/courses HTTP/1.1
```

| 内容 | 示例 | 作用 |
| --- | --- | --- |
| 请求方法 | `POST` | 表示要进行什么操作 |
| 资源路径 | `/api/courses` | 表示请求哪个资源 |
| HTTP版本 | `HTTP/1.1` | 表示使用的HTTP版本 |

**请求头**

请求头采用`名称: 值`的格式，每一行表示一项附加信息。

| 常见请求头 | 作用 |
| --- | --- |
| `Host` | 目标服务器的域名或地址 |
| `Accept` | 客户端希望接收的数据类型 |
| `Content-Type` | 请求体的数据类型，例如`application/json` |
| `Authorization` | 身份认证信息，例如Token |
| `Content-Length` | 请求体的字节长度 |

**请求体**

请求体用于携带提交给服务器的数据，并不是每个请求都有请求体。发送JSON数据时，需要把`Content-Type`设置为`application/json`。

```json
{
  "name": "Python",
  "status": 1
}
```

### GET和POST如何携带参数

`GET`请求通常把参数写在URL的查询字符串中：

```http
GET /api/courses?name=Python&status=1 HTTP/1.1
```

- `?`表示查询参数开始。
- 多个参数使用`&`连接。
- 因为参数会显示在URL中，所以不适合直接放密码等敏感信息。
- `GET`请求通常不使用请求体。

`POST`请求通常把要提交的数据放在请求体中：

```json
{
  "name": "Python",
  "status": 1
}
```

| 对比项 | GET | POST |
| --- | --- | --- |
| 常见用途 | 查询、获取数据 | 提交、新增数据或执行操作 |
| 参数常见位置 | URL查询字符串 | 请求体 |
| 是否通常有请求体 | 否 | 可以有 |
| 大小限制 | URL长度会受到浏览器、服务器等限制 | 协议没有统一的固定上限，但服务器和框架通常会限制请求体大小 |

> [!important] 注意
> 不能简单地记成“POST请求大小没有限制”。实际项目中的Web服务器、网关和后端框架通常都会设置大小上限。

请求方法的具体用法可以查看[[RESTful API设计]]。

## 4. HTTP响应

服务器处理请求后会返回HTTP响应，响应通常包含状态码和响应数据。

### HTTP响应报文的结构

下面是一段简化后的HTTP响应：

```http
HTTP/1.1 200 OK
Server: nginx
Content-Type: application/json
Connection: keep-alive

{"code": 1, "message": "success", "data": null}
```

它由三部分组成：

```text
响应行
响应头
空行
响应体（可以没有）
```

**响应行**

响应行位于第一行，由HTTP版本、状态码和状态说明组成：

```text
HTTP/1.1 200 OK
```

| 内容 | 示例 | 作用 |
| --- | --- | --- |
| HTTP版本 | `HTTP/1.1` | 表示使用的HTTP版本 |
| 状态码 | `200` | 使用数字说明处理结果 |
| 状态说明 | `OK` | 对状态码的简短文字说明 |

**响应头**

响应头和请求头一样，也采用`名称: 值`的格式。

| 常见响应头 | 作用 |
| --- | --- |
| `Server` | 服务器软件信息 |
| `Date` | 服务器产生响应的时间 |
| `Content-Type` | 响应体的数据类型，例如`application/json` |
| `Content-Length` | 响应体的字节长度 |
| `Connection` | 当前连接的管理方式 |

**响应体**

响应体存放服务器返回的实际数据，可以是JSON、HTML、图片或其他内容。API通常返回JSON数据：

```json
{
  "code": 1,
  "message": "success",
  "data": null
}
```

常见状态码：

| 状态码 | 含义 |
| ---: | --- |
| `200` | 请求成功 |
| `400` | 请求参数有问题 |
| `401` | 没有通过身份认证 |
| `403` | 没有访问权限 |
| `404` | 请求的资源不存在 |
| `500` | 服务器内部错误 |
