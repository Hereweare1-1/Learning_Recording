# FastAPI基础

## 1. 什么是FastAPI

FastAPI是一个现代、快速、高性能的Python Web框架，可以用于开发服务端API接口。

建议先了解[[Web网络基础]]和[[RESTful API设计]]。

## 2. 使用FastAPI开发服务端接口

主要步骤：

- 导入FastAPI。
- 创建FastAPI实例对象。
- 创建路径操作函数，定义访问路径。
- 运行FastAPI服务。

```python
from fastapi import FastAPI

# 创建FastAPI实例
app = FastAPI()


# 定义API接口
@app.get("/")
def root():
    return {"message": "Hello World"}


# 定义API接口
@app.get("/users")
def get_users():
    return [
        {"id": 1, "name": "张三"},
        {"id": 2, "name": "李四"},
        {"id": 3, "name": "王五"},
    ]


# 启动服务
if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="0.0.0.0", port=8000)
```

## 3. 运行FastAPI服务

方式一：

```bash
fastapi dev xxxx.py
```

方式二：

```bash
uvicorn xxxx:app --reload
```

方式三：在代码中启动

```python
if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="0.0.0.0", port=8000)
```

## 4. FastAPI、Flask和Django的区别

这三个都是Python Web框架，但它们的设计重点不同，不能只用“谁快、谁慢”来判断哪个更好。

| 对比维度 | FastAPI | Flask | Django |
| --- | --- | --- | --- |
| 主要定位 | 以API开发为核心的现代Web框架 | 轻量、灵活的Web框架 | 功能完整的Web框架 |
| 异步支持 | 基于ASGI设计，支持`async def`和`await` | 支持异步视图，但整体仍以WSGI设计为主 | 支持异步视图，使用ASGI时可以运行异步请求栈 |
| 数据校验 | 结合Python类型注解和Pydantic自动校验请求数据 | 核心框架通常需要手动处理或使用扩展 | 表单和模型有校验能力；开发API时通常还会配合其他工具 |
| API文档 | 根据OpenAPI自动生成Swagger UI和ReDoc文档 | 核心框架不自动生成，需要自行实现或使用扩展 | 核心框架不直接提供完整的REST API文档，通常需要配合其他工具 |
| 内置功能 | 偏向API所需功能 | 核心较小，按需选择扩展 | 内置ORM、后台管理、认证等功能 |
| 常见场景 | API、微服务、AI模型服务 | 小型Web项目、简单API、需要灵活组合的项目 | 数据库驱动的网站、内容系统和管理后台 |

### 4.1 异步支持应该怎样理解

异步适合需要等待数据库、外部API或文件等I/O操作的场景，但使用`async`并不代表程序一定更快。

- **FastAPI**：属于异步优先的ASGI框架，也可以混合使用普通的`def`和异步的`async def`。
- **Flask**：可以编写`async def`视图，但一次请求仍会占用一个工作进程，因此不是异步优先框架。
- **Django**：原生支持异步视图；在ASGI环境下可以使用异步请求栈，但部分代码或第三方组件仍可能是同步的。

FastAPI中的异步接口示例：

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items")
async def get_items():
    return {"items": []}
```

### 4.2 性能应该怎样比较

框架性能会受到业务代码、数据库、网络请求、部署方式和服务器配置等因素影响，不能简单地记成“FastAPI一定最快、Django一定最慢”。

我现阶段只需要记住：

- FastAPI针对API开发和并发I/O场景进行了良好设计。
- Flask轻量，但很多功能需要我自己选择和组合。
- Django内置功能多，适合需要ORM、认证和后台管理的完整网站。

### 4.3 我现阶段为什么优先学习FastAPI

我的求职方向是智能应用开发，经常需要把大模型、算法或业务能力封装成API，因此FastAPI与当前学习目标比较匹配。

```text
AI模型或业务功能
       ↓
使用FastAPI封装成API
       ↓
前端、智能体或其他程序调用
```

这并不表示Flask和Django不好，而是三个框架适合解决的问题不同。现阶段我应先掌握FastAPI，之后再根据项目需要学习其他框架。

### 4.4 官方资料

- [FastAPI官方文档](https://fastapi.tiangolo.com/)
- [FastAPI异步说明](https://fastapi.tiangolo.com/async/)
- [Flask异步说明](https://flask.palletsprojects.com/en/stable/async-await/)
- [Django异步说明](https://docs.djangoproject.com/en/stable/topics/async/)
