# Mysql为什么不建议使用join
### 用分解关联查询的方式重构查询的优势
* 让缓存的效率更高。许多应用程序可以方便地缓存单表查询对应的结果对象。如果关联中的某个表发生了变化，那么就无法使用查询缓存了，而拆分后，如果某个表很少改变，那么基于该表的查询就可以重复利用查询缓存结果了。

* 将查询分解后，执行单个查询可以减少锁的竞争。

* 在应用层做关联，可以更容易对数据库进行拆分，更容易做到高性能和可扩展。

* 查询本身效率也可能会有所提升。查询id集的时候，使用IN（）代替关联查询，可以让MySQL按照ID顺序进行查询，这可能比随机的关联要更高效。

* 可以减少冗余记录的查询。在应用层做关联查询，意味着对于某条记录应用只需要查询一次，而在数据库中做关联查询，则可能需要重复地访问一部分数据。从这点看，这样的重构还可能会减少网络和内存的消耗。

* 更进一步，这样做相当于在应用中实现了哈希关联，而不是使用MySQL的嵌套循环关联。某些场景哈希关联的效率要高很多。

* 在很多场景下，通过重构查询将关联放到应用程序中将会更加高效，这样的场景有很多，比如：当应用能够方便地缓存单个查询的结果的时候、当可以将数据分布到不同的MySQL服务器上的时候、当能够使用IN（）的方式代替关联查询的时候、当查询中使用同一个数据表的时候。
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  

# 数据库密码加密
[durid数据库密码加密](https://github.com/alibaba/druid/wiki/%E4%BD%BF%E7%94%A8ConfigFilter)
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  

# Mysql的update结合select更新数据
```sql
update panshi_sms_dev.ps_sms_product as a
 inner join (select id,
             concat('2019-06-24 ', date_format(create_time, '%H:%i:%s')) as modify_time,
             concat('2019-06-24 ', date_format(modify_time, '%H:%i:%s')) as create_time
             from panshi_sms_dev.ps_sms_product where id = 135404) as b
	      on a.id = b.id
  set a.create_time = b.create_time,
      a.modify_time = b.modify_time
where a.id = 135404;
```