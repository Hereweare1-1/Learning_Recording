# SQLAlchemy数据类型与映射

SQLAlchemy ORM使用Python类映射数据库表，使用类属性映射数据库字段。一个字段声明同时包含Python类型和数据库列配置：

```python
name: Mapped[str] = mapped_column(String(100))
```

## 1. ORM字段声明

```python
name: Mapped[str] = mapped_column(String(100))
```

这行代码分为三部分：

```text
name                  属性名
Mapped[str]           Python一侧的类型说明
mapped_column(...)    数据库列的类型和配置
```

### 1.1 Mapped

`Mapped[...]`是SQLAlchemy ORM使用的类型注解，表示这个类属性会映射到数据库字段。

```python
name: Mapped[str]
```

其中的`str`表示读取`name`后，Python中得到的是字符串。

Python类型注解用于说明数据类型，方便阅读、编辑器检查以及框架读取。类型注解本身不会自动转换数据，也不会在普通Python代码中阻止错误赋值。

类型注解的基本用法见：[[FastAPI请求参数#1.1 Python类型注解和Pydantic怎样完成自动校验|Python类型注解和Pydantic怎样完成自动校验]]。

### 1.2 mapped_column

`mapped_column()`用于建立ORM属性与数据库列之间的映射，并设置列类型、主键、默认值等数据库配置。

```python
id: Mapped[int] = mapped_column(primary_key=True)
name: Mapped[str] = mapped_column(String(100))
```

- `primary_key=True`：把`id`设置为主键。
- `String(100)`：使用SQLAlchemy的字符串类型，并把最大长度设置为100。

`Mapped[...]`说明Python中使用什么类型，`mapped_column(...)`说明数据库列如何定义。

## 2. Python、SQLAlchemy与MySQL类型

Python类型描述数据进入程序后的形式，MySQL字段类型描述数据在数据库中的存储形式。SQLAlchemy位于两者之间，使用自己的数据类型连接Python和数据库。

```text
Python类型 ←→ SQLAlchemy类型 ←→ MySQL字段类型
```

常用类型的对应关系如下：

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

SQLAlchemy类型是通用描述，最终生成的字段类型会受到数据库种类和字段配置影响。MySQL字段类型见：[[MySQL数据类型]]。

### 2.1 datetime、DateTime与DATETIME

```python
create_time: Mapped[datetime] = mapped_column(DateTime)
```

这行代码包含三个不同层次的时间类型：

```text
Python datetime ←→ SQLAlchemy DateTime ←→ MySQL DATETIME
```

- `datetime`：Python程序中使用的日期时间类型。
- `DateTime`：SQLAlchemy描述日期时间列的数据类型。
- `DATETIME`：MySQL数据表中的日期时间字段类型。

Python的`datetime`类来自标准库中的`datetime`模块，不是内置类型，使用前需要导入：

```python
from datetime import datetime

now = datetime.now()
```

也可以先导入模块，再通过`datetime.datetime`使用其中的类：

```python
import datetime

now = datetime.datetime.now()
```

## 3. SQLAlchemy 2.0的类型推断

SQLAlchemy 2.0可以读取`Mapped[...]`中的Python类型，并推断对应的SQLAlchemy列类型。

```python
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

## 4. 完整ORM模型

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

模型中的类、对象和属性与数据库中的表、记录和字段对应：

```text
Book类          ↔ book表
Book对象        ↔ book表中的一行记录
Book对象的属性   ↔ book表中的字段
```

各字段的类型映射如下：

| ORM字段 | Python类型 | SQLAlchemy类型 | MySQL字段类型 |
| --- | --- | --- | --- |
| `id` | `int` | 根据`Mapped[int]`推断为`Integer` | `INT` |
| `name` | `str` | `String(100)` | `VARCHAR(100)` |
| `price` | `Decimal` | `Numeric(10, 2)` | `DECIMAL(10, 2)` |
| `create_time` | `datetime` | `DateTime` | `DATETIME` |
