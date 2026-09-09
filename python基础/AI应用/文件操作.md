````
# Python 文件读写常用方法

# 1. 打开文件：open()
file = open("test.txt", "r", encoding="utf-8")
````

### 常用模式

| 模式     | 含义            |     |
| ------ | ------------- | --- |
| `"r"`  | 读取文件          |     |
| `"w"`  | 写入文件，会覆盖原内容   |     |
| `"a"`  | 追加内容，在文件末尾继续写 |     |
| `"r+"` | 读取 + 写入       |     |

---

## 2. 读取全部内容：read()

```
with open("test.txt", "r", encoding="utf-8") as file:
    content = file.read()
    print(content)
```
---

## 3. 读取一行：readline()

```
with open("test.txt", "r", encoding="utf-8") as file:
    line = file.readline()
    print(line)
```

连续调用可以继续读取下一行：

```
line1 = file.readline()
line2 = file.readline()
line3 = file.readline()
```

---

## 4. 读取所有行：readlines()

```
with open("test.txt", "r", encoding="utf-8") as file:
    lines = file.readlines()
    print(lines)
```

`readlines()`：读取所有行，返回一个列表。

例如文件内容：

```
第一行
第二行
第三行
```

读取后：

```
[
    "第一行\n",
    "第二行\n",
    "第三行\n"
]
```

---

## 5. 写入内容：write()

```
with open("test.txt", "w", encoding="utf-8") as file:
    file.write("你好")
```

`write()`：向文件中写入字符串。

### 注意

使用 `"w"` 模式时：

> 如果文件原来有内容，会覆盖原来的内容。

---

## 6. 追加内容：a

```
with open("test.txt", "a", encoding="utf-8") as file:
    file.write("新的内容")
```

使用 `"a"` 模式：

> 不会删除原来的内容，而是在文件末尾继续写入。

---

## 7. 推荐写法：with open()

推荐使用：

```
with open("test.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

`with` 会在文件操作完成后自动关闭文件（即使发生异常）。

因此不需要手动：

```
file.close()
```

---

# 重点速记

```
open()       → 打开文件

"r"          → 读取
"w"          → 写入（覆盖）
"a"          → 追加

read()       → 读取全部内容
readline()   → 读取一行
readlines()  → 读取所有行，返回列表

write()      → 写入内容

with open()  → 推荐的文件操作方式

encoding="utf-8" → 指定文件使用 UTF-8 编码
```

## JSON文件
````
import json
````

使用 Python 自带的 `json` 模块来处理 JSON 数据。

---

## 2. 写入 JSON 数据文件：json.dump()

```
user = {
    "name": "涛哥",
    "age": 18,
    "gender": "男",
    "hobbies": ["reading", "swimming"]
}

with open("resources/user.json", "w", encoding="utf-8") as f:
    json.dump(user, f, ensure_ascii=False, indent=2)
```

### json.dump()

```
json.dump(数据, 文件对象)
```

作用：

> 将 Python 中的数据写入 JSON 文件。
### 常用参数

#### ensure_ascii

```
ensure_ascii=False
```

作用：

>默认为True:所有的数据输出的数据都是ascll编码(非ASCII码会进行转义); 
>False:非ASCII码保留原样输出。

默认：

```
ensure_ascii=True
```

中文等非 ASCII 字符可能会被转换成 Unicode 转义形式。

设置：

```
ensure_ascii=False
```

可以让中文直接保存。

---

#### indent

```
indent=2
```

作用：

> 给 JSON 数据添加缩进，让 JSON 文件格式更加清晰、易读。

例如：

```
{
  "name": "涛哥",
  "age": 18
}
```

---

## 3. 读取 JSON 数据文件：json.load()

```
with open("resources/user.json", "r", encoding="utf-8") as f:
    user = json.load(f)
    print(user)
```

### json.load()

```
json.load(文件对象)
```

作用：

> 从 JSON 文件中读取数据，并解析成 Python 数据。

例如：

```
user = json.load(f)
```

读取 JSON 文件后，`user` 通常会得到一个 Python 字典。

例如 JSON 文件：

```
{
  "name": "涛哥",
  "age": 18,
  "gender": "男"
}
```

读取后可以：

```
print(user["name"])
print(user["age"])
```
