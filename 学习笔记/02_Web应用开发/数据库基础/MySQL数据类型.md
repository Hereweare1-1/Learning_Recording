# MySQL数据类型

数据类型决定一个字段可以保存什么数据、占用多少空间，以及可以执行哪些操作。选择类型时需要考虑数据范围、精度和实际用途。

## 1. 数值类型

### 1.1 整数类型

| MySQL类型         | 大小  | 有符号范围                  | 无符号范围        | Python类型 |
| --------------- | --- | ---------------------- | ------------ | -------- |
| `TINYINT`       | 1字节 | -128～127               | 0～255        | `int`    |
| `SMALLINT`      | 2字节 | -32768～32767           | 0～65535      | `int`    |
| `MEDIUMINT`     | 3字节 | -8388608～8388607       | 0～16777215   | `int`    |
| `INT`或`INTEGER` | 4字节 | -2147483648～2147483647 | 0～4294967295 | `int`    |
| `BIGINT`        | 8字节 | -2^63～2^63-1           | 0～2^64-1     | `int`    |

默认整数类型允许正数和负数。添加`UNSIGNED`后只允许非负数，并扩大正数范围：

```text
age TINYINT UNSIGNED
```

Python的`int`不会因为MySQL使用不同整数类型而改变，MySQL类型主要控制数据库中的范围和存储空间。

### 1.2 小数类型

| MySQL类型 | 大小 | 特点 | Python类型 |
| --- | --- | --- | --- |
| `FLOAT` | 4字节 | 单精度近似小数 | `float` |
| `DOUBLE` | 8字节 | 双精度近似小数 | `float` |
| `DECIMAL(M,D)` | 由精度决定 | 精确小数 | `decimal.Decimal` |

- **精度（M）**：一共可以保存多少位数字，不计算负号和小数点。
- **标度（D）**：总位数中有多少位是小数位。

例如：

```text
score DECIMAL(4,1)
```

表示总位数是4位，小数位数是1位，可以保存类似`999.9`的精确数值。

图片中的：

```text
score DOUBLE(4,1)
```

在旧式MySQL语法中同样表示总位数是4位、小数位数是1位。不过`DOUBLE(M,D)`属于非标准且已经弃用的MySQL扩展，新建表时不建议继续使用：

- 只需要近似小数时使用`DOUBLE`。
- 分数、金额等需要固定小数位和精确计算时使用`DECIMAL(4,1)`等`DECIMAL`类型。

`FLOAT`和`DOUBLE`属于近似值，可能出现浮点误差；`DECIMAL`属于精确值。

## 2. 字符串类型

### 2.1 CHAR与VARCHAR

| MySQL类型 | 特点 | 适用场景 | Python类型 |
| --- | --- | --- | --- |
| `CHAR(M)` | 定长字符串 | 性别代码、国家代码等长度固定的数据 | `str` |
| `VARCHAR(M)` | 变长字符串 | 用户名、标题、邮箱等长度变化的数据 | `str` |

```text
gender CHAR(1)
username VARCHAR(50)
```

`CHAR`会按固定长度保存，`VARCHAR`根据实际内容使用空间。不能简单理解为`CHAR`一定更快，应该先根据数据是否固定长度选择。

### 2.2 TEXT文本类型

| MySQL类型 | 最大长度 | Python类型 |
| --- | --- | --- |
| `TINYTEXT` | 约255字节 | `str` |
| `TEXT` | 约64KB | `str` |
| `MEDIUMTEXT` | 约16MB | `str` |
| `LONGTEXT` | 约4GB | `str` |

文本能够保存的实际字符数量还会受到字符集影响。普通短字符串优先使用`VARCHAR`，明显较长的正文再考虑`TEXT`。

### 2.3 BLOB二进制类型

| MySQL类型 | 最大长度 | Python类型 |
| --- | --- | --- |
| `TINYBLOB` | 约255字节 | `bytes` |
| `BLOB` | 约64KB | `bytes` |
| `MEDIUMBLOB` | 约16MB | `bytes` |
| `LONGBLOB` | 约4GB | `bytes` |

`BLOB`用于保存原始二进制数据。Web和AI应用通常把图片、音频和大文件保存在文件系统或对象存储中，数据库只保存文件路径或URL，因此直接使用BLOB保存大文件的情况较少。

## 3. 日期和时间类型

| MySQL类型     | 常见格式或范围                                             | 主要用途               | Python常用类型           |
| ----------- | --------------------------------------------------- | ------------------ | -------------------- |
| `DATE`      | `1000-01-01`～`9999-12-31`                           | 日期，格式为`YYYY-MM-DD` | `datetime.date`      |
| `TIME`      | `-838:59:59`～`838:59:59`                            | 时间或持续时长            | `datetime.timedelta` |
| `YEAR`      | `1901`～`2155`，还可以保存`0000`                           | 年份值，格式为`YYYY`      | `int`                |
| `DATETIME`  | `1000-01-01 00:00:00`～`9999-12-31 23:59:59`         | 日期和时间              | `datetime.datetime`  |
| `TIMESTAMP` | `1970-01-01 00:00:01` UTC～`2038-01-19 03:14:07` UTC | 时间戳，常用于创建和修改时间     | `datetime.datetime`  |

`DATETIME`保存的日期范围更广；`TIMESTAMP`范围较小，并会结合会话时区进行转换。两者还可以配合默认值和自动更新时间设置记录的创建、修改时间。

## 4. 建表示例

```sql
CREATE TABLE student (
    id BIGINT COMMENT '学生编号',
    age TINYINT UNSIGNED COMMENT '年龄',
    username VARCHAR(50) COMMENT '用户名',
    gender CHAR(1) COMMENT '性别',
    score DECIMAL(4,1) COMMENT '分数',
    created_at DATETIME COMMENT '创建时间'
) COMMENT '学生表';
```

返回：[[数据库学习导航]]。

## 5. 官方资料

- [MySQL 8.4数据类型](https://dev.mysql.com/doc/refman/8.4/en/data-types.html)
- [MySQL浮点数类型](https://dev.mysql.com/doc/refman/8.4/en/floating-point-types.html)
- [MySQL BLOB和TEXT类型](https://dev.mysql.com/doc/refman/8.4/en/blob.html)
- [MySQL日期和时间类型](https://dev.mysql.com/doc/refman/8.4/en/date-and-time-types.html)
