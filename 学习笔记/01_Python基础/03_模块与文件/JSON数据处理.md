# JSON数据处理

这篇笔记整理JSON的基本概念，以及Python读写JSON文件的方法。

## 1. JSON是什么

**JSON**是一种用于程序之间传递和保存结构化数据的格式。它的写法和Python字典、列表很像：

```json
{
  "name": "张三",
  "age": 20,
  "hobbies": ["音乐", "游戏"]
}
```

> [!important] JSON不等于Python字典
> JSON是一种数据格式；Python读取JSON后，通常会得到`dict`、`list`等Python对象。

API经常使用JSON传递数据，因为它结构清晰、跨语言，并且容易被程序解析。API相关概念可以查看[[Web网络基础#10. API是什么]]。

## 2. JSON文件

### 2.1 导入json模块

```python
import json
```

使用 Python 自带的 `json` 模块来处理 JSON 数据。

---

### 2.2 写入JSON数据文件：json.dump()

```python
user = {
    "name": "涛哥",
    "age": 18,
    "gender": "男",
    "hobbies": ["reading", "swimming"]
}

with open("resources/user.json", "w", encoding="utf-8") as f:
    json.dump(user, f, ensure_ascii=False, indent=2)
```

#### json.dump()

```python
json.dump(数据, 文件对象)
```

作用：

> 将 Python 中的数据写入 JSON 文件。
#### 常用参数

##### ensure_ascii

```python
ensure_ascii=False
```

作用：

>默认为True:所有的数据输出的数据都是ascll编码(非ASCII码会进行转义); 
>False:非ASCII码保留原样输出。

默认：

```python
ensure_ascii=True
```

中文等非 ASCII 字符可能会被转换成 Unicode 转义形式。

设置：

```python
ensure_ascii=False
```

可以让中文直接保存。

---

##### indent

```python
indent=2
```

作用：

> 给 JSON 数据添加缩进，让 JSON 文件格式更加清晰、易读。

例如：

```json
{
  "name": "涛哥",
  "age": 18
}
```

---

### 2.3 读取JSON数据文件：json.load()

```python
with open("resources/user.json", "r", encoding="utf-8") as f:
    user = json.load(f)
    print(user)
```

#### json.load()

```python
json.load(文件对象)
```

作用：

> 从 JSON 文件中读取数据，并解析成 Python 数据。

例如：

```python
user = json.load(f)
```

读取 JSON 文件后，`user` 通常会得到一个 Python 字典。

例如 JSON 文件：

```json
{
  "name": "涛哥",
  "age": 18,
  "gender": "男"
}
```

读取后可以：

```python
print(user["name"])
print(user["age"])
```
