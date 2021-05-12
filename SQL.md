#### SQL技巧及知识点

------

- 模糊匹配关键字`LIKE`除了有百分号`（%）`通配符外还有下划线`（____）`通配符,下划线通配符可以用来匹配字符个数, 


```
WHERE name LIKE '杨_' //这条语句表示检索姓杨,名字有两个字的记录,下划线的个数代表需要匹配的字符个数
```

- 在`sql`中`AND`比`OR`优先级要高, 在使用时要注意


```
WHERE product_id = 1 OR product_id = 3 AND price > 100 //这条语句会被解释成,查询商品ID为1并且价格大于100的记录,以及商品ID为3的记录,这是因为AND操作符的优先级比OR高造成的
```

- 使用`conect()`, `group_conect()`等函数可以实现字段拼接

- 在`sql`中字段与字段之间可以进行运算

```
SELECT income / user_count as avg_income FROM order
```

- `count(*)`与`count(col)`的区别,`count(*)`可以用于`NULL`,而`count(col)`要先排除`NULL`的行再进行统计

- 关于列设置可为`NULL`的情况说明, 在`SQL`中聚合函数遇见为`NULL`的列时,会排除掉再进行统计,为`NULL`的列会从二值逻辑变化为三值逻辑,也就是当列为`NULL`时`SQL`的逻辑判断不再是`false`或者`true`,而是被当成`unknown`来处理,所以当列存在`NULL`值时, 需要注意其处理方式

- `ALL`运算符是一个逻辑运算符，它将单个值与子查询返回的单列值集进行比较

  ```
  SELECT * FROM table_name WHERE column_name > ALL (subquery) 查询查找column_name列中的值大于子查询返回的最大值的行
  ```

- 求众数,如下案例是我需要从评价表中找出被评价最多的客服人员

  ```
  SELECT star, user_id, count(*) AS cnt FROM rates GROUP BY star HAVING count(*) >= ALL (SELECT count(*) FROM rates GROUP BY star)
  ```

- `case`表达式的写法(  )

  ```
  简单case表达式 CASE sex WHEN '1' THEN '男' WHEN '2' THEN '女' ELSE '其它' END
  搜索case表达式 CASE WHEN sex='1' THEN '男' WHEN sex='2' THEN '女' ELSE '其它' END
  ```

- case表达式需要注意的点

  ```
  1.在发现为真的WHEN子句时, CASE表达式的真假判断就会中止
  2.case表达式里各个分支返回的数据类型需要一致,某个分支返回字符串,而其它分支返回数值型的写法是不正确的(在实际测试中不存在此问题,可能是mysql版本的问题)
  3.不要忘记写case表达式的结束子句end
  4.每个case表达式最好明确的写上else子句
  ```

- 在连接查询时两张表的结果集是一个笛卡尔集,并不是简单的左表结果集连上右表结果集,我们可以把两张待连接的表看成两个集合,在一对一和一对多关系的两个集合,在进行连接操作后行数不会( 异常 )增加, 这个概念在需要使用连接和聚合来解决问题时非常有用

  ```
  假设集合A={a, b}，集合B={0, 1, 2}，则两个集合的笛卡尔积为{(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}。
  ```

- 使用自连接删除重复行( `“自连接”(self join)，这个技巧常常被人们忽视，其实是有挺多妙用的` )

<img src="C:\Users\BL\Desktop\工作文件夹\bestPhper\img\718C8900-B493-429e-A9FD-9321428A0C8E.png" style="zoom:80%;" />

```
DELETE FROM Products P1 WHERE id < (SELECT MAX(P2.id) FROM Products P2  WHERE P1.name = P2.name AND P1.price = P2.price)
```

- 求余额,通过下列`sql`实现图中的查询结果 

  ```
  ![B181CD88-E0A4-41d8-9A7C-483FC5DC75B6](C:\Users\BL\Desktop\工作文件夹\bestPhper\img\B181CD88-E0A4-41d8-9A7C-483FC5DC75B6.png)SELECT date, amount,(SELECT sum(amount) FROM accounts A2 WHERE A2.date <= A1.date ) AS balance FROM accounts A1 ORDER BY A1.date
  ```

  <img src="C:\Users\BL\Desktop\工作文件夹\bestPhper\img\66CDA565-AF75-4c0f-BDCC-C45D4ADC57E8.png" style="zoom:67%;" />

- `union` 和 `union all`的区别,前者会过滤掉重复行而后者不会, 集合运算为了排除重复行, 默认的会发生排序, 而加上`all`选项后,就不会再排序了, 所以性能会有提升

- 有如下会议表meetings求没有参加某次会议的人

  ```
  SELECT DISTINCT M1.meeting, M2.person FROM meetings M1 JOIN meetings M2 WHERE NOT EXISTS (SELECT * FROM meetings M3 WHERE M1.meeting = M3.meeting AND M2.person = M3.person)
  ```

<img src="C:\Users\BL\Desktop\工作文件夹\bestPhper\img\82BF7DA9-78E3-4dc1-8FCD-C69D9F02E39C.png"  />

<img src="C:\Users\BL\Desktop\工作文件夹\bestPhper\img\B181CD88-E0A4-41d8-9A7C-483FC5DC75B6.png"  />

- `in`子句的变种用法( 下列语句是查询`p1`, `p2`, `p3`, `p4`, `p5`, `p6`六个字段中值等于100的记录, 只要有一个字段的值为100记录就能匹配上 )

  ```
  SELECT * FROM table_name WHERE 100 IN (p1, p2, p3, p4, p5, p6)
  ```

  