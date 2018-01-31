
##Query（DML）##
<https://docs.microsoft.com/en-us/sql/t-sql/queries/queries>


## 函数 ##
<https://docs.microsoft.com/en-us/sql/t-sql/functions/functions>
可以使用内置函数，以及自定义函数实现一些高级功能。

### Rowset functions ###
Rowset functions Return an object that can be used like table references in an SQL statement.
- [OPENXML](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/openxml-transact-sql)
- [OPENJSON](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/openjson-transact-sql)
- [OPENQUERY](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/openquery-transact-sql)
- [OPENROWSET](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/openrowset-transact-sql)
- [OPENDATASOURCE](https://docs.microsoft.com/zh-cn/sql/t-sql/functions/opendatasource-transact-sql)

#### OPENROWSET ####
The OPENROWSET function can be referenced in the FROM clause of a query as if it were a table name.
The OPENROWSET function can also be referenced as the target table of an INSERT, UPDATE, or DELETE statement, subject to the capabilities of the [OLE DB provider](https://msdn.microsoft.com/en-us/library/ms709836(v=vs.85).aspx).
Although the query might return multiple result sets, OPENROWSET returns only the first one. 

OPENROWSET also supports bulk operations through a built-in BULK provider that enables data from a file to be read and returned as a rowset.

可通过OPENROWSET函数，连接到外部数据库（ad hoc manner,use linked servers for frequently reference）或者**文件**。

> Uses the BULK rowset provider for OPENROWSET to read data from a file.In SQL Server, OPENROWSET can read from a data file without loading the data into a target table. This lets you use OPENROWSET with a simple SELECT statement. 

**Remarks**

> OPENROWSET can be used to **access remote data** from OLE DB data sources **only when** the DisallowAdhocAccess registry option is explicitly set to 0 for the specified provider, and the Ad Hoc Distributed Queries advanced configuration option is enabled. When these options are not set, the default behavior does not allow for ad hoc access. 

> OPENROWSET does not accept variables for its arguments. 

##### Using OPENROWSET with the BULK Option #####

A correlation name must be specified for the bulk rowset in the from clause. 
   FROM OPENROWSET(BULK...) AS table_alias[(column_alias,...n)]

可以使用 BULK INSERT 或者 OPENROWSET(BULK...) 方法导入数据 [bulk imports data from a data file into a SQL Server table](https://docs.microsoft.com/en-us/sql/relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server)

使用OPENROWSET 的 BULK 选项时，需要结合使用Format文件：
* [Create a Format File](https://docs.microsoft.com/en-us/sql/relational-databases/import-export/create-a-format-file-sql-server)
* [Non-XML Format](https://docs.microsoft.com/en-us/sql/relational-databases/import-export/non-xml-format-files-sql-server)
* [Specify Field and Row Terminators](https://docs.microsoft.com/en-us/sql/relational-databases/import-export/specify-field-and-row-terminators-sql-server)**When you use native or Unicode native format, use length prefixes rather than field terminators**
* [Use Format File](https://docs.microsoft.com/en-us/sql/relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server)

不确定如何使用format file时，使用bcp（bulk copy program）工具

        # 使用相同的格式导出、导入数据
        PS C:\> bcp MoliVideo20_Dev.dbo.Site out .\test.bcp -U <userID> -P <password> -S <host> -w
        PS C:\> bcp MoliVideo20_Dev.dbo.Site in .\test.bcp -U <userID> -P <password> -S <host> -w

        # 使用相同的格式导出数据（test.bcp）和format file（test.fmt），然后再T-SQL中使用format file
        PS C:\> bcp MoliVideo20_Dev.dbo.Site format nul -f ./test.fmt -U <userID> -P <password> -S <host> -w ##ErrorLog -e .\BCP\Error_out.log -o .\BCP\Output_out.log
        PS C:\> bcp MoliVideo20_Dev.dbo.Site out ./test.bcp -U <userID> -P <password> -S <host> -w
        PS C:\> bcp MoliVideo20_Dev.dbo.Site in .\test.bcp -f .\test.fmt -U <userID> -P <password> -S <host>

        select * from 
        openrowset(bulk '/test.bcp', 
            formatfile = '/test.fmt') as t;