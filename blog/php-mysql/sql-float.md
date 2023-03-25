# 数据类型-小数

浮点型：

又称为精度类型，是一种可能丢失精度的数据类型，数据可能不那么准确

简单的举例

```
所有位都为1

整数    1 1 1 1 1 1 1 1   都存整数表示具体数据值
浮点数  1 1 1 | 1 1 1 1 1  10^7 * 数据值，其中3位用来存指数
```

## float 单精度类型

4字节存储，7位精度，表示数据范围比整数大得多

基本语法

```
float 表示不指定小数位的浮点数
float(M, D)表示一共存储M个有效数字，其中小数部分占D位

例如：

float(10, 2) 整数部分为8位，小数部分为2位
```

示例
```sql
create table my_float(
    f1 float,
    f2 float(10, 2)
);

desc my_float;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| f1    | float       | YES  |     | NULL    |       |
| f2    | float(10,2) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+

insert into 
my_float (f1, f2)
values(123.123, 12345678.90);

-- 如果精度丢失，按照四舍五入方式计算
select * from my_float;
+---------+-------------+
| f1      | f2          |
+---------+-------------+
| 123.123 | 12345679.00 |
+---------+-------------+

-- 超出指定位数
insert into 
my_float (f1, f2)
values(123.123456789, 123456789.90);
ERROR 1264 (22003): Out of range value for column 'f2' at row 1

-- 用户不能直接插入超过指定整数部分的长度，但是如果是系统自动进位导致，可以允许
insert into 
my_float (f1, f2)
values(123.123456789, 99999999.99);

select * from my_float;
+---------+--------------+
| f1      | f2           |
+---------+--------------+
| 123.123 |  12345679.00 |
| 123.123 | 100000000.00 |
+---------+--------------+

-- 浮点数可以采用科学计数法来存储数据
insert into 
my_float (f1, f2)
values(123.123, 10e5);

select * from my_float;
+---------+--------------+
| f1      | f2           |
+---------+--------------+
| 123.123 |  12345679.00 |
| 123.123 | 100000000.00 |
| 123.123 |   1000000.00 |
+---------+--------------+
```

## double 双精度 

8个字节存储，表示范围更大，精度有15位左右

## 定点数decimal

能够保证数据精确的小数（小数部分可能不精确，超出长度会四舍五入），整数部分一定精确

decimal(M, D), M表示总长度，最大值不能超过65，D代表小数部分长度，最长不能超过30

```sql
create table my_decimal(
    float_data float(10, 2),
    decimal_data decimal(10, 2)
);

mysql> desc my_decimal;
+--------------+---------------+------+-----+---------+-------+
| Field        | Type          | Null | Key | Default | Extra |
+--------------+---------------+------+-----+---------+-------+
| float_data   | float(10,2)   | YES  |     | NULL    |       |
| decimal_data | decimal(10,2) | YES  |     | NULL    |       |
+--------------+---------------+------+-----+---------+-------+

insert into 
my_decimal(float_data, decimal_data)
values(12345678.90, 12345678.90);

mysql> select * from my_decimal;
+-------------+--------------+
| float_data  | decimal_data |
+-------------+--------------+
| 12345679.00 |  12345678.90 |
+-------------+--------------+

insert into 
my_decimal(float_data, decimal_data)
values(99999999.99, 99999999.99);


mysql> select * from my_decimal;
+--------------+--------------+
| float_data   | decimal_data |
+--------------+--------------+
|  12345679.00 |  12345678.90 |
| 100000000.00 |  99999999.99 |
+--------------+--------------+

-- 定点数，整数部分被进位，会抛出异常
insert into 
my_decimal(float_data, decimal_data)
values(99999999.99, 99999999.999);
ERROR 1264 (22003): Out of range value for column 'decimal_data' at row 1
```