# shiro

> 作者：子墨同学
>
> 时间：2020-04-16

##### shiro标签

```html
    <html lang="en" xmlns:th="http://www.thymeleaf.org"
          xmlns:shiro="http://www.pollix.at/thymeleaf/shiro">

<!-- 自行测试即可-->
<shiro:authenticated>
       <span>
           通过登录或者记住我的
       </span>
</shiro:authenticated>

<shiro:guest>
    <span>你是游客 没有用户信息的</span>
</shiro:guest>

<shiro:hasPermission name="user:add">
    <span>用户必须具有某一个权限才能访问</span>
</shiro:hasPermission>

<shiro:hasAllRoles name="buyer,seller">
    <span>拥有某一个角色下面才显示</span>
</shiro:hasAllRoles>

<shiro:lacksPermission>
    <span>没有某一个权限的时候才能访问</span>
</shiro:lacksPermission>

<shiro:lacksRole name="xxx">
    <span>没有某一个角色的时候才能访问</span>
</shiro:lacksRole>

<shiro:notAuthenticated>
    <span>没有认证通过才能显示</span>
</shiro:notAuthenticated>
```









## 参考资料

1. [shiro的使用（四）—springboot整合shiro之高级应用](https://blog.csdn.net/fan521dan/article/details/104548262/)