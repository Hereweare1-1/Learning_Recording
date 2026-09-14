# FastAPI介绍

## 1. 什么是FastAPI

FastAPI是一个现代、快速、高性能的Python Web框架，可以用于开发服务端API接口。

学习前建议先了解[[Web网络基础]]和[[RESTful API设计]]。

## 2. 主要特点

- 以API开发为核心，适合开发API、微服务和AI模型服务。
- 支持普通的`def`和异步的`async def`。
- 结合Python类型注解和Pydantic，可以自动校验请求数据。
- 根据OpenAPI自动生成Swagger UI和ReDoc接口文档。

FastAPI与Flask、Django的详细区别可以查看[[Python Web框架对比]]。

## 3. 我现阶段为什么优先学习FastAPI

我的求职方向是智能应用开发，经常需要把大模型、算法或业务能力封装成API，因此FastAPI与我当前的学习目标比较匹配。

```text
AI模型或业务功能
       ↓
使用FastAPI封装成API
       ↓
前端、智能体或其他程序调用
```

这并不表示Flask和Django不好，而是三个框架适合解决的问题不同。现阶段我应先掌握FastAPI，之后再根据项目需要学习其他框架。

## 4. 官方资料

- [FastAPI官方文档](https://fastapi.tiangolo.com/)
