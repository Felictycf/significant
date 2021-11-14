mysql里面的备份命令

先启动mysql

### 数据库的复制

`msyql -u root -p `

`在输入密码后 mysqldump -u root -p -B hj_bd01 > d:|\\mysql.sql这里是要把sql语句导入的地方 `



### 数据库恢复

`mysql -u root -p`

` 输入密码后` 

`source d:\\mysql.sql`



这里的恢复，其实就是把msyql.sql语句重新的执行了一边。mysql.sql语句里面都是sql命令，和我们平时写的order一样。



#### 数据库表的备份

`mysqldump -u root -p hj_db01 t1 >d:\\ ttt.sql`

