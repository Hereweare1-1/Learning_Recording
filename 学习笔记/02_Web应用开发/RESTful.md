# RESTful

## 1. 什么是RESTful

RESTful是指遵循REST架构风格的API接口服务。

REST全称为Representational State Transfer，中文为“表述性状态转移”，是一种软件架构风格。

## 2. RESTful接口设计

RESTful使用URL定位资源，使用HTTP请求方式对资源进行操作。

|URL|请求方式|含义|
|---|---|---|
|`http://localhost:8000/users/1`|GET|查询ID为1的用户|
|`http://localhost:8000/users/1`|DELETE|删除ID为1的用户|
|`http://localhost:8000/users`|POST|新增用户|
|`http://localhost:8000/users`|PUT|修改用户|

## 3. 注意事项

- REST是一种架构风格和约定方式，不是必须遵守的规定。
- 表示一类资源的URL通常使用复数形式，例如`users`、`books`、`items`。
