panic是什么

​	

1. 路由里：name(路由参数的类型有哪些)
2. context 啥意思



SECTION2： 创建controller接口 、路由  

SECTION3: 编写查找用户、添加用户 接口

>1. ShouldBindJSON / Query(Key)
>2. c.JSON
>3. gin.H
>4. gorm {select, insert}

SECTION4：用户密码加密(bcrypt, scrypt)

好文https://astaxie.gitbooks.io/build-web-application-with-golang/content/zh/09.5.html

SECTION5: 更新用户，删除用户

> 1. gorm {delete软删除}
> 2. gin   c.Param c.Query  的区别是什么  获取入参



<font color=red>出现的问题：</font> 

> 1. category、user、article都在一个model包下   按照之前的喜欢命名 insert 、getById 会重复  目前只能每个方法后加上Category、user的后缀  有没有更好的办法解决
> 2. retrun 抽离公共方法返回  code、msg、data为参数
> 3. 入参加日志、 sql加日志 