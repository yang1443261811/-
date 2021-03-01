#### SQL技巧及知识点

------

1. 模糊匹配关键字LIKE除了有百分号（%）通配符外还有下划线（____）通配符,下划线通配符可以用来匹配字符个数, 

   ```
   WHERE name LIKE '杨_' //这条语句表示检索姓杨,名字有两个字的记录,下划线的个数代表需要匹配的字符个数
   ```

2. 在sql中AND比OR优先级要高, 在使用时要注意

   ```
   WHERE product_id = 1 OR product_id = 3 AND price > 100 //这条语句会被解释成,查询商品ID为1并且价格大于100的记录,以及商品ID为3的记录,这是因为AND操作符的优先级比OR高造成的
   ```

3. 使用conect(), group_conect()等函数可以实现字段拼接

4. 在sql中字段与字段之间可以进行运算

   ```
   SELECT income / user_count as avg_income FROM order
   ```

   

