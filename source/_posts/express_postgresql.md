---
title: EXPRESS + POSTGRESQL 学习笔记
date: 2023-04-13 21:42:36
tags: express postgresql
categories: 学习笔记
---
# EXPRESS + POSTGRESQL 学习文档

## 认识工具

### EXPRESS

1. 基于nodejs的后端服务器框架，[express官网](http://expressjs.com/zh-cn/)

### POSTGRESQL

1. 免费的对象-关系型数据库服务器，[postgresql官网](https://www.postgresql.org/)，[教程](https://www.runoob.com/postgresql/postgresql-tutorial.html)

### DBeaver

1. 免费的可视化数据库管理软件，[DBeaver官网](https://dbeaver.io/)


### 服务器代码使用第三方库

1. [Sequelize](https://www.sequelize.cn/core-concepts/getting-started)，服务器链接数据库的第三方库
2. [pg pg-hstore](https://github.com/brianc/node-postgres)， Sequelize 链接 postgresql 数据库需要的驱动
3. [cors](https://github.com/expressjs/cors)，express服务器CORS配置中间件


## 服务器搭建步骤

### POSTGRESQL 模块（基于Centos 7）

1. 安装 postgresql 
```
sudo yum install postgresql-server postgresql-contrib
```

2. 初始化数据库
```
sudo postgresql-setup initdb
```

3. 启动服务
```
sudo systemctl start postgresql
```

4. 配置开机自启动
```
sudo systemctl enable postgresql
```

5. 配置远程访问
- 默认情况下 postgresql 只允许本地访问，如果远程访问，需要修改 pg_hba.conf 和 postgresql.conf 文件，文件路径如下
```
/var/lib/pgsql/data/
```
- 修改 postgresql.conf 文件，找到以下行
```
//修改前
# listen_addresses = 'localhost' 
# PORT=5432

// 修改后
listen_addresses = '*' 
PORT=5432
```
- 修改 pg_hba.conf 文件，在最末尾添加
```
host  all   all  0.0.0.0/0   md5
```
- 重启 postgresql 服务
```
sudo systemctl restart postgresql
```

- 如果无法重启并报以下错误，请检查上述两个文件修改后的文件权限，是否属于 postgres 用户。如果不是，可以通过如下命令修改
```
sudo chown postgres:postgres /var/lib/pgsql/data/postgresql.conf
sudo chown postgres:postgres /var/lib/pgsql/data/pg_hba.conf
```