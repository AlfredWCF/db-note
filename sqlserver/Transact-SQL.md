<https://docs.microsoft.com/en-us/sql/t-sql/functions/functions>
##函数##
可以使用内置函数，以及自定义函数实现一些高级功能。

###Rowset functions###
Rowset functions Return an object that can be used like table references in an SQL statement.
- [OPENXML](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/openxml-transact-sql)
- [OPENJSON](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/openjson-transact-sql)
- [OPENQUERY](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/openquery-transact-sql)
- [OPENROWSET](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/openrowset-transact-sql)
- [OPENDATASOURCE](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/opendatasource-transact-sql)

####OPENROWSET####
The OPENROWSET function can be referenced in the FROM clause of a query as if it were a table name.
The OPENROWSET function can also be referenced as the target table of an INSERT, UPDATE, or DELETE statement, subject to the capabilities of the [OLE DB provider](https://msdn.microsoft.com/en-us/library/ms709836(v=vs.85).aspx).
Although the query might return multiple result sets, OPENROWSET returns only the first one. 

OPENROWSET also supports bulk operations through a built-in BULK provider that enables data from a file to be read and returned as a rowset.

