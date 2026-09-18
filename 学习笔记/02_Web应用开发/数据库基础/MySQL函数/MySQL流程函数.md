# MySQL流程函数

流程函数用于根据条件返回不同的值，可以在SQL查询结果中实现简单的条件判断。

## 1. 函数汇总

| 函数或表达式 | 作用 |
| --- | --- |
| `IF(condition, true_value, false_value)` | 条件成立时返回第二个参数，否则返回第三个参数 |
| `IFNULL(value1, value2)` | `value1`不为`NULL`时返回`value1`，否则返回`value2` |
| `CASE WHEN condition THEN result ... ELSE default END` | 依次判断多个不同条件并返回对应结果 |
| `CASE expr WHEN value THEN result ... ELSE default END` | 把同一个表达式依次与多个值比较并返回对应结果 |

## 2. IF：判断一个条件

```text
IF(condition, true_value, false_value)
```

例如，判断成绩是否及格：

```sql
SELECT IF(85 >= 60, '及格', '不及格');
```

条件`85 >= 60`成立，因此结果为`及格`。

## 3. IFNULL：处理NULL

```text
IFNULL(value1, value2)
```

- `value1`不为`NULL`时，返回`value1`。
- `value1`为`NULL`时，返回`value2`。

例如：

```sql
SELECT IFNULL(NULL, '未填写');
```

结果为`未填写`。

处理字段中的空值：

```sql
SELECT name, IFNULL(phone, '未填写') AS phone
FROM employee;
```

这个示例假设`employee`表中存在`phone`字段。

## 4. CASE WHEN：判断多个条件

```text
CASE
    WHEN 条件1 THEN 结果1
    WHEN 条件2 THEN 结果2
    ...
    ELSE 默认结果
END
```

`CASE WHEN`会按照书写顺序依次判断条件，遇到第一个成立的条件后返回对应结果，不再继续判断后面的条件。

例如，根据成绩划分等级：

```sql
SELECT CASE
    WHEN 85 >= 90 THEN '优秀'
    WHEN 85 >= 60 THEN '及格'
    ELSE '不及格'
END AS score_level;
```

结果为`及格`。

条件的顺序会影响结果。范围较严格的条件通常写在前面，例如先判断`>= 90`，再判断`>= 60`。

## 5. CASE表达式：匹配多个值

```text
CASE 表达式
    WHEN 值1 THEN 结果1
    WHEN 值2 THEN 结果2
    ...
    ELSE 默认结果
END
```

例如，根据岗位代码显示岗位名称：

```sql
SELECT CASE 'dev'
    WHEN 'dev' THEN '开发'
    WHEN 'test' THEN '测试'
    ELSE '其他'
END AS job_name;
```

结果为`开发`。

## 6. ELSE可以省略

`CASE`中的`ELSE`可以省略，但如果所有条件或值都不匹配，结果会是`NULL`。需要明确的默认结果时，应写出`ELSE`。

## 7. 相关笔记

- [[MySQL字符串函数]]
- [[MySQL数值函数]]
- [[MySQL日期函数]]
- [[DQL基本查询]]
- [[数据库学习导航]]
