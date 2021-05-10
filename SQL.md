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
  SELECT  * FROM table_name WHERE column_name > ALL (subquery) 查询查找column_name列中的值大于子查询返回的最大值的行
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

  

