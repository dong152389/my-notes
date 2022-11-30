# MyBatis 单表 CRUD 操作

## 概述

MyBatis 的单表 CRUD 的相关操作方法

## INSERT

继续以 `tb_user` 表为例，修改映射文件，增加如下配置：

```xml
<insert id="insert">
    INSERT INTO tb_user (
      id,
      username,
      password,
      phone,
      email,
      created,
      updated
    )
    VALUES
      (
        #{id},
        #{username},
        #{password},
        #{phone},
        #{email},
        #{created},
        #{update}
      )
</insert>
```

单元测试代码如下：

```java
@Test
public void testInsert() {
    TbUser tbUser = new TbUser();
    tbUser.setEmail("admin@admin.com");
    tbUser.setPassword("admin");
    tbUser.setPhone("15888888888");
    tbUser.setUsername("Lusifer");
    tbUser.setCreated(new Date());
    tbUser.setUpdate(new Date());

    tbUserDao.insert(tbUser);
}
```

## DELETE

继续以 `tb_user` 表为例，修改映射文件，增加如下配置：

```xml
<delete id="delete">
    DELETE FROM tb_user WHERE id = #{id}
</delete>
```

单元测试代码如下：

```java
@Test
public void testDelete() {
    TbUser tbUser = new TbUser();
    tbUser.setId(37L);
    tbUserDao.delete(tbUser);
}
```

## 查询单个对象

继续以 `tb_user` 表为例，修改映射文件，增加如下配置：

```xml
<select id="getById" resultType="TbUser">
    SELECT
      a.id,
      a.username,
      a.password,
      a.phone,
      a.email,
      a.created,
      a.updated AS "update"
    FROM
      tb_user AS a
    WHERE
      a.id = #{id}
</select>
```

单元测试代码如下：

```java
@Test
public void testGetById() {
    TbUser tbUser = tbUserDao.getById(36L);
    System.out.println(tbUser.getUsername());
}
```

## UPDATE

继续以 `tb_user` 表为例，修改映射文件，增加如下配置：

```xml
<update id="update">
    UPDATE
      tb_user
    SET
      username = #{username},
      password = #{password},
      phone = #{phone},
      email = #{email},
      created = #{created},
      updated = #{update}
    WHERE id = #{id}
</update>
```

单元测试代码如下：

```java
@Test
public void testUpdate() {
    TbUser tbUser = tbUserDao.getById(36L);
    tbUser.setUsername("Lusifer");

    tbUserDao.update(tbUser);
}
```

## 使用模糊查询

继续以 `tb_user` 表为例，修改映射文件，增加如下配置：

```xml
<select id="selectByName" resultType="TbUser">
    SELECT
      a.id,
      a.username,
      a.password,
      a.phone,
      a.email,
      a.created,
      a.updated AS "update"
    FROM
      tb_user AS a
    WHERE
      a.username LIKE CONCAT ('%', #{username}, '%')
</select>
```

在进行模糊查询时，需要进行字符串的拼接。SQL 中的字符串的拼接使用的是函数 `concat(arg1, arg2, …)` 。注意不能使用 Java 中的字符串连接符 `+`。

单元测试代码如下：

```java
@Test
public void testSelectByName() {
    List<TbUser> tbUsers = tbUserDao.selectByName("uni");
    for (TbUser tbUser : tbUsers) {
        System.out.println(tbUser.getUsername());
    }
}
```