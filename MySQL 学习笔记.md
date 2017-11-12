1、检索某个数据库中的某个列
SELECT database_row
FROM databases;
----------
2、检索限制的个数，关键字：LIMIT 和 OFFSET
SELECT database_row
FROM database
LIMIT 5 OFFSET 3；
或者
SELECT database_row
FROM databases
LIMIT  3 ,5；
第一个数字是从第几行开始，第二个数字是选取多少行。
例如从第3行开始选取5行。
----------
3、选择不重复的行，关键字：DISTINCT
SELECT DISTINCT vend_id
FROM Products;
----------
4、注释总共有三种方式：
第一种以 “-- ”开头接着的整行都是注释。
第二种以 “ # ”开头接着整行都是注释。
第三种以  "/* …. */ " 类似于代码注释的形式。
推荐使用第一种，使用的更加广泛。
SELECT prod_name -- 这是一条注释 -- 
# 这是一条注释
FROM Products
/* 这是一条注释*/
LIMIT 5 OFFSET 5;
----------
5、选择结果进行排序关键字：ORDER BY 必须是最后一行子句。
排序的默认方向是升序关键字：ASC 也可以选择不填写
排序的降序关键字是：DESC 
# 排序
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC, prod_name;
----------
6、选择特定行的内容关键字：WHERE
# 选择特定行内容
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
WHERE 和 ORDER BY 同时使用时，where 必须位于前面
# 检索特定数据，并进行排序
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49
ORDER BY prod_name;
# 选择价格小于10美元的商品,先进行价格排序，然后在进行名字排序。
SELECT prod_name, prod_price
FROM Products
WHERE prod_price < 10
ORDER BY prod_price DESC ,prod_name;
# 不匹配检查，下例是匹配除了'DLL01'之外的所有匹配项。
SELECT vend_id, prod_name
FROM Products
WHERE vend_id <> 'DLL01';
# <> 和 != 有时候可以进行互换。
SELECT vend_id, prod_name
FROM Products
WHERE vend_id != 'DLL01';
# 区间值选取和匹配，关键字：WHERE cow BETWEEN a AND b;
BETWEEN 和 WHERE 关键字配合使用，达到筛选指定区间的匹配项。