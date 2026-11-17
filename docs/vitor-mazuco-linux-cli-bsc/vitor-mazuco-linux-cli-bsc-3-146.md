# 146：临时表空间管理 🗂️

在本节课中，我们将要学习Oracle数据库中的临时表空间。临时表空间用于存储数据库会话期间产生的临时数据，例如排序操作的中间结果。这些数据在会话结束后会自动被清除，类似于Linux系统中的 `/tmp` 目录。

上一节我们介绍了其他类型的表空间，本节中我们来看看临时表空间的具体管理和操作。

## 临时表空间概述

临时表空间（Temporary Tablespace）在Oracle中简称为“temp”。其存在是为了提升数据库性能，专门用于处理临时性数据。常见的用途包括存储不适合在内存中完成的大型排序操作所产生的临时数据。这些数据是临时的，仅在数据库会话存续期间存在，会话结束后会自动被移除。

Oracle数据库在创建时会生成一个默认的临时表空间。用户也可以根据需要创建新的临时表空间，或更改默认设置。

## 查看临时表空间

以下是查看当前数据库临时表空间信息的命令。



```sql
SELECT * FROM DBA_TEMP_FILES;
```


执行此命令可以查看已分配的临时表空间文件、其路径以及总大小。


## 创建新的临时表空间

如果需要创建新的临时表空间，可以使用 `CREATE TEMPORARY TABLESPACE` 命令。

以下是创建新临时表空间的语法和示例。



```sql
CREATE TEMPORARY TABLESPACE temp2
TEMPFILE '/u01/app/oracle/oradata/ORCL/temp02.dbf'
SIZE 500M;
```

此命令创建了一个名为 `temp2` 的临时表空间，指定了其数据文件路径，并设置了初始大小为500MB。


## 更改默认临时表空间


数据库的默认临时表空间可以更改。在更改之前，需要先确认当前的默认设置。

以下是查看和更改默认临时表空间的步骤。


首先，查看当前的默认临时表空间：


```sql
SELECT PROPERTY_NAME, PROPERTY_VALUE
FROM DATABASE_PROPERTIES
WHERE PROPERTY_NAME = 'DEFAULT_TEMP_TABLESPACE';
```


然后，使用 `ALTER DATABASE` 命令进行更改：



```sql
ALTER DATABASE DEFAULT TEMPORARY TABLESPACE temp2;
```



此命令将默认临时表空间从原来的 `temp` 更改为新创建的 `temp2`。


## 删除临时表空间



请注意，无法直接删除当前作为默认的临时表空间。必须先将默认设置更改为其他表空间，然后才能执行删除操作。


以下是删除临时表空间的完整流程。



1.  确保目标表空间不是默认表空间。如果不是，请先更改默认设置。
2.  使用 `DROP TABLESPACE` 命令进行删除：



```sql
DROP TABLESPACE temp INCLUDING CONTENTS AND DATAFILES;
```


此命令会删除名为 `temp` 的表空间，包括其中的所有内容和对应的数据文件。删除后，可以再次执行查询命令确认该表空间已不存在。


## 总结


![](img/27848eb2d57cbe8d8839fe79028358a5_49.png)

本节课中我们一起学习了Oracle数据库临时表空间的管理。我们了解了临时表空间的作用，掌握了如何查看、创建新的临时表空间，以及如何更改默认设置和删除不再需要的临时表空间。关键点在于，删除临时表空间前必须确保其不是当前的默认表空间。