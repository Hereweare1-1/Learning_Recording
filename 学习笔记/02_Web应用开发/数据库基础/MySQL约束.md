# MySQL约束

约束（Constraint）是定义在数据表字段或整张表上的规则，用于限制能够保存的数据，保证数据的正确性、有效性和完整性。

约束可以在创建表时定义，也可以通过修改表结构添加或删除。

## 1. 常见约束

| 约束 | 作用 | 关键字 |
| --- | --- | --- |
| 非空约束 | 字段值不能为`NULL` | `NOT NULL` |
| 唯一约束 | 字段值不能重复 | `UNIQUE` |
| 主键约束 | 唯一标识表中的一条记录，同时保证非空和唯一 | `PRIMARY KEY` |
| 默认约束 | 插入记录时没有指定字段值，就使用默认值 | `DEFAULT` |
| 检查约束 | 要求字段值或一条记录满足指定条件 | `CHECK` |
| 外键约束 | 在两张表之间建立引用关系，保持关联数据一致 | `FOREIGN KEY` |

## 2. 约束的作用位置

约束可以分为两种写法：

- **字段级约束**：直接写在某个字段定义后面，主要限制该字段。
- **表级约束**：写在所有字段定义之后，可以同时涉及一个或多个字段。

`NOT NULL`和`DEFAULT`通常写成字段级约束；主键、唯一、检查和外键约束可以根据需要使用字段级或表级形式。复合主键、复合唯一约束和外键通常使用表级写法。

## 3. 各约束的基本规则

### 3.1 NOT NULL：非空约束

字段必须有值，不能保存`NULL`。

`NULL`表示缺少或未知的值，它与数字`0`、空字符串`''`不同。

### 3.2 UNIQUE：唯一约束

字段中非`NULL`的值不能重复。MySQL允许可空的`UNIQUE`字段出现多个`NULL`，因为`NULL`不被当作彼此相等的普通值。

### 3.3 PRIMARY KEY：主键约束

主键用于唯一标识一条记录，必须同时满足非空和唯一。

- 一张表只能有一个主键。
- 一个主键可以只包含一个字段，也可以由多个字段组成复合主键。

### 3.4 DEFAULT：默认约束

插入记录时如果没有为字段提供值，MySQL会使用该字段的默认值。

默认值不会代替显式传入的普通值。字段是否允许显式保存`NULL`，仍然由`NULL`或`NOT NULL`规则决定。

### 3.5 CHECK：检查约束

`CHECK`要求插入或修改后的数据满足指定条件。条件结果为`FALSE`时，MySQL会拒绝该操作。

MySQL从`8.0.16`开始真正创建并检查`CHECK`约束；当前使用的MySQL 8.4支持该约束。

### 3.6 FOREIGN KEY：外键约束

外键在子表字段和父表字段之间建立引用关系，用于防止子表保存父表中不存在的关联值，从而保持两张表的数据一致性。

例如，部门表保存部门信息，员工表中的`department_id`保存员工所属部门：

- **父表**：保存被引用数据的表，例如部门表`department`。
- **子表**：通过外键引用父表的表，例如员工表`employee`。
- **外键字段**：子表中引用父表字段的字段，例如`employee.department_id`。

建立外键后，子表的外键值必须满足以下条件之一：

- 该值能够在父表被引用字段中找到。
- 外键字段允许为`NULL`，并且该值是`NULL`。

仅仅让两个字段保存相同的编号，并没有建立数据库层面的外键关系。必须使用`FOREIGN KEY`和`REFERENCES`定义外键，MySQL才能检查关联数据的一致性。

#### 创建表时添加外键

```sql
CREATE TABLE department (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL
);

CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    department_id INT,
    CONSTRAINT fk_employee_department
        FOREIGN KEY (department_id)
        REFERENCES department(id)
);
```

`CONSTRAINT fk_employee_department`用于指定外键名称。外键名称可以省略，由MySQL自动生成，但主动命名更方便以后删除和排查问题。

