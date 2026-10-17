https://bbs.zkaq.cn/t/32674.html#top

# 从一个需要登陆的Drupal站点开始,先通过页面源码识别出Drupal 7,再从前端JS引用中发现`/sites/all/modules/custom/common/`自定义模块路径,随后根据Drupal模块命名规则尝试访问`common.info`和`common.module`,成功获取模块源码

## 首页无权限,需要登录
## 查看页面源码发现使用了Drupal 7 (cms)
### Drupal :用PHP编写的、免费且开源的CMS(内容管理系统),和WordPress同一类
### 并且发现引用的JS资源存在于此路径`/sites/all/modules/custom`下,此路径是Drupal的站点自定义模块
### 查看其中`/sites/all/modules/custom/common/xxxx_xxxx_view.js`发现资源下载相关路径:`?q=xxxx/xxxx/downloadResource/`,说明站点存在资源下载功能
## 根据Drupal 7的模块文件命名规则,`/sites/all/modules/custom/common`中目录名为common的模块通常包含`common.info`和`common.module`
## 尝试访问`https://xxxx/sites/all/modules/custom/common/common.module`成功下载源码
## 审计源码发现三个关键证据
### 真实路由是`downloadResourceBegin/%`
### 回调函数是`downloadResourceBegin`
### `access callback => true`表示该路由没有登录权限判断
## 继续查看源码中的正常下载函数
## 可以确认正常业务会把文件URL拼接到`?q=downloadResourceBegin/<下载文件的路径>`
## 尝试构造请求,成功实现任意我呢见读取
## 后续继续审计发现一个布尔sql注入,设置1=2条件为假,立马响应;1=1条件为真,响应超时,推测是sleep时间设置太长了,数据库执行多次sleep导致的超时,将sleep时间设置为0.3,成功延迟20秒后响应
## 通过fofa发现32条匹配结果,证明为通用系统