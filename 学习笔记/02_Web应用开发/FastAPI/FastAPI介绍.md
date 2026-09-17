# FastAPI介绍

## 1. 什么是FastAPI

FastAPI是一个现代、快速、高性能的Python Web框架，可以用于开发服务端API接口。

学习前建议先了解[[Web网络基础]]和[[RESTful API设计]]。

## 2. 主要特点

- 以API开发为核心，适合开发API、微服务和AI模型服务。
- 支持普通的`def`和异步的`async def`。
- 结合Python类型注解和Pydantic，可以[[FastAPI请求参数#1.1 Python类型注解和Pydantic怎样完成自动校验|自动校验请求数据]]。
- 根据OpenAPI自动生成Swagger UI和ReDoc接口文档。

FastAPI与Flask、Django的详细区别可以查看[[Python Web框架对比]]。

## 3. FastAPI在智能应用开发中的适用场景

智能应用开发经常需要把大模型、算法或业务能力封装成API，FastAPI的类型提示、异步支持和自动接口文档适合这类场景。

```text
AI模型或业务功能
       ↓
使用FastAPI封装成API
       ↓
前端、智能体或其他程序调用
```

FastAPI、Flask和Django适合解决的问题不同，具体区别见[[Python Web框架对比]]。

## 4. 官方资料

- [FastAPI官方文档](https://fastapi.tiangolo.com/)
