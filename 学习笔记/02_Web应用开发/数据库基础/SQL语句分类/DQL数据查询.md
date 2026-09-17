# DQL数据查询

DQL（Data Query Language，数据查询语言）用于查询数据表中的记录，核心关键字是`SELECT`。

## 1. 语句汇总

| 查询类型 | 关键字或函数 | 作用 |
| --- | --- | --- |
| 基本查询 | `SELECT`、`FROM` | 指定要查询的字段和数据表 |
| 条件查询 | `WHERE` | 筛选符合条件的记录 |
| 聚合查询 | `COUNT`、`MAX`、`MIN`、`AVG`、`SUM` | 统计数量、最大值、最小值、平均值或总和 |
| 分组查询 | `GROUP BY`、`HAVING` | 对记录分组，并筛选分组后的结果 |
| 排序查询 | `ORDER BY` | 按指定字段排序 |
| 分页查询 | `LIMIT` | 限制返回记录的位置和数量 |

## 2. DQL查询语句结构

```text
SELECT 字段列表
FROM 表名列表
[WHERE 条件列表]
[GROUP BY 分组字段列表]
[HAVING 分组后条件列表]
[ORDER BY 排序字段列表]
[LIMIT 分页参数];
```

方括号`[]`表示其中的子句可以省略，实际书写SQL时不需要输入方括号。各子句在SQL中应按照上面的顺序书写：

1. `SELECT`指定返回哪些字段。
2. `FROM`指定从哪些表中查询数据。
3. `WHERE`在分组前筛选记录。
4. `GROUP BY`按照指定字段分组。
5. `HAVING`筛选分组后的结果。
6. `ORDER BY`对查询结果排序。
7. `LIMIT`限制最终返回的记录。

`WHERE`和`HAVING`都用于筛选，但作用对象不同：`WHERE`筛选原始记录，`HAVING`筛选分组后的结果。

## 3. SELECT：基本查询

### 3.1 查询指定字段或全部字段

查询一个或多个指定字段：

```text
SELECT 字段1, 字段2, 字段3, ...
FROM 表名;
```

例如：

```sql
SELECT id, name
FROM employee;
```

这条语句会查询`employee`表中的`id`和`name`字段。

使用`*`可以查询表中的全部字段：

```text
SELECT *
FROM 表名;
```

例如：

```sql
SELECT *
FROM employee;
```

`*`书写方便，但查询结果会依赖表中现有的全部字段。只需要部分数据时，明确写出字段名更容易看出查询目的。

### 3.2 设置字段别名

```text
SELECT 字段1 [AS 别名1], 字段2 [AS 别名2], ...
FROM 表名;
```

方括号表示设置别名是可选的，实际SQL中不写方括号。别名只改变查询结果中显示的列名，不会修改数据表原来的字段名。

例如：

```sql
SELECT name AS employee_name, job AS employee_job
FROM employee;
```

在MySQL中，`AS`关键字也可以省略，但保留`AS`通常更容易看出原字段名和别名的关系。

### 3.3 去除重复记录

```text
SELECT DISTINCT 字段列表
FROM 表名;
```

例如，查询员工表中不重复的岗位：

```sql
SELECT DISTINCT job
FROM employee;
```

当`DISTINCT`后面有多个字段时，只有这些字段的组合完全相同，才会被视为重复记录。

`SELECT`还可以与查询条件、排序、分组和表连接等语法组合，完成不同的数据查询需求。

## 4. WHERE：条件查询

### 4.1 基本语法

```text
SELECT 字段列表
FROM 表名
WHERE 条件列表;
```

例如，查询年龄大于或等于`18`岁的员工：

```sql
SELECT id, name, age
FROM employee
WHERE age >= 18;
```

### 4.2 比较运算符

| 运算符 | 作用 | 条件示例 |
| --- | --- | --- |
| `>` | 大于 | `age > 18` |
| `>=` | 大于或等于 | `age >= 18` |
| `<` | 小于 | `age < 60` |
| `<=` | 小于或等于 | `age <= 60` |
| `=` | 等于 | `job = '开发'` |
| `<>`或`!=` | 不等于 | `job <> '开发'` |
| `BETWEEN 最小值 AND 最大值` | 在指定范围内，包含最小值和最大值 | `age BETWEEN 18 AND 35` |
| `IN (值1, 值2, ...)` | 匹配列表中的任意一个值 | `job IN ('开发', '测试')` |
| `LIKE` | 按指定模式进行模糊匹配 | `name LIKE '张%'` |
| `IS NULL` | 判断字段值是否为`NULL` | `dept_id IS NULL` |

判断空值时应使用`IS NULL`，不能写成`= NULL`。判断不是空值时使用`IS NOT NULL`。

### 4.3 LIKE占位符

| 占位符 | 作用 | 示例 |
| --- | --- | --- |
| `_` | 匹配任意一个字符 | `name LIKE '张_'` |
| `%` | 匹配任意数量的字符，也可以不匹配字符 | `name LIKE '张%'` |

例如，查询姓名以“张”开头的员工：

```sql
SELECT id, name
FROM employee
WHERE name LIKE '张%';
```

### 4.4 逻辑运算符

- `AND`或`&&`：并且，连接的多个条件需要同时成立。
- `OR`或`||`：或者，连接的多个条件只要有一个成立即可。
- `NOT`或`!`：对条件取反。

SQL中通常优先使用含义更清楚的`AND`、`OR`和`NOT`。

例如，查询年龄在`18`到`35`岁之间并且岗位为“开发”的员工：

```sql
SELECT id, name, age, job
FROM employee
WHERE age BETWEEN 18 AND 35
  AND job = '开发';
```

多个逻辑运算符同时出现时，可以使用圆括号明确条件的组合顺序。

## 5. 相关笔记

- [[SQL基础语法]]
- [[数据库学习导航]]
