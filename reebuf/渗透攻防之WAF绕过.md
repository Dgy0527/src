https://www.freebuf.com/articles/defense/381473.html

# WAF绕过

# 1、双写注入绕过
## 将`and`双写为`anandd`

# 2、大小写绕过

# 3、脏数据绕过
## 传入一长串数据使waf失效,从而实现绕过waf;某些waf处理POST数据时,只会检测开头的8k,后面选择全部放过

# 4、复参数绕过(&id=)
## 例如一个请求`GET /pen/news.php?id=1 union select user,password from mysql.user`可以修改为`GET pen/news.php?id=1&id=union&id=select&id=user,password&id=from%20mysql.user`;很多waf都可以这样绕,测试最新版waf能绕过不符语句