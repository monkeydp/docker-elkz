# Elastic stack (ELK) + Zombodb on Docker 中文版

前置步骤参考 [Elastic stack (ELK) on Docker 中文版](https://github.com/monkeydp/docker-elk)

## 1. Zombodb 教程

[官方教程](https://github.com/zombodb/zombodb/blob/master/TUTORIAL.md)

### 1.1 创建表

```sql
CREATE TABLE products (
    id int8 NOT NULL PRIMARY KEY,
    name text NOT NULL,
    keywords varchar(64)[],
    short_summary text,
    long_description zdb.fulltext, 
    price bigint,
    inventory_count integer,
    discontinued boolean default false,
    availability_date date
);
```

### 1.2 插入数据

```sql
INSERT INTO "public"."products" VALUES (1, '小夜灯', '{light}', '智能小夜灯', '一款智能小夜灯, 支持定时开关', 10, 2200, 'f', '2021-04-17');
INSERT INTO "public"."products" VALUES (3, '灭火器', '{extinguishing}', '智能灭火器', '一款智能灭火器, 支持烟雾感应自动启动', 200, 1000, 'f', '2021-07-05');
INSERT INTO "public"."products" VALUES (2, '灭蚊灯', '{light,mosquito}', '智能灭蚊灯', '一款智能灭蚊灯, 支持击杀统计', 30, 0, 't', '2021-11-23');
```

### 1.3 创建索引

192.168.x.x 是你的本机 ip

```sql
CREATE INDEX idxproducts 
ON products 
USING zombodb ((products.*)) 
WITH (url='http://elastic:changeme@192.168.x.x:9200/');
```

打开 `Kibana` → 左侧栏 `Discover`, 创建索引模式 `postgres.*`

可以看到刚才创建的索引

![](.README/kibana_discover.png)

### 1.4 查询数据

```sql
-- 查询单个字段
SELECT * FROM products 
WHERE products ==> 'name:灭';

-- 查询数组
SELECT * FROM products 
WHERE products ==> 'keywords: mosquito OR extinguishing';

-- 查询小于指定值
SELECT * FROM products 
WHERE products ==> 'price<50';
```