#### 为已有表添加外键

如果数据表创建时还没有定义该外键，可以使用以下语句添加；不要与上面的建表示例重复执行。

```sql
ALTER TABLE employee
ADD CONSTRAINT fk_employee_department
FOREIGN KEY (department_id)
REFERENCES department(id);
```

添加外键前，子表中已经存在的外键值也必须能在父表中找到，否则添加操作会失败。

#### 删除外键

```sql
ALTER TABLE employee
DROP FOREIGN KEY fk_employee_department;
```

删除外键约束不会删除字段，也不会删除字段中已经保存的数据。

#### 父表数据变化时的处理方式

可以使用`ON UPDATE`和`ON DELETE`分别指定父表记录被更新或删除时，如何处理子表中的关联记录。

| 处理方式 | 作用 |
| --- | --- |
| `RESTRICT` | 子表存在关联记录时，拒绝更新或删除父表记录 |
| `NO ACTION` | 在InnoDB中与`RESTRICT`相同 |
| `CASCADE` | 父表字段更新或记录删除时，自动更新或删除子表中的关联数据 |
| `SET NULL` | 父表字段更新或记录删除时，将子表的外键值设为`NULL` |
| `SET DEFAULT` | MySQL能够识别该语法，但InnoDB不支持，会拒绝创建外键 |

如果没有写`ON UPDATE`或`ON DELETE`，MySQL默认使用`NO ACTION`。在InnoDB中，它会在存在关联记录时拒绝更新或删除父表数据。

下面的外键会在父表编号更新时同步更新子表外键，在删除仍被员工引用的部门时拒绝删除：

```sql
ALTER TABLE employee
ADD CONSTRAINT fk_employee_department
FOREIGN KEY (department_id)
REFERENCES department(id)
ON UPDATE CASCADE
ON DELETE RESTRICT;
```

使用`SET NULL`时，子表的外键字段必须允许保存`NULL`，不能同时设置`NOT NULL`。

外键字段与被引用字段需要使用相互兼容的数据类型。对于整数类型，长度和是否使用`UNSIGNED`等属性需要保持一致。父表和子表还需要使用相同且支持外键的存储引擎，例如InnoDB。MySQL会使用索引检查外键关系；如果子表外键字段没有合适的索引，MySQL会自动创建。

## 4. AUTO_INCREMENT：自动递增

`AUTO_INCREMENT`表示自动递增，它是字段属性，不属于约束。插入数据时如果不指定该字段的值，MySQL会自动生成一个递增的整数。

它通常与整数类型的主键一起使用：

```sql
CREATE TABLE user (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL
);
```

插入数据时可以省略`id`：

```sql
INSERT INTO user (username) VALUES ('小明');
```

MySQL会自动为`id`赋值，默认从`1`开始递增。一张表只能有一个`AUTO_INCREMENT`字段，并且该字段必须是索引的一部分。

删除记录、插入失败或事务回滚都可能使自动生成的编号出现空缺，因此不能把`AUTO_INCREMENT`理解为永远连续的编号。

## 5. 添加约束的时机

约束通常在以下两种操作中添加：

- 使用`CREATE TABLE`创建表时直接定义约束。
- 使用`ALTER TABLE`修改已有表时添加、修改或删除约束。

约束属于表结构的一部分。添加约束前，表中已经存在的数据也必须满足该约束，否则操作可能失败。

## 6. 相关笔记

- [[DDL数据库与表结构操作]]
- [[MySQL数据类型]]
- [[数据库学习导航]]

## 7. 官方资料

- [MySQL 8.4 CREATE TABLE语句](https://dev.mysql.com/doc/refman/8.4/en/create-table.html)
- [MySQL 8.4 CHECK约束](https://dev.mysql.com/doc/refman/8.4/en/create-table-check-constraints.html)
- [MySQL 8.4外键约束](https://dev.mysql.com/doc/refman/8.4/en/create-table-foreign-keys.html)
