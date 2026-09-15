# FastAPI学习导航

学习FastAPI前，建议先了解[[Web网络基础]]和[[RESTful API设计]]。

## 1. 推荐学习顺序

```text
FastAPI介绍 → FastAPI快速入门 → FastAPI路由 → FastAPI请求参数 → FastAPI响应模型 → FastAPI响应类型 → FastAPI异常处理 → FastAPI中间件
```

1. [[FastAPI介绍]]：了解FastAPI是什么、主要特点，以及我为什么优先学习它。
2. [[FastAPI快速入门]]：创建FastAPI实例，编写最小程序并运行服务。
3. [[FastAPI路由]]：理解路由、装饰器、请求方法、请求路径和处理函数。
4. [[FastAPI请求参数]]：学习路径参数、查询参数、`Path()`、`Query()`和请求体。
5. [[FastAPI响应模型]]：使用`response_model`约束JSON响应的数据结构。
6. [[FastAPI响应类型]]：重点掌握JSON、HTML和文件三种响应。
7. [[FastAPI异常处理]]：使用`HTTPException`返回明确的HTTP错误。
8. [[FastAPI中间件]]：为请求和响应添加统一的处理逻辑。

## 2. 相关笔记

- [[Python Web框架对比]]：比较FastAPI、Flask和Django的定位与适用场景。
- [[闭包与装饰器#六、FastAPI中的装饰器|FastAPI中的装饰器]]：补充理解`@app.get()`等写法。

## 3. 后续扩展规则

以后学习到依赖注入、数据库、认证和部署等独立主题时，我再建立对应笔记并加入这里。暂时不提前创建空白笔记，避免目录变得杂乱。
