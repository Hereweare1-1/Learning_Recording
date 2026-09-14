# FastAPI响应模型

## 1. response_model是什么

`response_model`是`@app.get()`、`@app.post()`等路径操作装饰器的参数，用于规定接口返回的JSON数据应该包含哪些字段，以及每个字段是什么类型。

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class News(BaseModel):
    id: int
    title: str
    content: str


@app.get("/news/{news_id}", response_model=News)
async def get_news(news_id: int):
    return {
        "id": news_id,
        "title": f"这是第{news_id}条新闻",
        "content": "这是新闻内容",
    }
```

FastAPI会按照`News`模型处理返回结果。

## 2. response_model的作用

- **检查数据**：检查接口返回的数据是否符合模型要求。
- **转换数据**：把返回结果转换成符合模型的JSON数据。
- **过滤字段**：只保留响应模型中定义的字段，避免返回不应暴露的数据。
- **生成文档**：在Swagger UI等接口文档中显示响应的数据结构。

例如，接口内部的数据中可能包含`password`，但响应模型中没有该字段，FastAPI就不会把它返回给客户端。这也是`response_model`能够提高数据安全性的原因。

## 3. 什么时候使用response_model

当接口返回JSON数据，并且我希望明确规定返回字段和字段类型时，适合使用`response_model`。

```text
返回JSON业务数据
       +
需要固定字段、校验数据或隐藏多余字段
       ↓
使用response_model
```

返回HTML或文件时，通常不使用`response_model`，而是使用[[FastAPI响应类型|响应类型]]。

## 4. response_model与响应类型的区别

| 写法 | 控制的内容 | 例子 |
| --- | --- | --- |
| `response_model=News` | JSON响应中有哪些字段、字段是什么类型 | 新闻详情、用户信息、商品列表 |
| `response_class=HTMLResponse` | 响应以什么格式发送 | HTML网页 |
| `return FileResponse(...)` | 直接创建并返回具体响应 | 文件下载 |

> [!summary] 简单记忆
> `response_model`管“返回的数据长什么样”，响应类型管“这些数据以什么形式发送”。

## 5. 官方资料

- [FastAPI响应模型](https://fastapi.tiangolo.com/tutorial/response-model/)
