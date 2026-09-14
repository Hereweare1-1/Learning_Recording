# FastAPI快速入门

## 1. 使用FastAPI开发服务端接口

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

其中，`@app.get()`定义接口的方式可以查看[[FastAPI路由]]，函数参数的写法可以查看[[FastAPI请求参数]]。

## 2. 运行FastAPI服务

### 2.1 使用FastAPI命令

```bash
fastapi dev xxxx.py
```

### 2.2 使用Uvicorn命令

```bash
uvicorn xxxx:app --reload
```

### 2.3 在Python代码中启动

```python
if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="0.0.0.0", port=8000)
```
