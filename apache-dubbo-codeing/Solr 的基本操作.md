# Solr 的基本操作

## 功能界面

### Dashboard

![img](./img/Lusifer1512745071.png)

### Logging

![img](./img/Lusifer1512745395.png)

### Core Admin

![img](./img/Lusifer1512745505.png)

### Thread Dump

![img](./img/Lusifer1512745735.png)

### Core

![img](./img/Lusifer1512746145.png)

## 分析功能

### 修改 managed-schema 配置业务系统字段

需要用到的业务字段如下：

- 商品 ID
- 商品分类 ID
- 分类名称
- 商品名称
- 商品卖点
- 商品详情

由于 Solr 中自带 id 字段所以无需添加，其它字段需要手动添加 Solr 字段

```
# 字段域
<field name="tb_item_cid" type="plong" indexed="true" stored="true"/>
<field name="tb_item_cname"  type="text_ik" indexed="true" stored="true"/>
<field name="tb_item_title"  type="text_ik" indexed="true" stored="true"/>
<field name="tb_item_sell_point" type="text_ik" indexed="true" stored="true" />
<field name="tb_item_desc" type="text_ik" indexed="true" stored="true" />

# 复制域（Solr 的搜索优化功能，将多个字段域复制到一个域里，提高查询效率）
<field name="tb_item_keywords" type="text_ik" indexed="true" stored="false" multiValued="true"/>
<copyField source="tb_item_cname" dest="tb_item_keywords"/>
<copyField source="tb_item_title" dest="tb_item_keywords"/>
<copyField source="tb_item_sell_point" dest="tb_item_keywords"/>
<copyField source="tb_item_desc" dest="tb_item_keywords"/>
```

![img](./img/Lusifer_20181107015350.png)

### 复制配置到容器并重启

```text
# 复制到容器
docker cp managed-schema solr:/opt/solr/server/solr/ik_core/conf

# 重启容器
docker-compose restart
```

### 分词效果图

![img](./img/Lusifer1520889921.png)

## 维护功能

维护功能即对数据库的 CRUD 操作

### 添加索引库

```json
{
    "id": 536563,
    "tb_item_cid": 560,
    "tb_item_cname": "手机",
    "tb_item_title": "new2 - 阿尔卡特 (OT-927) 炭黑 联通3G手机 双卡双待",
    "tb_item_sell_point": "清仓！仅北京，武汉仓有货！"
}
```

![img](./img/Lusifer_20181107021450.png)

### 测试查询

![img](./img/Lusifer_20181107021538.png)

### 删除索引库

设置文档类型为 XML

![img](./img/Lusifer1520892036.png)

### 根据 ID 删除

```text
<delete>
    <id>536563</id>
</delete>
<commit />
```

### 根据查询删除

```text
<delete>
    <query>*:*</query>
</delete>
<commit/>
```

## 查询功能

### 查询条件

![img](./img/Lusifer1520892126.png)

说明：

- q：查询条件，*:* 为查询所有域，单独查询某个域如：`tb_item_title:手机`
- fq: 过滤条件
- sort：排序条件
- start,rows：分页条件
- fl：字段列表返回域，如只希望返回 `id`
- df：默认搜索域，如之前配置的复制域 `tb_item_keywords`

### 高亮显示

![img](./img/Lusifer_20181107022036.png)

![img](./img/Lusifer_20181107022156.png)

说明：上图意为在默认搜索域 `tb_item_keywords` 中搜索关键字 `手机` 并指定需要高亮显示的结果域 `tb_item_title` 以 `红色` 显示