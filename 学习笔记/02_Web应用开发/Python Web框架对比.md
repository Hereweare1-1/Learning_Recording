# Python Web框架对比

## 1. FastAPI、Flask和Django的区别

这三个都是Python Web框架，但它们的设计重点不同，不能只用“谁快、谁慢”来判断哪个更好。

| 对比维度 | FastAPI | Flask | Django |
| --- | --- | --- | --- |
| 主要定位 | 以API开发为核心的现代Web框架 | 轻量、灵活的Web框架 | 功能完整的Web框架 |
| 异步支持 | 基于ASGI设计，支持`async def`和`await` | 支持异步视图，但整体仍以WSGI设计为主 | 支持异步视图，使用ASGI时可以运行异步请求栈 |
| 数据校验 | 结合Python类型注解和Pydantic自动校验请求数据 | 核心框架通常需要手动处理或使用扩展 | 表单和模型有校验能力；开发API时通常还会配合其他工具 |
| API文档 | 根据OpenAPI自动生成Swagger UI和ReDoc文档 | 核心框架不自动生成，需要自行实现或使用扩展 | 核心框架不直接提供完整的REST API文档，通常需要配合其他工具 |
| 内置功能 | 偏向API所需功能 | 核心较小，按需选择扩展 | 内置ORM、后台管理、认证等功能 |
| 常见场景 | API、微服务、AI模型服务 | 小型Web项目、简单API、需要灵活组合的项目 | 数据库驱动的网站、内容系统和管理后台 |

## 2. 异步支持应该怎样理解

异步适合需要等待数据库、外部API或文件等I/O操作的场景，但使用`async`并不代表程序一定更快。

同步代码在等待一个耗时操作完成时，当前执行流程只能继续等待。异步代码可以在等待期间把处理机会交给其他任务，例如等待大模型返回结果时，服务器可以先处理其他用户的请求。

```text
用户A调用大模型 → 等待结果
                         ↘
              服务器先处理用户B的请求
```

- `async def`用于声明异步函数。
- `await`表示等待一个异步操作完成，等待期间程序可以处理其他任务。

- **FastAPI**：属于异步优先的ASGI框架，也可以混合使用普通的`def`和异步的`async def`。
- **Flask**：可以编写`async def`视图，但一次请求仍会占用一个工作进程，因此不是异步优先框架。
- **Django**：原生支持异步视图；在ASGI环境下可以使用异步请求栈，但部分代码或第三方组件仍可能是同步的。

下面只演示异步调用的基本流程，`call_llm()`表示一个支持异步的大模型调用函数：

```python
@app.get("/chat")
async def chat():
    result = await call_llm()
    return {"result": result}
```

异步特别适合调用大模型API、查询数据库和发送网络请求等需要等待的任务。模型训练、复杂图像处理和大量数学计算会持续占用CPU或GPU，异步通常不会让这些计算本身变快。

> [!summary] 简单记忆
> 异步的重点不是让一个任务完成得更快，而是等待一个任务时，不耽误处理其他任务。

## 3. 性能应该怎样比较

框架性能会受到业务代码、数据库、网络请求、部署方式和服务器配置等因素影响，不能简单地记成“FastAPI一定最快、Django一定最慢”。

我现阶段只需要记住：

- FastAPI针对API开发和并发I/O场景进行了良好设计。
- Flask轻量，但很多功能需要我自己选择和组合。
- Django内置功能多，适合需要ORM、认证和后台管理的完整网站。

我现阶段选择FastAPI的原因可以查看[[FastAPI介绍#3. 我现阶段为什么优先学习FastAPI]]。

## 4. 官方资料

- [FastAPI异步说明](https://fastapi.tiangolo.com/async/)
- [Flask异步说明](https://flask.palletsprojects.com/en/stable/async-await/)
- [Django异步说明](https://docs.djangoproject.com/en/stable/topics/async/)
