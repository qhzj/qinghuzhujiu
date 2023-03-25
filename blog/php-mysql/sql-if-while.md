# 流程结构 if while

## if分支

两种基本用法

1、用在select查询中

基本语法

```sql
if(条件, 结果为真, 结果为假);
```

```sql
mysql> select * from my_student;
+----+-----------+----------+------+--------+
| id | name      | class_id | age  | gender |
+----+-----------+----------+------+--------+
|  1 | 刘备      |        1 |   18 |      2 |
|  2 | 李四      |        1 |   19 |      1 |
|  3 | 王五      |     NULL |   20 |      2 |
|  4 | 张飞      |     NULL |   21 |      1 |
|  5 | 关羽      |     NULL |   22 |      2 |
|  6 | 曹操      |        1 |   20 |   NULL |
| 10 | 司马懿    |        6 |   26 |      1 |
+----+-----------+----------+------+--------+

mysql> select *, if(age > 20, '符合', '不符合') as result from my_student;
+----+-----------+----------+------+--------+-----------+
| id | name      | class_id | age  | gender | result    |
+----+-----------+----------+------+--------+-----------+
|  1 | 刘备      |        1 |   18 |      2 | 不符合    |
|  2 | 李四      |        1 |   19 |      1 | 不符合    |
|  3 | 王五      |     NULL |   20 |      2 | 不符合    |
|  4 | 张飞      |     NULL |   21 |      1 | 符合      |
|  5 | 关羽      |     NULL |   22 |      2 | 符合      |
|  6 | 曹操      |        1 |   20 |   NULL | 不符合    |
| 10 | 司马懿    |        6 |   26 |      1 | 符合      |
+----+-----------+----------+------+--------+-----------+
7 rows in set (0.00 sec)
```

2、用在复杂语句块中（函数、存储过程、触发器）

基本语法

```sql
if 条件表达式 then
    满足条件要执行的语句;
end if;
```

3、复合语法

基本语法

```sql
if 条件表达式 then
    满足条件要执行的语句;
else
    不满足条件要执行的语句;
end if;

-- 如果有其他分支，嵌套使用
if 条件表达式 then
    满足条件要执行的语句;
else
    不满足条件要执行的语句;
    if 条件表达式 then
        满足条件要执行的语句;
    else
        不满足条件要执行的语句;
    end if;
end if;
```

## while循环

基本语法

```sql
while 条件 do
    要循环执行的代码
end while;
```

结构标识符: 为结构命名

基本语法
```sql
标识名字: while 条件 do
    要循环执行的代码
end while [标识名字];
```

mysql中没有continue 和break

- iterate 迭代 以下代码不执行，重新开始循环,相当于continue
- leave 离开 终止循环，相当于break

```sql
标识名字: while 条件 do
    if 条件判断 then
        循环控制
        iterate / leave 标识名字;
    end if;

    循环体

end while [标识名字];
```