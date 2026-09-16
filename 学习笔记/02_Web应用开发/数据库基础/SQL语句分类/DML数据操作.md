# DML数据操作

DML（Data Manipulation Language，数据操作语言）用于新增、修改和删除数据表中的记录，常见关键字包括`INSERT`、`UPDATE`和`DELETE`。

## 1. 语句汇总

| 作用 | 基本语句 | 当前学习状态 |
| --- | --- | --- |
| 新增数据 | `INSERT INTO 表名 (...) VALUES (...);` | 正在学习 |
| 修改数据 | `UPDATE 表名 SET 字段 = 值 WHERE 条件;` | 后续学习 |
| 删除数据 | `DELETE FROM 表名 WHERE 条件;` | 后续学习 |

## 2. INSERT：新增数据

```sql
INSERT INTO employee (name, job, dept_id)
VALUES ('小明', '开发', 1);
```

这条语句会向`employee`表中新增一条员工记录。字段列表与`VALUES`中的值需要按照相同顺序一一对应。

## 3. 当前需要掌握什么

现阶段先掌握`INSERT`新增数据。`UPDATE`修改数据和`DELETE`删除数据等学到对应内容后再补充，不提前展开。

## 4. 相关笔记

- [[SQL基础语法]]
- [[数据库学习导航]]
