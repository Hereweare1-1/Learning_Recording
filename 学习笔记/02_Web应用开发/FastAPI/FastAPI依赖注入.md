# FastAPI依赖注入

## 1. 依赖注入是什么

**依赖项**是可以被重复使用的函数或其他组件，负责提供某种功能或数据。

**依赖注入**是指路由函数先声明自己需要哪个依赖项，然后由FastAPI自动调用依赖项，并把它的返回结果传入路由函数。

```text
路由函数声明需要某个依赖项
          ↓
FastAPI自动调用依赖项
          ↓
获得依赖项的返回结果
          ↓
把结果传给路由函数
```

## 2. 为什么使用依赖注入

- **减少重复**：公共逻辑只写一次，可以供多个路由使用。
- **降低耦合**：把公共逻辑与具体的业务处理分开。
- **方便测试**：测试时可以使用模拟依赖替换真实依赖。

常见场景包括：

- 统一提取和校验请求参数。
- 共享数据库连接。
- 封装多个路由都会使用的业务逻辑。
- 验证用户身份、权限和角色。

## 3. 使用Depends声明依赖项

使用依赖注入的基本步骤是：

```text
创建依赖项 → 导入Depends → 在路由函数中声明依赖项
```

```python
from typing import Annotated

from fastapi import Depends, FastAPI, Query

app = FastAPI()


# 1. 创建依赖项
async def common_parameters(
    skip: int = Query(0, ge=0),
    limit: int = Query(10, ge=1, le=60),
):
    return {"skip": skip, "limit": limit}


# 2. 在路由函数中声明依赖项
@app.get("/news")
async def get_news_list(
    commons: Annotated[dict, Depends(common_parameters)],
):
    return commons
```

当客户端访问`/news?skip=0&limit=10`时：

1. FastAPI先调用`common_parameters()`。
2. `common_parameters()`读取并校验`skip`和`limit`。
3. 函数返回的字典被传给路由函数的`commons`参数。
4. `get_news_list()`使用`commons`继续处理请求。

> [!warning] 注意
> `Depends()`中传入函数名`common_parameters`，不要写成`common_parameters()`。依赖函数由FastAPI负责调用。

## 4. Depends的两种常见写法

官方更推荐使用`Annotated`：

```python
commons: Annotated[dict, Depends(common_parameters)]
```

也可以使用下面的写法：

```python
commons: dict = Depends(common_parameters)
```

两种写法的作用相同。我优先认识`Annotated`写法，同时也要能看懂普通写法。

## 5. 依赖注入和中间件的区别

| 对比项 | 依赖注入 | 中间件 |
| --- | --- | --- |
| 使用范围 | 可以按需要应用到指定路由 | 通常统一经过每个请求和响应 |
| 主要作用 | 为路由提供参数、数据或可复用功能 | 在请求前后执行统一处理逻辑 |
| 是否可以提供结果 | 可以把依赖项的返回结果注入路由函数 | 通常继续传递请求并返回响应 |

中间件的基本用法见：[[FastAPI中间件]]。

## 6. 官方资料

- [FastAPI依赖项](https://fastapi.tiangolo.com/tutorial/dependencies/)
