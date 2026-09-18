# MySQL字符串函数

字符串函数用于拼接、转换、填充、清理或截取字符串，可以在`SELECT`、`WHERE`等语句中使用。

## 1. 函数汇总

| 函数                           | 作用                         |
| ---------------------------- | -------------------------- |
| `CONCAT(S1, S2, ..., Sn)`    | 把多个字符串拼接成一个字符串             |
| `LOWER(str)`                 | 把字符串中的英文字母转换为小写            |
| `UPPER(str)`                 | 把字符串中的英文字母转换为大写            |
| `LPAD(str, n, pad)`          | 在字符串左侧填充内容pad，使总长度达到`n`    |
| `RPAD(str, n, pad)`          | 在字符串右侧填充内容pad，使总长度达到`n`    |
| `TRIM(str)`                  | 删除字符串开头和结尾的空格              |
| `SUBSTRING(str, start, len)` | 从`start`位置开始截取长度为`len`的字符串 |

通用调用形式为：

```text
SELECT 函数(参数);
```

## 2. CONCAT：拼接字符串

```sql
SELECT CONCAT('Hello', ' ', 'MySQL');
```

结果为：

```text
Hello MySQL
```

`CONCAT()`也可以拼接字段值：

```sql
SELECT CONCAT(name, '-', job) AS employee_info
FROM employee;
```

## 3. LOWER和UPPER：转换大小写

```sql
SELECT LOWER('MySQL'), UPPER('MySQL');
```

结果分别为`mysql`和`MYSQL`。

## 4. LPAD和RPAD：填充字符串

```sql
SELECT LPAD('123', 5, '0');
SELECT RPAD('123', 5, '0');
```

结果分别为`00123`和`12300`。

参数`n`表示填充后的总长度，不是要补充的字符数量。如果原字符串已经超过长度`n`，结果会被截取到指定长度。

## 5. TRIM：删除两端空格

```sql
SELECT TRIM('  MySQL  ');
```

结果为`MySQL`。默认情况下，`TRIM()`只删除字符串两端的空格，不会删除字符串中间的空格。

## 6. SUBSTRING：截取字符串

```text
SUBSTRING(str, start, len)
```

- `str`：要截取的字符串。
- `start`：开始截取的位置。
- `len`：需要截取的字符数量。

> [!注意]
> MySQL的`SUBSTRING()`字符串位置从`1`开始

例如，从第`1`个字符开始截取`3`个字符：

```sql
SELECT SUBSTRING('abcdef', 1, 3);
```

结果为`abc`。

从第`2`个字符开始截取`3`个字符：

```sql
SELECT SUBSTRING('abcdef', 2, 3);
```

结果为`bcd`。

## 7. 相关笔记

- [[MySQL数值函数]]
- [[MySQL日期函数]]
- [[MySQL流程函数]]
- [[DQL基本查询]]
- [[数据库学习导航]]
