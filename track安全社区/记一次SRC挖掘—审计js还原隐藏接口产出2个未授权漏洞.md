https://bbs.zkaq.cn/t/32673.html#top

# 从一个运维系统开始,通过审计JS中的Chunk映射,还原出未在首页直接加载、未被常规插件识别的JS文件,并沿着请求封装继续定位到多个隐藏接口

# 雪瞳和小熊猫插件(FindSomething)识别接口

## 查看引用的JS内容(xxx.edu.cn/static/js/xxx.js),发现`baseURL /api`
## 继续查看JS内容,发现Chunk映射("chunk-xxxxxx":"xxxxxx")
## 计算出的完整地址：https://xxxxx.xxxxx.edu.cn/static/js/chunk-xxxxx.xxxxx.js
## 打开发现众多插件未识别出来的接口
## 恢复接口发包成功获取全站用户名和MD5密码等敏感信息
## 查询一个未来的日期,发现没有排班信息,通过savaxxx接口给用户写入2099-12-31排班日期,成功查询出写入的排班信息,存在未授权排班信息写入漏洞

# 隐藏模块、前端未暴露但服务端缺少授权控制的未授权接口

# Vue :JS框架
# Webpack :静态模块打包器,将零散的.vue、.js、.css文件打包成大文件(如chunk-vendors.js)

# Chunk映射 :建立一种对应关系,将数据块(Chunk)和它的物理位置关联起来;在前端指向资源文件路径,在数据库和存储中指向物理存储位置