## Mysql 临时表

<a href="README.md">目录</a>

> `MySQL` 临时表在我们需要保存一些临时数据时是非常有用的。临时表只在当前连接可见，当关闭连接时，`Mysql`会自动删除表并释放所有空间。临时表在`MySQL 3.23`版本中添加

> `MySQL`临时表只在当前连接可见，如果你使用`nodejs`脚本来创建`MySQL`临时表，那每当`nodejs`脚本执行完成后，该临时表也会自动销毁。

> 如果你使用了其他`MySQL`客户端程序连接`MySQL`数据库服务器来创建临时表，那么只有在关闭客户端程序时才会销毁临时表，当然你也可以手动销毁。

实例

```mysql
mysql> CREATE TEMPORARY TABLE SalesSummary (
     product_name VARCHAR(50) NOT NULL
     , total_sales DECIMAL(12,2) NOT NULL DEFAULT 0.00
     , avg_unit_price DECIMAL(7,2) NOT NULL DEFAULT 0.00
     , total_units_sold INT UNSIGNED NOT NULL DEFAULT 0);
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT INTO SalesSummary
     (product_name, total_sales, avg_unit_price, total_units_sold)
     VALUES
     ('cucumber', 100.25, 90, 2);

mysql> SELECT * FROM SalesSummary;
+--------------+-------------+----------------+------------------+
| product_name | total_sales | avg_unit_price | total_units_sold |
+--------------+-------------+----------------+------------------+
| cucumber     |      100.25 |          90.00 |                2 |
+--------------+-------------+----------------+------------------+
1 row in set (0.00 sec)
```

当你使用 `SHOW TABLES`命令显示数据表列表时，你将无法看到 `SalesSummary`表。
如果你退出当前`MySQL`会话，再使用 `SELECT` 命令来读取原先创建的临时表数据，那你会发现数据库中没有该表的存在，因为在你退出时该临时表已经被销毁了。

### 删除MySQL 临时表

默认情况下，当你断开与数据库的连接后，临时表就会自动被销毁。当然你也可以在当前MySQL会话使用 `DROP TABLE` 命令来手动删除临时表。

以下是手动删除临时表的实例：
```mysql
mysql> CREATE TEMPORARY TABLE SalesSummary (
     product_name VARCHAR(50) NOT NULL
     , total_sales DECIMAL(12,2) NOT NULL DEFAULT 0.00
     , avg_unit_price DECIMAL(7,2) NOT NULL DEFAULT 0.00
     , total_units_sold INT UNSIGNED NOT NULL DEFAULT 0);
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT INTO SalesSummary
     (product_name, total_sales, avg_unit_price, total_units_sold)
     VALUES
     ('cucumber', 100.25, 90, 2);

mysql> SELECT * FROM SalesSummary;
+--------------+-------------+----------------+------------------+
| product_name | total_sales | avg_unit_price | total_units_sold |
+--------------+-------------+----------------+------------------+
| cucumber     |      100.25 |          90.00 |                2 |
+--------------+-------------+----------------+------------------+
1 row in set (0.00 sec)

mysql> DROP TABLE SalesSummary;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT * FROM SalesSummary;
ERROR 1146: Table 'RUNOOB.SalesSummary' doesn't exist
```


<a href="clone-tables.md" style="float: right;"><—— mysql 复制表</a>
