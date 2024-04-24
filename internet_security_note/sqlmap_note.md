# sqlmap的使用
---
## sqlmap介绍
sqlmap是一个自动化的sql注入工具，其主要功能是扫描、发现并利用给定URL的SQL注入漏洞，内置了很多绕过插件，支持的数据库有MySQL, Oracle,PostgreSQL, Microsoft SQL Server, Microsoft Access, IBM DB2, SQLite, Firebird,Sybase和SAP MaxDB。
sqlmap支持五种不同的注入模式：

- 基于布尔的盲注，即可以根据返回页面判断条件真假的注入；
- 基于时间的盲注，即不能根据页面返回内容判断任何信息，用条件语句查看时间延迟判断语句是否执行（即页面返回时间是否增加）来判断；
- 基于报错的注入，即页面会返回错误信息，或者把注入的语句的结果返回在页面里；
- 联合查询注入，可以使用union的情况下的注入；
- 堆查询注入，可以同时执行多条语句的执行时的注入

## sqlmap入门

在kali环境下使用sqlmap可以直接使用，Windows环境需要添加环境变量等操作。
工具的简单使用大致可以分为:

1.判断是否存在注入点
2.