# MySQL日期函数

日期函数用于获取当前日期和时间、提取日期中的组成部分，以及进行日期计算。

## 1. 函数汇总

| 函数 | 作用 |
| --- | --- |
| `CURDATE()` | 返回当前日期 |
| `CURTIME()` | 返回当前时间 |
| `NOW()` | 返回当前日期和时间 |
| `YEAR(date)` | 获取指定日期的年份 |
| `MONTH(date)` | 获取指定日期的月份 |
| `DAY(date)` | 获取指定日期是当月的第几天 |
| `DATE_ADD(date, INTERVAL expr type)` | 在日期或时间上增加指定的时间间隔 |
| `DATEDIFF(date1, date2)` | 返回`date1`减去`date2`相差的天数 |

通用调用形式为：

```text
SELECT 函数(参数);
```

## 2. 获取当前日期和时间

```sql
SELECT CURDATE(), CURTIME(), NOW();
```

- `CURDATE()`只返回当前日期，例如`2026-09-18`。
- `CURTIME()`只返回当前时间，例如`14:30:00`。
- `NOW()`同时返回当前日期和时间，例如`2026-09-18 14:30:00`。

这些函数返回的是MySQL服务器当前使用的日期和时间。

## 3. 提取年、月和日

```sql
SELECT
    YEAR('2026-09-18') AS year_value,
    MONTH('2026-09-18') AS month_value,
    DAY('2026-09-18') AS day_value;
```

结果分别为`2026`、`9`和`18`。

这些函数也可以处理日期字段。假设`employee`表中存在`created_at`字段：

```sql
SELECT YEAR(created_at), MONTH(created_at), DAY(created_at)
FROM employee;
```

## 4. DATE_ADD：增加时间间隔

```text
DATE_ADD(date, INTERVAL expr type)
```

- `date`：原始日期或时间。
- `expr`：增加的数量。
- `type`：时间单位，例如`YEAR`、`MONTH`、`DAY`、`HOUR`、`MINUTE`或`SECOND`。

在指定日期上增加`7`天：

```sql
SELECT DATE_ADD('2026-09-18', INTERVAL 7 DAY);
```

结果为`2026-09-25`。

在指定日期上增加`1`个月：

```sql
SELECT DATE_ADD('2026-09-18', INTERVAL 1 MONTH);
```

## 5. DATEDIFF：计算相差天数

```text
DATEDIFF(date1, date2)
```

`DATEDIFF()`按照`date1 - date2`的顺序计算两个日期相差的天数：

```sql
SELECT DATEDIFF('2026-09-18', '2026-09-10');
```

结果为`8`。如果`date1`早于`date2`，结果会是负数。

`DATEDIFF()`只比较日期部分，即使参数中包含时间，也不会根据小时、分钟或秒计算小数天数。

## 6. 相关笔记

- [[MySQL字符串函数]]
- [[MySQL数值函数]]
- [[MySQL流程函数]]
- [[DQL基本查询]]
- [[数据库学习导航]]
