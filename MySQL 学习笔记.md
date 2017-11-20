## SELECT 作用更像是需要显示的列的值
## WHERE 子句更是选出对应的筛选后匹配的行

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
# 进行空值检查，关键字：IS NULL 包含空的email列
SELECT cust_name
FROM CUSTOMERS
WHERE cust_email IS NULL;
# 检查不包含null的行。
SELECT cust_name
FROM CUSTOMERS
WHERE cust_email IS NOT NULL;
# AND 关键字用来作为 WHERE 筛选子句，过滤所有的条件使用。
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' AND prod_price <= 4;
# OR 关键字匹配任一条件，在第一个条件满足时，不管第二个条件是否满足，相应的行都将被检索出来
SELECT prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01';

# 进行复杂的操作符运算的时候，可以使用括号提升某个低操作优先运算。'AND' 的优先级高于 'OR'
SELECT prod_name, prod_price
FROM Products
WHERE (vend_id = 'DLL01' OR vend_id = ‘BRS01’)
AND prod_price >= 10;

# IN 关键字
SELECT prod_name, prod_price
FROM Products
WHERE vend_id IN ( 'DLL01', 'BRS01' )
ORDER BY prod_name;
# 以下含有OR 的筛选和 IN 具有同样的效果。
SELECT prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'
ORDER BY prod_name;
## IN
# WHERE子句中用来指定要匹配值的清单的关键字，功能与OR相当。

# NOT
# WHERE子句中用来否定其后条件的关键字。
# 选择不是DLL01 中的匹配项
SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01'
ORDER BY prod_name;

#### 通配符（wildcard）用来匹配值的一部分的特殊字符。
## 在搜索串中，'%' 表示任何字符出现任意次数。

# 进行通配符搜索关键字：LIKE ，% 代表着任意的字符
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE 'Fish%';
# 可以将 '%’ 放在两端，进行匹配
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '%bean bag%' 
ORDER BY prod_name;
# 匹配以 F 开头以 y结尾的
SELECT prod_name
FROM Products
WHERE prod_name LIKE 'F%y%';
# 一个下划线只能匹配一个字符，所以'_' 和 '__' 匹配的分别是一位和两位（数字也是）
SELECT prod_id , prod_name
FROM Products
WHERE prod_name LIKE '__ inch teddy bear'; # 两个下划线匹配的是两位数
# 相应的如果使用的是 '%' 那么匹配的将是多个字符，
SELECT prod_id , prod_name
FROM Products
WHERE prod_name LIKE '% inch teddy bear';# 此处的%同时匹配了一位数字8 和 12 、18. 与%能匹配0个字符不同，_总是刚好匹配一个字符，不能多也不能少。


# 拼接搜索结果，关键字: CONCAT

# 这是MySQL中的特殊函数操作,来进行搜索结果的拼接
SELECT  CONCAT(vend_name ,' (',vend_country ,')') AS Result
FROM Vendors
ORDER BY vend_name;
输出结果：
Bear Emporium (USA)
Bears R Us (USA)
Doll House Inc. (USA)
Fun and Games (England)
Furball Inc. (USA)
Jouets et ours (France)