# Mybatis

## mybatis占位符的处理

1. 占位符一：`#{xxx}`

- reparedStatement预编译sql语句
-  xxx表达式的写法
  - 参数类型为javabean类，xxx表达式必须和javabean中属性对应的get方法名字一样
  - 参数类型为简单类型，xxx表达式随表写，保持和参数的名字一致

2. 占位符二：`${xxx}`

- Statement拼接sql语句
- xxx表达式的写法
  - 参数类型为javabean类，xxx表达式必须和javabean中属性对应的get方法名字一样
  - 参数类型为简单类型，xxx表达式执行能写${value}

## 模糊查询

**在查询参数有前后加%%[不推荐]**

```java
User user = new User();
user.setName("%张三%");
//......
```

**使用$**

```xml
<select id="selectLike" resultType="com.zimo.mybatis.demo.User" parameterType="com.zimo.mybatis.demo.User">
	SELECT id,Name FROM sys_user WHERE name like '%${name}%'
</select>    
```

**使用数据库方法 concat连接字符串的方法**

```xml
<select id="selectLike" resultType="com.zimo.mybatis.demo.User" parameterType="com.zimo.mybatis.demo.User">
	SELECT id,Name FROM sys_user WHERE name like CONCAT('%',#{name},'%')
</select> 
```

**更改别名**

```xml
<select id="selectLike" resultType="com.zimo.mybatis.demo.User" parameterType="com.zimo.mybatis.demo.User">
    <bind name="newName" value="'%'+name+'%'"/>
	SELECT id,Name FROM sys_user WHERE name like #{newName}
</select> 
```

## SQL片段

**目地：提高sql的复用性**

```xml
<!-- 定义sql片段 -->
<sql id="selectUser">
   select * from User 
</sql>
<!-- 查询某个商品的详细信息 -->
<select id="loadUserById" parameterType="java.lang.Integer" 
        resultType="com.bjsxt.pojo.User">
<!-- select * from user where uid=${value}         -->
   <!-- 引用sql片段 -->
   <include refid="selectUser"></include>
   where pid=${value}  
</select>
```

## 动态sql

- if
- choose (when, otherwise)
- where, set
- foreach

**if**

```xml
<!--更新好友申请通知消息-->
<update id="updateApplicationSelective">
    UPDATE webchat_friend_application
    <set>
        <if test="content != null">
            content = #{content,jdbcType=VARCHAR},
        </if>
        <if test="time != null">
            create_time = #{time,jdbcType=TIMESTAMP},
        </if>
    </set>
    WHERE uid = #{uid,jdbcType=INTEGER}
    AND from_id =#{from,jdbcType=INTEGER}
</update>
```

**choose**

有些时候，我们不想用到所有的条件语句，而只想从中择其一二。针对这种情况，MyBatis 提供了 choose 元素，它有点像 Java 中的 switch 语句。

```xml
    <select id="selectLike" resultMap="UserMap">
        SELECT id,
               username,
               email,
               avatar,
               birth
        FROM sys_user
        <where>
            <choose>
                <when test="username!=null">
                    username like concat('%',#{username},'%')
                </when>
                <when test="accountId!=null">
                    accountId like concat('%',#{accountId},'%')
                </when>
                <otherwise>
    
                </otherwise>
            </choose>
        </where>
    </select>
```

以上的是如果用户提供了username就按username查，提交accountId就按accountId查，否则就走otherwise

**where  set**

where就相就是sql后台拼接一个where关键字

set多用于更新中

**foreach**

foreach 元素的功能是非常强大的，它允许你指定一个集合，声明可以用在元素体内的集合项和索引变量。它也允许你指定开闭匹配的字符串以及在迭代中间放置分隔符。这个元素是很智能的，因此它不会偶然地附加多余的分隔符。

```xml
<!--根据RoleIDS查询权限-->
<select id="queryByRoleIds" resultMap="PermissionResultMap">
    SELECT
    p.id,
    p.CODE,
    p.href
    FROM
    sys_role_permission rp
    LEFT JOIN sys_permission p ON rp.permission_id = p.id
    WHERE
    rp.role_id in
    <foreach collection="list" item="item" open="(" close=")" separator=",">
        #{item}
    </foreach>
</select>
```

