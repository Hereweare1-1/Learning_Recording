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
fastapi dev main.py
```

### 2.2 使用Uvicorn命令

```bash
uvicorn main:app --reload
```

### 2.3 在Python代码中启动

#### 2.3.1 直接传入FastAPI实例

不使用自动重载时，可以直接把`app`对象传给Uvicorn：

```python
if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="0.0.0.0", port=8000)
```

#### 2.3.2 使用自动重载

开发时如果希望保存代码后自动重启服务，需要把应用写成`"模块名:实例名"`的字符串：

```python
if __name__ == "__main__":
    import uvicorn

    uvicorn.run(
        "main:app",
        host="0.0.0.0",
        port=8000,
        reload=True,
    )
```

假设代码保存在`main.py`中，并且创建了`app = FastAPI()`：

```text
"main:app"
   │    └─ FastAPI实例名
   └────── Python文件名，不写.py
```

使用`reload=True`时，应把`uvicorn.run()`放在`if __name__ == "__main__"`中，避免Uvicorn重新加载代码时重复启动。

### 2.4 启动参数

| 参数 | 作用 |
| --- | --- |
| `app`或`"main:app"` | 指定要运行的FastAPI应用 |
| `host="0.0.0.0"` | 监听这台电脑的所有网卡 |
| `port=8000` | 使用`8000`端口提供服务 |
| `reload=True` | 保存代码后自动重启，适合开发阶段 |

`0.0.0.0`表示监听范围，不是通常在浏览器中输入的访问地址。在当前电脑上一般访问：

```text
http://127.0.0.1:8000
```

### 2.5 使用局域网中的其他设备访问

当`host="0.0.0.0"`时，可以在Windows终端中执行下面的命令查看局域网IP：

```bash
ipconfig
```

在当前使用的无线网卡或以太网适配器中找到“IPv4地址”，例如`192.168.1.10`。同一局域网中的其他设备可以尝试访问：

```text
http://192.168.1.10:8000
```

`127.0.0.1`只代表当前电脑自己，不是局域网IP。如果其他设备无法访问，还需要检查Windows防火墙是否允许Python或`8000`端口通过。

## 3. 官方资料

- [Uvicorn配置说明](https://www.uvicorn.org/settings/)
