# FastAPI异常处理

## 1. 为什么需要异常处理

客户端请求接口时，可能出现资源不存在、参数错误或没有访问权限等情况。对于这类能够预料的错误，可以使用FastAPI提供的`HTTPException`中断正常处理流程，并返回明确的HTTP错误响应。

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()


@app.get("/news/{news_id}")
async def get_news(news_id: int):
    news_id_list = [1, 2, 3, 4, 5, 6]

    if news_id not in news_id_list:
        raise HTTPException(
            status_code=404,
            detail="当前新闻不存在",
        )

    return {"id": news_id}
```

## 2. HTTPException中的参数

| 参数 | 作用 | 示例 |
| --- | --- | --- |
| `status_code` | 设置HTTP错误状态码 | `404` |
| `detail` | 告诉客户端具体的错误原因 | `"当前新闻不存在"` |

当客户端请求一个不存在的新闻时，会收到`404`状态码和下面的JSON响应：

```json
{
  "detail": "当前新闻不存在"
}
```

## 3. 为什么使用raise而不是return

`HTTPException`是一个异常，因此要使用`raise`抛出，而不是使用`return`返回。

```text
发现请求无法正常处理
        ↓
raise HTTPException(...)
        ↓
立即停止当前请求后面的代码
        ↓
FastAPI生成错误响应并返回给客户端
```

## 4. 常见的客户端错误状态码

| 状态码 | 含义 | 常见情况 |
| --- | --- | --- |
| `400` | 请求错误 | 请求内容不符合业务要求 |
| `401` | 未认证 | 没有登录或认证信息无效 |
| `403` | 禁止访问 | 已经认证，但没有操作权限 |
| `404` | 资源不存在 | 请求的新闻、用户或文件不存在 |

这些状态码属于`4xx`，通常表示客户端提交的请求无法完成。状态码的基础知识可以查看[[HTTP协议#4. HTTP响应|HTTP响应与状态码]]。

> [!summary] 简单记忆
> 能够预料的客户端请求错误，可以使用`raise HTTPException(status_code=..., detail=...)`立即结束处理，并告诉客户端哪里出了问题。

## 5. 官方资料

- [FastAPI错误处理](https://fastapi.tiangolo.com/tutorial/handling-errors/)
