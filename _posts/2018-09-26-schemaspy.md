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

## 提供更多 SchemaMeta 信息

按照 [SchemaMeta](https://schemaspy.readthedocs.io/en/latest/configuration.html#schemameta) 规定的格式编写 xml ，通过 `schemaspy.meta=文件名` 传入即可。

## 参考

* [schemaspy/schemaspy](https://github.com/schemaspy/schemaspy)
* [SchemaSpy: documentation](https://schemaspy.readthedocs.io/en/latest/index.html)
