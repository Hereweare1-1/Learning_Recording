# DML数据操作

DML（Data Manipulation Language，数据操作语言）用于新增、修改和删除数据表中的记录，常见关键字包括`INSERT`、`UPDATE`和`DELETE`。

## 1. INSERT：新增数据

```sql
INSERT INTO employee (name, job, dept_id)
VALUES ('小明', '开发', 1);
```

这条语句会向`employee`表中新增一条员工记录。字段列表与`VALUES`中的值需要按照相同顺序一一对应。

## 2. 当前需要掌握什么

现阶段先掌握`INSERT`新增数据。`UPDATE`修改数据和`DELETE`删除数据等学到对应内容后再补充，不提前展开。

## 3. 相关笔记

- [[SQL基础语法]]
- [[数据库学习导航]]
