https://www.freebuf.com/articles/defense/381473.html

# WAF绕过

# 1、双写注入绕过
## 将`and`双写为`anandd`

# 2、大小写绕过

# 3、脏数据绕过
## 传入一长串数据使waf失效,从而实现绕过waf;某些waf处理POST数据时,只会检测开头的8k,后面选择全部放过

# 4、复参数绕过(&id=)
## 例如一个请求`GET /pen/news.php?id=1 union select user,password from mysql.user`可以修改为`GET pen/news.php?id=1&id=union&id=select&id=user,password&id=from%20mysql.user`;很多waf都可以这样绕,测试最新版waf能绕过不符语句

# 5、高并发绕过
## 对请求进行并发,攻击请求会被负载均衡调度到不同节点,导致某些请求绕过了waf的拦截

# 6、特殊字符替换空格
## mysql中`%0a`是换行符,可以代替空格,可以部分绕过最新版本的waf
## sqlsercer中可以用`/**/`代替空格
## `http://192.168.60.68/sql.php?id=1/*|%23--%23|*/union/*|%23--%23|*/select/*|%23--%23|*/1,user(),3,4,5`
## `http://192.168.60.68/sql.php?id=1/*|%23--%23|*/and/*|%23--%23|*/1=2`
## mysql：`%0a/**/`
##sqlserver :`/**/`、`/*|%23--%23|*/`

# 7、使用其他变量或者命令对注入语句进行替换
## `version()| @@version`
## `concat() | concat_ws()`
## `group_concat() | concat_ws()`
## `= | like`
## `1| MOD(8,7)`

# 8、HTTP参数污染
## php语言中`id=1&id=2`后面的值会自动覆盖前面的值

# 9、特殊字符拼接(+)
## mysql中可以利用注释`/**/`来绕过
## mssql中函数里面可以用+来拼接
## 如`GET /pen/news.php?id=1;exec(master..xp_cmdshell 'net user')`可以改为`GET /pen/news.php?id=1; exec('maste'+'r..xp'+'_cmdshell'+'"net user"')`

# 10、数据格式混淆

# 11、编码绕过
## url编码、Unicode编码
## url编码对数据进行二次url编码,waf进行一次解码并不能解析到有效的数据
## 将`id=1 and 1=1`写为`id=1/*!and*/1=1`

# 12、利用waf本身的功能绕过
## 把"*"替换为空
## 如`uni*on+sel*ect+1,2,3,4....`
## 其它方法-15+(uNioN)+(sElECt)….-15+(uNioN+SeleCT)+…-15+(UnI)(oN)+(SeL)(ecT)+….-15+union (select 1,2,3,4…)

# 13、利用http协议绕过waf

# 14、请求方式转换
## 如将GET请求包转换为POST请求包

# 15、文件上传绕过
## (1)等号绕过 :如在filename后多加两个等号
## (2)换行绕过 :如在文件后缀名处换行
## (3)填充垃圾字符 :有些主句waf软件为了不影响web服务器的性能,会对校验的用户数据设置大小上限,如1M;可以构造一个大文件,前面1M的内容为垃圾内容后面才是真正的木马内容
## (4)NTFS ADS特性绕过
## (5)利用waf的缺陷
### 在恶意代码前加垃圾数据
### 在数据包前加垃圾数据
### 在`Content-Disposition`参数后面加垃圾数据
### 多加一个filename
### 更改HTTP请求方法
### 删除实体里面的Conten-Type字段；第一种是删除Content整行，第二种是删除C后面的字符。删除掉ontent-Type: image/jpeg只留下c，将.php加c后面即可，但是要注意额，双引号要跟着php
### 删除`Content-Disposition`字段里的空格
### 增加一个空格
### 修改`Content-Disposition`字段值的大小写
### 文件名后缀处回车
### 多个`Content-Disposition`
## (6)双文件上传
### 上传点支持多文件上传,但却只对第一个文件做了过滤
## (7)条件竞争上传
## (8)特性
### 多个filename
### 交换name和filename的顺序
### 去掉引号,双引号变成单引号
### 大小写
### 去掉或修改Content-Disposition值
### 多个boundary
### 多个分号
### Header在boundary前添加任意字符
### filename换行
### name和filename添加任意字符串
### POST<=>GET