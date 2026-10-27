# 1、`ssh-keygen -f key -N '' key key.pub` 
## 生成一对SSH密钥;当前目录下会生成两个文件,key(绝密,不能给别人)和key.pub(可以公开)
## `-f key` :指定生成的文件名为key(私钥)和key.pub(公钥)
## `-N ''` :表示不设置密码短语,回车直接生成

# 2、`ssh -i key hacker@dojo.pwn.college`
## 使用指定的私钥登录远程服务器
## `-i key` :指定身份文件,告诉SSH客户端用`key`这个私钥去证明我的身份
## `hacker@dojo.pwn.college` :目标服务器的用户名和地址