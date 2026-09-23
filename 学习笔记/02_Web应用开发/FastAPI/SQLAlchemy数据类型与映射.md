# SQLAlchemy数据类型与映射

阅读SQLAlchemy ORM模型前，我需要先区分Python类型、SQLAlchemy类型和MySQL字段类型。它们处在不同层次，但会在ORM模型中建立对应关系。

```text
Python类型注解
      ↓
Python标准库datetime
      ↓
SQLAlchemy数据类型
      ↓
MySQL字段类型
      ↓
SQLAlchemy ORM模型
```

## 1. Python类型注解

Python类型注解用于说明变量、函数参数、返回值或对象属性应该是什么类型。

```python
name: str = "Python编程"
price: float = 59.9
```

其中，`str`说明`name`应该是字符串，`float`说明`price`应该是浮点数。

类型注解主要用于表达代码中的类型信息，方便阅读、编辑器检查以及框架读取。它本身不会自动把错误的数据转换成正确类型，也不会在普通Python代码中自动阻止错误赋值。

FastAPI中类型注解的基本用法见：[[FastAPI请求参数#1.1 Python类型注解和Pydantic怎样完成自动校验|Python类型注解和Pydantic怎样完成自动校验]]。

## 2. Python标准库datetime

`datetime`不是Python内置类型，而是Python标准库`datetime`模块提供的日期时间类。使用前需要先导入。

### 2.1 导入模块

```python
import datetime

now = datetime.datetime.now()
```

第一个`datetime`是模块名，第二个`datetime`是模块中的类名。

### 2.2 直接导入类

```python
from datetime import datetime

now = datetime.now()
```

这种写法把`datetime`类直接导入当前文件，因此使用时不需要再写模块名。

```python
print(now)
print(type(now))
```

输出结果类似：

```text
2026-09-23 15:30:20.123456
<class 'datetime.datetime'>
```

`<class 'datetime.datetime'>`表示这个对象来自`datetime`模块中的`datetime`类。

## 3. SQLAlchemy为什么有独立的数据类型

Python类型描述数据进入程序后以什么形式存在，MySQL字段类型描述数据在数据库中如何保存。SQLAlchemy位于Python程序和数据库之间，因此提供了一套自己的数据类型来连接两边。

```text
Python类型 ←→ SQLAlchemy类型 ←→ MySQL字段类型
```

例如，同一个创建时间在三个位置使用不同的类型名称：

```text
Python datetime
        ↕
SQLAlchemy DateTime
        ↕
MySQL DATETIME
```

- `datetime`：Python程序取得数据后使用的时间对象类型。
- `DateTime`：SQLAlchemy用来描述日期时间列的类型。
- `DATETIME`：MySQL数据表中真正使用的字段类型。

SQLAlchemy使用统一类型描述字段，再根据当前连接的数据库转换成对应的数据库字段类型。因此，同一套ORM模型可以适配不同的数据库。

MySQL自身的数据类型见：[[MySQL数据类型]]。

## 4. 常用类型对应关系

| Python类型 | SQLAlchemy类型 | 常见MySQL字段类型 |
| --- | --- | --- |
| `int` | `Integer` | `INT` |
| `str` | `String(100)` | `VARCHAR(100)` |
| `float` | `Float` | `FLOAT` |
| `bool` | `Boolean` | `BOOLEAN`，在MySQL中等价于`TINYINT(1)` |
| `datetime` | `DateTime` | `DATETIME` |
| `date` | `Date` | `DATE` |
| `time` | `Time` | `TIME` |
| `bytes` | `LargeBinary` | `BLOB` |
| `Decimal` | `Numeric(10, 2)` | `DECIMAL(10, 2)` |
| `dict`、`list` | `JSON` | `JSON` |
| `str` | `Text` | `TEXT` |

表中的SQLAlchemy类型是通用写法。最终生成的字段类型会受到数据库种类和字段配置影响。

## 5. Mapped与mapped_column的分工

SQLAlchemy 2.0的ORM模型通常使用下面的写法声明字段：

```python
name: Mapped[str] = mapped_column(String(100))
```

这行代码可以分成三部分：

```text
name                  属性名
Mapped[str]           Python一侧的类型说明
mapped_column(...)    数据库列的类型和配置
```

### 5.1 Mapped

`Mapped[...]`是SQLAlchemy ORM使用的类型注解，表示这个类属性会映射到数据库字段。

```python
name: Mapped[str]
```

其中的`str`表示读取`name`后，Python中得到的是字符串。

```python
create_time: Mapped[datetime]
```

其中的`datetime`表示读取`create_time`后，Python中得到的是`datetime`对象。

### 5.2 mapped_column

`mapped_column()`用于建立ORM属性与数据库列之间的映射，并设置列类型、主键、默认值等数据库配置。

```python
id: Mapped[int] = mapped_column(primary_key=True)
name: Mapped[str] = mapped_column(String(100))
```

- `primary_key=True`：把`id`设置为主键。
- `String(100)`：使用SQLAlchemy的字符串类型，并把最大长度设置为100。

因此，`Mapped[...]`重点说明Python中使用什么类型，`mapped_column(...)`重点说明数据库列如何定义。

## 6. SQLAlchemy 2.0的类型推断

SQLAlchemy 2.0可以读取`Mapped[...]`中的Python类型，并推断对应的SQLAlchemy列类型。

```python
from datetime import datetime

from sqlalchemy.orm import Mapped, mapped_column


id: Mapped[int] = mapped_column(primary_key=True)
create_time: Mapped[datetime] = mapped_column()
```

在这段代码中：

- SQLAlchemy可以根据`Mapped[int]`推断整数类型。
- SQLAlchemy可以根据`Mapped[datetime]`推断日期时间类型。

需要补充数据库字段细节时，仍然要在`mapped_column()`中显式填写类型或配置：

```python
name: Mapped[str] = mapped_column(String(100))
price: Mapped[Decimal] = mapped_column(Numeric(10, 2))
```

MySQL的`VARCHAR`需要指定长度，因此字符串字段通常明确写成`String(100)`、`String(255)`等形式。金额等需要固定精度的小数也应该明确写出`Numeric`的精度和小数位数。

## 7. 完整ORM模型

```python
from datetime import datetime
from decimal import Decimal

from sqlalchemy import DateTime, Numeric, String
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column


class Base(DeclarativeBase):
    pass


class Book(Base):
    __tablename__ = "book"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(100))
    price: Mapped[Decimal] = mapped_column(Numeric(10, 2))
    create_time: Mapped[datetime] = mapped_column(DateTime)
```

模型中的类、对象和属性会与数据库中的表、记录和字段建立映射：

```text
Book类          ↔ book表
Book对象        ↔ book表中的一行记录
Book对象的属性   ↔ book表中的字段
```

以`create_time`为例：

```python
create_time: Mapped[datetime] = mapped_column(DateTime)
```

- `create_time`是Python类的属性名，也是默认生成的数据库字段名。
- `Mapped[datetime]`表示Python中读取到的是`datetime`对象。
- `DateTime`是SQLAlchemy的数据类型。
- SQLAlchemy连接MySQL时，会把它转换成MySQL的`DATETIME`字段。

这个模型对应的核心MySQL字段类型类似：

```sql
CREATE TABLE book (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    price DECIMAL(10, 2),
    create_time DATETIME
);
```

这段SQL只用于展示字段类型之间的对应关系。ORM实际生成的完整建表语句还会包含SQLAlchemy根据模型配置确定的其他约束。
