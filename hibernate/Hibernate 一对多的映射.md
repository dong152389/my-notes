# Hibernate一对多的映射

## 一对多

* 什么样的关系是属于一对多的的
  * 一个部门可以对应多个员工,一个员工只能属于某一个部门
  * 一个客户可以对应多个联系人,一个联系人只能属于一个某一个客户
* 在多的一方创建外键指向一的一方的主键

## 多对多

* 什么样的关系属于多对多
  * 一个学生可以选择多门课程,一门课程可以被多个学生选择
  * 必选创建一个中间表存放外键

## 一对一

* 什么样的关系属于一对一
  * 一个公司只能有一个注册地址,一个注册地址只能被一个公司注册