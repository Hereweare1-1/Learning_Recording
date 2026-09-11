# Web网站的组成

## 1. Web网站的组成部分

- 前端程序：负责界面展示。
- 后端程序：负责业务逻辑处理。
- 数据库：负责数据的存储与管理。

## 2. 前端网页程序的组成部分

- HTML：负责网页的结构和内容。
- CSS：负责网页的样式。
- JavaScript（JS）：负责网页的动作行为和交互效果。
-
# FastAPI基础

## 1. 什么是FastAPI

FastAPI是一个现代、快速、高性能的Python Web框架，可以用于开发服务端API接口。

## 2. 使用FastAPI开发服务端接口

主要步骤：

- 导入FastAPI。
- 创建FastAPI实例对象。
- 创建路径操作函数，定义访问路径。
- 运行FastAPI服务。

```
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

```
fastapi dev xxxx.py
```

方式二：

```
uvicorn xxxx:app --reload
```

方式三：在代码中启动

```
if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="0.0.0.0", port=8000)
```