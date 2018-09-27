---
title: 使用 SchemaSpy 生成数据库文档
layout: post
---

# 使用 SchemaSpy 生成数据库文档

1. 安装 Java 和 Graphviz， 确保 PATH 中含有 `java` 和 `dot` 两个程序。
2. 下载 SchemaSpy ，就是一个 jar 包
3. 准备数据库相关的驱动 ，一般情况下也是一个 jar 包
4. 编写 `schemaspy.properties` ，例子如下：

```ini
# type of database. Run with -dbhelp for details
schemaspy.t=mysql
# optional path to alternative jdbc drivers.
schemaspy.dp=./mysql/mysql-connector-java-5.1.46.jar
# database properties: host, port number, name user, password
schemaspy.host=localhost
schemaspy.port=3306
schemaspy.db=[dbname]
schemaspy.u=[username]
schemaspy.p=[password]
# output dir to save generated files
schemaspy.o=./generated
# db scheme for which generate diagrams
schemaspy.s=dbname
```

5. 运行 `java -jar schemaspy-6.0.0.jar` 

```
  ____       _                          ____
 / ___|  ___| |__   ___ _ __ ___   __ _/ ___| _ __  _   _
 \___ \ / __| '_ \ / _ \ '_ ` _ \ / _` \___ \| '_ \| | | |
  ___) | (__| | | |  __/ | | | | | (_| |___) | |_) | |_| |
 |____/ \___|_| |_|\___|_| |_| |_|\__,_|____/| .__/ \__, |
                                             |_|    |___/

                                              

SchemaSpy generates an HTML representation of a database schema's relationships.
SchemaSpy comes with ABSOLUTELY NO WARRANTY.
SchemaSpy is free software and can be redistributed under the conditions of LGPL version 3 or later.
http://www.gnu.org/licenses/

INFO  - Starting Main on biggates-PC with PID 16328 (E:\git\schemaspy\target\classes started by bigga in E:\git\schemaspy)
INFO  - The following profiles are active: default
INFO  - Found configuration file: schemaspy.properties
INFO  - Started Main in 2.136 seconds (JVM running for 3.082)
INFO  - Starting schema analysis
INFO  - Connected to MySQL - 5.1.73-log
INFO  - Gathering schema details
Gathering schema details..................................................................................................................................................................................
WARN  - Failed to retrieve stored procedure/function details: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Unknown table 'parameters' in information_schema: select specific_name, parameter_name, dtd_identifier, parameter_mode from information_schema.parameters where specific_schema=:schema and ordinal_position != 0 order by ordinal_position
(1sec)
Connecting relationships.................................................................................................................................................................................(17sec)
Writing/graphing summary.INFO  - Gathered schema details in 17 seconds
INFO  - Writing/graphing summary
.....(35sec)
Writing/diagramming detailsINFO  - Completed summary in 35 seconds
INFO  - Writing/diagramming details
..............................................................................................................................................................................(24sec)
Wrote relationship details of 174 tables/views to directory '.\generated' in 79 seconds.
View the results by opening .\generated\index.html
INFO  - Wrote table details in 24 seconds
INFO  - Wrote relationship details of 174 tables/views to directory '.\generated' in 79 seconds.
INFO  - View the results by opening .\generated\index.html

```

生成的效果还是比较美观的，其中一些 ER 图基本上可以直接拿来做插图用。可以在 [schemaspy sample](http://schemaspy.org/sample/index.html) 查看示意。

## 一些注意事项

因为 `-dbHelp` 太弱鸡了，全部支持的 database type 和一些细节可以在 [这里](https://github.com/schemaspy/schemaspy/tree/master/src/main/resources/org/schemaspy/types) 研究。

这里也整理常见的一些信息如下，请注意在 Maven Repository 中下载相应的 driver：

### MySQL

请注意较新版本的 driver 不支持较旧的 (5.1) 数据库。

```
schemaspy.t=mysql
schemaspy.dp=mysql-connector-java-5.1.46.jar
```

### MS SQL Server

MS SQL Server 的 JDBC driver 在 Maven Repo 中能够获取到的只有比较新的版本，请注意 `t` 需要指定 `mssql05` 或者 `mssql08`，按实际情况选择。


```ini
schemaspy.t=mssql05
# 这个 driver 在 maven 上没有
schemaspy.dp=sqljdbc4-3.0.jar
```

```ini
schemaspy.t=mssql08
schemaspy.dp=mssql-jdbc-6.2.2.jre8.jar
```

## 提供更多 SchemaMeta 信息

在某些情况下（比如数据库中没有 comment ，或者需要手动调整外键之类的时候），可能需要手动对结果做一些修改。

按照 [SchemaMeta](https://schemaspy.readthedocs.io/en/latest/configuration.html#schemameta) 规定的格式编写 xml ，通过 `schemaspy.meta=文件名` 传入即可。

当然了最好还是做好基础工作。

## 参考

* [schemaspy/schemaspy](https://github.com/schemaspy/schemaspy)
* [SchemaSpy: documentation](https://schemaspy.readthedocs.io/en/latest/index.html)
