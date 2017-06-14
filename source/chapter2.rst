第2章 SQL命令参考
=================

   Greenplum数据库中可以使用如下的SQL命令：
   
   * :ref:`ABORT <abort>`
   * :ref:`ALTER AGGREGATE<alter-aggregate>`
   * :ref:`ALTER CONVERSION<alter-conversion>`
   * ALTER DATABASE
   * ALTER DOMAIN
   * ALTER EXTERNAL TABLE
   * ALTER FILESPACE
   * ALTER FOREIGN DATA WRAPPER*
   * ALTER FOREIGN TABLE*
   * ALTER FUNCTION
   * ALTER GROUP
   * ALTER INDEX
   * ALTER LANGUAGE
   * ALTER OPERATOR
   * ALTER OPERATOR CLASS
   * ALTER PROTOCOL
   * ALTER RESOURCE QUEUE
   * ALTER ROLE
   * ALTER SCHEMA
   * ALTER SEQUENCE
   * ALTER SERVER*
   * ALTER TABLE
   * ALTER TABLESPACE
   * ALTER TYPE
   * ALTER USER
   * ALTER USER MAPPING*
   * ANALYZE
   * BEGIN
   * CHECKPOINT
   * CLOSE
   * CLUSTER
   * COMMENT
   * COMMIT
   * COPY
   * CREATE AGGREGATE
   * CREATE CAST
   * CREATE CONVERSION
   * CREATE DATABASE
   * CREATE DOMAIN
   * CREATE EXTERNAL TABLE
   * CREATE FOREIGN DATA WRAPPER*
   * CREATE FOREIGN TABLE*
   * CREATE FUNCTION
   * CREATE GROUP
   * CREATE INDEX
   * CREATE LANGUAGE
   * CREATE OPERATOR
   * CREATE OPERATOR CLASS
   * CREATE RESOURCE QUEUE
   * CREATE ROLE
   * CREATE RULE
   * CREATE SCHEMA
   * CREATE SEQUENCE
   * CREATE SERVER*
   * CREATE TABLE
   * CREATE TABLE AS
   * CREATE TABLESPACE
   * CREATE TYPE
   * CREATE USER
   * CREATE USER MAPPING*
   * CREATE VIEW
   * DEALLOCATE
   * DECLARE
   * DELETE
   * DROP AGGREGATE
   * DROP CAST
   * DROP CONVERSION
   * DROP DATABASE
   * DROP DOMAIN
   * DROP EXTERNAL TABLE
   * DROP FILESPACE
   * DROP FOREIGN DATA WRAPPER*
   * DROP FOREIGN TABLE*
   * DROP FUNCTION
   * DROP GROUP
   * DROP INDEX
   * DROP LANGUAGE
   * DROP OPERATOR
   * DROP OPERATOR CLASS
   * DROP OWNED
   * DROP RESOURCE QUEUE
   * DROP ROLE
   * DROP RULE
   * DROP SCHEMA
   * DROP SEQUENCE
   * DROP SERVER*
   * DROP TABLE
   * DROP TABLESPACE
   * DROP TYPE
   * DROP USER
   * DROP USER MAPPING*
   * DROP VIEW
   * END
   * EXECUTE
   * EXPLAIN
   * FETCH
   * GRANT
   * INSERT
   * LOAD
   * LOCK
   * MOVE
   * PREPARE
   * REASSIGN OWNED
   * REINDEX
   * RELEASE SAVEPOINT
   * RESET
   * REVOKE
   * ROLLBACK
   * ROLLBACK TO SAVEPOINT
   * SAVEPOINT
   * SELECT
   * SELECT INTO
   * SET
   * SET ROLE
   * SET SESSION AUTHORIZATION
   * SET TRANSACTION
   * SHOW
   * START TRANSACTION
   * TRUNCATE
   * UPDATE
   * VACUUM
   * VALUES
   
Not implemented in 4.3
   
2.1 SQL语法概要
---------------

**ABORT**

中断当前事务。

ABORT [WORK | TRANSACTION]

详见 :ref:`ABORT <abort>`

**ALTER AGGREGATE**

改变聚集函数的定义。

ALTER AGGREGATE name ( type [ , ... ] ) RENAME TO new_name

ALTER AGGREGATE name ( type [ , ... ] ) OWNER TO new_owner

ALTER AGGREGATE name ( type [ , ... ] ) SET SCHEMA new_schema

详见 :ref:`ALTER AGGREGATE <alter-aggregate>`

**ALTER CONVERSION**

改变转换的定义。

ALTER CONVERSION name RENAME TO newname

ALTER CONVERSION name OWNER TO newowner

详见 :ref:`ALTER CONVERSION <alter-conversion>`


.. _abort:

2.2 ABORT
---------
   
   中断当前事务。
   
   **语法**
   
   ABORT [WORK | TRANSACTION]
   
   **描述**
   
   ABORT回退当前的事务，丢弃由当前事务引起的所有修改。这个命令和标准的SQL命令ROLLBACK完全相同，保留下来完全是历史原因。
   
   **参数**
   
   WORK
   
   TRANSACTION
   
   可选关键字，他们不起作用。
   
   **注解**
   
   请使用COMMIT去成功终止一个事务。
   
   当不在一个事务内部时，发出ABORT没有危害，但是会引出一条警告信息。
   
   **兼容性**
   
   这个命令是一个Greenplum数据库的扩展，由于历史原因而保留下来。ROLLBACK是和它相当的SQL标准命令。
   
   **参考**
   
   BEGIN, COMMIT, ROLLBACK

.. _alter-aggregate:
   
2.3 ALTER AGGREGATE
-------------------
   
   改变聚集函数的定义。
   
   **语法**
   
   ALTER AGGREGATE name ( type [ , ... ] ) RENAME TO new_name
   ALTER AGGREGATE name ( type [ , ... ] ) OWNER TO new_owner
   ALTER AGGREGATE name ( type [ , ... ] ) SET SCHEMA new_schema

   **描述**
   
   ALTER AGGREGATE 改变聚集函数的定义。
   
   在用ALTER AGGREGATE改变一个函数的定义之前，必须拥有这个函数。如果改变一个聚集函数的模式，必须在这个模式上有CREATE权限。如果改变函数的拥有者，必须直接或者间接地是新的拥有者中的一员，并且这个新的拥有者必须有这个聚集函数的模式的CREATE权限。（有了这些规定，当你删除然后重建聚集函数做不了的事情，通过修改聚集函数拥有者也做不了。然而，超级用户无论如何都可以修改一个聚集函数的拥有者。）
   
   **参数**
   
   name
   
   已经存在的聚集函数的名字，可以用模式限定。
   
   type 
   
   聚集函数在其上操作的一个输入数据类型。如果想引用一个没有参数的聚集函数，用*来替换输入数据类型列表。
   
   new_name
   
   聚集函数的新名字。
   
   new_owner
   
   聚集函数的新所有者。
   
   new_schema
   
   聚集函数的新模式。

   **示例**
   
   重命名一个integer类型的聚集函数myavg为my_average:

   ALTER AGGREGATE myavg(integer) RENAME TO my_average;

   把integer类型的聚集函数myavg的所有者修改为joe：

   ALTER AGGREGATE myavg(integer) OWNER TO joe;
   
   把integer类型的聚集函数myavg移动到myschema模式下:

   ALTER AGGREGATE myavg(integer) SET SCHEMA myschema;

   **兼容性**
   
   SQL标准中没有ALTER AGGREGATE语句。
   
   **参考**
   
   CREATE AGGREGAT, DROP AGGREGATE
   
.. _alter-conversion:

2.4 ALTER CONVERSION
--------------------

   修改转换的定义。

   **语法**
   
   ALTER CONVERSION name RENAME TO newname
   
   ALTER CONVERSION name OWNER TO newowner
   
   **描述**
   
   ALTER CONVERSION用来改变一个转换的定义。
   
   只有转换的拥有者才能修改转换。只有新的拥有者中的一员，才能修改拥有者为新的拥有者，这个新的拥有者还得拥有这个转换所在模式的CREATE权限。（有了这些规定，当你删除然后重建转换做不了的事情，通过修改转换的拥有者也做不了。然而，超级用户无论如何都可以修改一个转换的拥有者。）

   **参数**
   
   Name
   
   要修改的转换的名字，可以用模式限定。
 
   new_name
   
   转换的新名字。

   Newowner
   
   转换的新所有者。
   

   **示例**
   
   修改转换iso_8859_1_to_utf8的名字为latin1_to_unicode:

   ALTER CONVERSION iso_8859_1_to_utf8 RENAME TO latin1_to_unicode;

   将iso_8859_1_to_utf8的拥有者修改为joe：

   ALTER CONVERSION iso_8859_1_to_utf8 OWNER TO joe;

   **兼容性**
   
   标准SQL中不存在ALTER CONVERSION语句。

   **参考**
   
   CREATE CONVERSION, DROP CONVERSION
   
2.5 ALTER DATABASE
-------------------

   改变数据库的属性。
   
   **语法**
   
   ALTER DATABASE name [ WITH CONNECTION LIMIT connlimit ]

   ALTER DATABASE name SET parameter { TO | = } { value | DEFAULT }

   ALTER DATABASE name RESET parameter

   ALTER DATABASE name RENAME TO newname

   ALTER DATABASE name OWNER TO new_owner
   
   **描述**
   
   ALTER DATABASE 改变数据库的属性。

   第一种格式是改变允许连接到数据库的连接数。只有数据库的所有者或者超级用户才能改变这个设置。

   第二种和第三种格式改变Greenplum数据库配置参数的会话默认值。接下来无论何时在这个数据库中开启一个新的会话，新指定的值都会成为会话的默认值。数据库相关的默认值会覆盖服务器配置文件中的设置（postgresql.conf）。只有数据库的所有者或者超级用户才能改变数据库的会话默认值。某些参数不能通过这种方式设置，或者只能由超级用户来设置。

   第四种格式改变数据库的名称。只有数据库的所有者或者超级用户才能重命名数据库。非超级用户有CREATEDB 权限。不能重命名当前的数据库。请先连接另外一个数据库。

   第五种格式修改数据库的所有者。如果想修改所有者，你必须是数据库的所有者或者是数据库所有者中的直接成员或者间接成员，并且你必须有CREATEDB权限。（注意超级用户自动拥有所有这些权限）

   **参数**
   
   name

   要修改属性的数据库的名称。

   connlimit

   并发连接的最大连接数。默认值是-1，表示没有不限连接数。

   Parameter value

   设置数据库指定配置参数的会话默认值为给定的值。如果这个值是DEFAULT或者使用RESET，数据库特有的知识就会删除，因此系统范围的默认设置就会被新会话继承。使用RESET ALL可以清除所有的数据库特有的设置。关于服务器参数和所有的用户可设置的配置参数详见服务器配置参数。

   newname

   数据库的新名称。

   new_owner

   数据库新的所有者。
   
   **注解**
   
   除了给数据库的配置参数设置会话默认值外，还可以给某一个特定的角色（用户）的配置参数设置会话默认值。如果参数值有冲突，角色相关的设置会覆盖数据库相关的设置。参见ALTER ROLE。

   **示例**
   
   查看mydatabase数据库的默认模式搜过路径：

   ALTER DATABASE mydatabase SET search_path TO myschema,public, g_catalog;
   
   **兼容性**
   
   ALTER DATABASE语句是Greenplum数据库的扩展。

   **参见**
   
   CREATE DATABASE, DROP DATABASE, SET
   
2.6 ALTER DOMAIN
---------------------

   改变域的定义。

   **语法**

   ALTER DOMAIN name { SET DEFAULT expression | DROP DEFAULT }

   ALTER DOMAIN name { SET | DROP } NOT NULL

   ALTER DOMAIN name ADD domain_constraint
 
   ALTER DOMAIN name DROP CONSTRAINT constraint_name [RESTRICT | CASCADE]

   ALTER DOMAIN name OWNER TO new_owner

   ALTER DOMAIN name SET SCHEMA new_schema

   **描述**

   ALTER DOMAIN改变已有域的定义。有几种子格式：

   * SET/DROP DEFAULT——这种格式设置或者删除域的默认值。注意默认值只对紧接着的INSERT    命令有效。他们不会影响表中已存在使用的此域的行。

   * SET/DROP NOT NULL——这种格式设置域是允许还是拒绝NULL值。只有当使用此域的列不含有NULL值的时候，才可以设置为SET NOT NULL。

   * ADD domain_constraint——这种格式使用和CREATE DOMAIN类似的语法为一个域增加新的约束。只有当所有使用此域的列都满足新的约束的时候，这种格式才会成功。

   * DROP CONSTRAINT——这种格式删除域上的约束。

   * OWNER——这种格式将一个域的所有者改变为指定的用户。

   * SET SCHEMA——这种格式改变域的模式。域上的所有约束都会同时移动到新的模式中去。

   只有域的拥有者才能使用ALTER DOMAIN。想要改变域的模式，必须在新的模式上有CREATE权限。如果改变域的所有者，必须是所有者中的直接成员或者间接成员，而且这个角色必须在域的模式上有CREATE权限。（有了这些规定，当你删除然后重建域做不了的事情，通过修改域拥有者也做不了。然而，超级用户无论如何都可以修改一个域的拥有者。）

   **参数**

   Name

   已有域的名称，可以用模式限定。

   domain_constraint

   新的域约束。

   constraint_name

   要删除的约束的名称。

   CASCADE

   自动删除依赖此约束的对象。

   RESTRICT

   如果有任何对象依赖此约束，则拒绝删除此约束。这是默认行为。

   New_owner

   域的新所有者的名称。

   New_schema

   域的新模式。

   **示例**

   为域添加NOT NULL约束：

   ALTER DOMAIN zipcode SET NOT NULL;

   删除域上的NOT NULL：

   ALTER DOMAIN zipcode DROP NOT NULL;

   为域增加一个检查约束：

   ALTER DOMAIN zipcode ADD CONSTRAINT zipchk CHECK (char_length (VALUE) = 5);

   删除域上的检查约束：

   ALTER DOMAIN zipcode DROP CONSTRAINT zipchk;

   移动域到另一个模式中：

   ALTER DOMAIN zipcode SET SCHEMA customers;

   **兼容性**

   ALTER DOMAIN符合SQL标准，除了OWNER和SET SCHEMA变体是Greenplum的扩展。

   **参见**

   CREATE DOMAIN, DROP DOMAIN

2.7 ALTER EXTERNAL TABLE
------------------------
   
   改变外部表的定义。

   **语法**
   
   ALTER EXTERNAL TABLE name RENAME [COLUMN] column TO new_column

   ALTER EXTERNAL TABLE name RENAME TO new_name

   ALTER EXTERNAL TABLE name SET SCHEMA new_schema

   ALTER EXTERNAL TABLE name action [, ... ]

   action如下：

   ADD [COLUMN] column_name type

   DROP [COLUMN] column

   ALTER [COLUMN] column

   TYPE type [USING expression]

   OWNER TO new_owner

   **描述**

   ALTER EXTERNAL TABLE改变已有的外部表定义。有如下几个子格式：

   * ADD COLUMN——为外部表添加个新列。

   * DROP COLUMN——从外部表中删除一列。注意如果你删除一个可读的外部表的列，这个命令只会改变Greenplum数据库中的表定义。外部表的文件是不会改变的。

   * ALTER COLUMN TYPE——改变列的数据类型。可选子句USING指定了从列的旧值计算新值的方法。如果忽略，默认的转换和旧数据类型向新数据类型赋值转换一样。如果没有隐含的或者旧数据类型向新数据类型赋值转换，必须提供USING子句。

   * OWNER——将外部表的所有者修改为指定的用户。

   * OWNER——改变外部表的名称，或者改变外部表中某一列的名称。这个操作对外部表的数据没有影响。

   * SET SCHEMA——把外部表移动到另一个模式中去。

   想要使用ALTER EXTERNAL TABLE必须拥有此外部表。想改变外部表的模式，必须在新模式上有CREATE权限。想改变所有者，你必须是外部表所有者中的直接成员或者间接成员，而且这个角色必须在外部表的模式上有CREATE权限。超级用户自动拥有这些权限。

   在这个发布中，ALTER EXTERNAL TABLE不能修改外部表的类型、数据格式和外部表的数据的位置。如果想修改这些，你必须删除然后重建外部表的定义。

   **参数**

   name

   要修改的外部表的名称，可以用模式限定。

   column

   新的或者已有的列的名称。

   new_column

   已有列的名称。

   new_name

   外部表的新名称。

   type

   新列的数据类型，或者已有列的新数据类型。

   new_owner

   外部表新所有者的角色名。

   new_schema

   即将移入外部表的模式的名称。

   **示例**

   为外部表的定义增加新列：

   ALTER EXTERNAL TABLE ext_expenses ADD COLUMN manager text;

   修改外部表的名字：

   ALTER EXTERNAL TABLE ext_data RENAME TO ext_sales_data;

   改变外部表的所有者：

   ALTER EXTERNAL TABLE ext_data OWNER TO jojo;

   改变外部表的模式：

   ALTER EXTERNAL TABLE ext_leads SET SCHEMA marketing;

   **兼容性**
   
   ALTER EXTERNAL TABLE是Greenplum的扩展。在SQL标准和PostgreSQL中没有ALTER EXTERNAL TABLE。

   **参见**

   CREATE EXTERNAL TABLE, DROP EXTERNAL TALBE
   
2.8 ALTER FILESPACE
-------------------
   
   该表表空间的定义。

   **语法**

   ALTER FILESPACE name RENAME TO newname

   ALTER FILESPACE name OWNER TO newowner

   **描述**

   ALTER FILESPACE改变表空间的定义。

   只有表空间的所有者才能使用ALTER FILESPACE。想改变所有者，你必须是外部表所有者中的直接成员或者间接成员，超级用户自动拥有这些权限。

   **参数**

   Name

   已有表空间的名字。

   newname

   表空间的新名字。新名字不能以pg_或者gp_开头（pg_和gp_预留给系统表空间）。

   newowner

   表空间的新所有者。

   **示例**

   重命名myfsw为fast_ssd：

   ALTER FILESPACE myfs RENAME TO fast_ssd;

   改变myfs表空间的所有者：

   ALTER FILESPACE myfs OWNER TO dba;

   **兼容性**

   在SQL标准和PostgreSQL中没有ALTER FILESPACE。

   **参见**

   DROP FILESPACE，Greenplum数据库工具指南中的gpfilespace

2.9 ALTER FUNCTION
------------------

   改变函数的定义。

   **语法**

   ALTER FUNCTION name ( [ [argmode] [argname] argtype [, ...] ] ) action [, ... ] [RESTRICT]

   ALTER FUNCTION name ( [ [argmode] [argname] argtype [, ...] ] ) RENAME TO new_name

   ALTER FUNCTION name ( [ [argmode] [argname] argtype [, ...] ] ) OWNER TO new_owner

   ALTER FUNCTION name ( [ [argmode] [argname] argtype [, ...] ] ) SET SCHEMA new_schema

   action如下：

   {CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT | STRICT}

   {IMMUTABLE | STABLE | VOLATILE}

   {[EXTERNAL] SECURITY INVOKER | [EXTERNAL] SECURITY DEFINER}

   **描述**

   ALTER FUNCTION改变函数的定义。

   只有函数的所有者才能使用ALTER FUNCTION。如果想改变函数模式，你必须在新的模式上有CREATE权限。想改变所有者，你必须是函数所有者中的直接成员或者间接成员，而且这个角色必须在外部表的模式上有CREATE权限。超级用户自动拥有这些权限。（有了这些规定，当你删除然后重建函数做不了的事情，通过修改函数拥有者也做不了。然而，超级用户无论如何都可以修改一个函数的拥有者。）

   **参数**

   name

   已有函数的名称，可以使用模式限定。

   argmode

   参数的模式：IN、OUT或者INOUT。如果忽略，默认值是IN。注意，实际上，ALTER FUNCTION不会注意OUT参数，因为在决定函数的身份的时候只需要输入参数。因此列出IN和OUT参数就够了。

   argname

   参数的名字。注意，事实上，ALTER FUNCTION并不注意参数的名字，因为只有在决定函数的身份的时候，只需要参数的类型。

   argtype

   函数参数的数据类型，可以用模式限定。

   new_name

   函数的新名字。

   new_owner

   函数的新所有者。注意，如果函数被标记为SECURITY DEFINER，它接下来会以新的所有者执行。

   new_schema

   函数的新模式。

   CALLED ON NULL INPUT

   RETURNS NULL ON NULL INPUT

   STRICT

   CALLED ON NULL INPUT
   
   使得函数在部分或者全部参数为NULL的时候可以被调用。RETURNS NULL ON NULL INPUT或者STRICT使得函数在任何参数为空的时候都不可以被调用。相反，NULL返回值则可以自动假定的。详见CREATE FUNCTION。

   IMMUTABLE

   STABLE

   VOLATILE

   设置函数的属性为指定的设置。详见CREATE FUNCTION。

   [ EXTERNAL ] SECURITY INVOKER

   [ EXTERNAL ] SECURITY DEFINER

   设置函数是否为安全定义者。为了符合SQL标准EXTERNAL被忽略了。关于这个功能请参考CREATE FUNCTION以获取更多信息。

   RESTRICT

   为了符合SQL标准，忽略该参数。

   **注解**

   Greenplum数据库对定义为STABLE或者VOLATILE的函数有限制。详见CREATE FUNCTION。

   **示例**

   重命名integer类型的函数sqrt为square_root：

   ALTER FUNCTION sqrt(integer) RENAME TO square_root;

   改变integer类型的函数sqrt的所有者为joe：

   ALTER FUNCTION sqrt(integer) OWNER TO joe;

   改变sqrt函数的模式为math：

   ALTER FUNCTION sqrt(integer) SET SCHEMA math;

   **兼容性**

   这个语句和SQL标准中的ALTER FUNCTION部分兼容。标准中允许要修改的函数有更多的属性，但是没有提供重命名函数、定义函数为安装定义者或者修改所有者、模式、或者函数属性等功能。标准需要RESTRICT关键字，而这个关键字在Greenplum中是可选的。

   **参见**

   CREATE FUNCTION, DROP FUNCTION

2.10 ALTER GROUP
----------------
   
   改变角色名或者成员关系。

   **语法**

   ALTER GROUP groupname ADD USER username [, ... ]

   ALTER GROUP groupname DROP USER username [, ... ]

   ALTER GROUP groupname RENAME TO newname

   **描述**

   ALTER GROUP是一个过时的命令，保留是为了向后兼容。组（和用户）已经被更加一般的概念角色所替代。详见ALTER ROLE。

   **参数**

   Groupname

   将要修改的组（角色）的名字

   Username

   将要添加到组或者从组里删除的用户（角色），这个用户（角色）必须是已有的。

   newname

   组（角色）的新名字。

   **示例**

   添加用户到一个组：

   ALTER GROUP staff ADD USER karl, john;

   从组里删除一个用户：

   ALTER GROUP workers DROP USER beth;

   **兼容性**

   SQL标准中没有ALTER GROUP。

   **参见**

   ALTER ROLE, GRANT, REVOKE

2.11 ALTER INDEX
----------------

   改变索引的定义。

   **语法**

   ALTER INDEX name RENAME TO new_name

   ALTER INDEX name SET TABLESPACE tablespace_name

   ALTER INDEX name SET ( FILLFACTOR = value )

   ALTER INDEX name RESET ( FILLFACTOR )

   **描述**

   ALTER INDEX改变已有索引的定义。有几种子格式：

   * RENAME——改变索引的名字，这对存储的数据没有影响。

   * SET TABLESPACE——改变索引的表空间到指定的表空间，将关联到索引的数据文件移动到新的表空间中。参见CREATE TABLESPACE。

   * SET FILLFACTOR——改变索引的索引方法特定的存储参数。内置的索引方法都接受单一参数：FILEFACTOR。索引的装填因子是一个百分比，它决定了索引方法压缩索引页的程度。索引的内容不会被这个命令立即修改。使用REINDEX重建索引以获得想要的效果。

   * RESET FILLFACTOR——用默认值重置FILLFACTOR。和SET一样，需要使用REINDEX更新整个索引。

   **参数**

   name

   将要修改的已有索引的名字，可用模式限定。

   new_name

   索引的新名字。

   tablespace_name

   索引的目标表空间。

   FILLFACTOR

   索引的装填因子是一个百分比，它决定了索引方法压缩索引页的程度。对B树索引来说，叶子页在初始创建索引的时候就用这个百分比填好了，当从右边扩展索引的时候也会用这个百分比填好的。如果叶子页接下来完全满了，这会导致索引的效率下降，所以它们就会被分拆。

   B树索引的默认装填因子是90，但是它值可以是10到100的任意值。如果表是静态的，那么装填因子最好设为100，以便让索引的物理大小达到最小。但是对于一个经常更新的表，更小的装填因子更好，因为这样可以减少索引的拆分。另外一个索引方法用另一种不同但大体类似的方法使用装填因子。不同方法的默认装填因子不同。

   **注解**

   这些操作也可以用ALTER TABLE来完成。

   不允许改变系统分类索引。

   **示例**

   重命名现有索引：

   ALTER INDEX distributors RENAME TO suppliers;

   移动索引到另外一个表空间：

   ALTER INDEX distributors SET TABLESPACE fasttablespace;

   改变索引的装填因子（假设这个索引方法支持装填因子）：

   ALTER INDEX distributors SET (fillfactor = 75);

   REINDEX INDEX distributors;

   **兼容性**

   ALTER INDEX是一个Greenplum数据库的扩展。

   **参见**

   CREATE INDEX, REINDEX, ALTER TABLE

2.12 ALTER LANGUAGE
-------------------

   改变过程语言的名字。

   **语法**

   ALTER LANGUAGE name RENAME TO newname

   **描述**

   ALTER LANGUAGE改变过程语言的名字，只有超级用户可以重命名一个语言。

   **参数**

   Name

   语言的名字。

   newname

   语言的新名字。

   **兼容性**

   SQL标准中没有ALTER LANGUAGE。

   **参见**

   CREATE LANGUAGE, DROP LANGUAGE

2.13 ALTER OPERATOR
-------------------

   改变运算符的定义。

   **语法**

   ALTER OPERATOR name ( {lefttype | NONE} , {righttype | NONE} )

   OWNER TO newowner

   **描述**

   ALTER OPERATOR改变运算符的定义。目前仅支持改变运算符的所有者。

   你必须是运算符的所有者，才能使用ALTER OPERATOR。要改变所有者，你必须是新所有者的直接成员或者间接成员，而且这个角色还必须在运算符的模式上有CREATE权限。（有了这些规定，当你删除然后重建运算符做不了的事情，通过修改运算符拥有者也做不了。然而，超级用户无论如何都可以修改一个运算符的拥有者。）

   **参数**

   name

   已有运算符的名字，可用模式限定。

   lefttype

   运算符左边操作数的数据类型；如果运算符没有左操作数，写NONE。

   righttype

   运算符右边操作数的数据类型；如果运算符没有右操作数，写NONE。

   newowner

   运算符的新所有者。

   **示例**

   改变一个自定义的text型的运算符a @@ b的所有者：

   ALTER OPERATOR @@ (text, text) OWNER TO joe;

   **兼容性**

   标准SQL中没有ALTER OPERATOR。

   **参见**

   CREATE OPERATOR，DROP OPERATOR
   
2.14 ALTER OPERATOR CLASS
-------------------------

   改变运算符类的定义。

   **语法**

   ALTER OPERATOR CLASS name USING index_method RENAME TO newname

   ALTER OPERATOR CLASS name USING index_method OWNER TO newowner

   **描述**

   ALTER OPERATOR CLASS改变运算符类的定义。

   你必须是运算符类的所有者，才能使用ALTER OPERATOR CLASS。要改变所有者，你必须是新所有者的直接成员或者间接成员，而且这个角色还必须在运算符类的模式上有CREATE权限。（有了这些规定，当你删除然后重建运算符类做不了的事情，通过修改运算符类拥有者也做不了。然而，超级用户无论如何都可以修改一个运算符类的拥有者。）

   **参数**

   name

   已有运算符类的名字，可以用模式名限定。

   index_method

   运算符类所属的索引方法的名称。

   newname

   运算符类的新名称。

   newowner

   运算符类的新所有者。

   **兼容性**

   标准SQL中没有ALTER OPERATOR CLASS。

   **参见**

   CREATE OPERATOR CLASS, DROP OPERATOR CLASS
   
2.15 ALTER PROTOCAL
-------------------

   改变协议的定义。

   **语法**

   ALTER PROTOCOL name RENAME TO newname

   ALTER PROTOCOL name OWNER TO newowner

   **描述**

   ALTER PROTOCOL改变协议的定义。只能改变协议的名称和所有者。要改变所有者，你必须是新所有者的直接成员或者间接成员，而且这个角色还必须在协议的模式上有CREATE权限。

   **参数**

   name

   已有协议的名称，可以用模式名限定。

   newname

   协议的新名字。

   newowner

   协议的新所有者。

   **示例**

   重命名协议GPDBauth 为GPDB_authentication：

   ALTER PROTOCOL GPDB_auth RENAME TO GPDB_authentication;

   修改GPDB_authentication的所有者为joe：

   ALTER PROTOCOL GPDB_authentication OWNER TO joe;

   **兼容性**

   SQL标准中没有ALTER PROTOCOL语句。
   
2.16 ALTER RESOURCE QUEUE
---------------------------

   改变资源队列的限制。
   
   **语法**
   
   ALTER RESOURCE QUEUE name WITH ( queue_attribute=value [, ... ] )

   这里的queue_attribute是：

   ACTIVE_STATEMENTS=integer

   MEMORY_LIMIT='memory_units'

   MAX_COST=float

   COST_OVERCOMMIT={TRUE|FALSE}

   MIN_COST=float

   PRIORITY={MIN|LOW|MEDIUM|HIGH|MAX}

   ALTER RESOURCE QUEUE name WITHOUT ( queue_attribute [, ... ] )

   这里的queue_attribute是：

   ACTIVE_STATEMENTS

   MEMORY_LIMIT

   MAX_COST

   MIN_COST

   注意：资源队列必须有ACTIVE_STATEMENTS和MAX_COST两者之一。不要同时从一个资源队列中将这两个队列属性移除。
   
   **描述**

   ALTER RESOURCE QUEUE修改资源队列的限制。只有超级用户可以修改资源队列。资源队列必须有ACTIVE_STATEMENTS和MAX_COST两者之一（或者两者都有）。你也可以设置或重置资源队列的优先级，以便控制与此队列绑定的查询占可用CPU资源的相对份额，或者也可以设置或重置资源队列的内存限制，以便控制一个Segment主机上过这个队列提交的所有查询可用的内存总量。

   ALTER RESOURCE QUEUE WITHOUT删除之前设置在一个资源上的指定的限制。资源队列必须有ACTIVE_STATEMENTS和MAX_COST两者之一。不要同时从一个资源队列中将这两个队列属性移除。

   **参数**

   name

   将要改变限制的资源队列的名字。

   ACTIVE_STATEMENTS integer

   任一时刻系统允许的在这个资源队列中的用户提交的活动语句得数量。ACTIVE_STATEMENTS的值应该是一个大于0的整数。输入-1可以重置ACTIVE_STATEMENTS到无限制。

   MEMORY_LIMIT ‘memory_units’

   设置这个资源队列中的用户提交的所有语句的内存配额。内存单位可以指定为kB、MB、GB。一个资源队列的最小内存配额是10MB。没有最大值；但是查询执行期间的内存上界受限于一个Segment主机的物理内存。默认值是-1，没有限制。

   MAX_COST float

   任一时刻系统允许在这个资源队列上的用户提交的语句总的查询优化成本。MAX_COST的值可以指定为一个浮点数（比如100.0）或者也可以指定为一个科学计数法表示的数（比如1e+2）。输入-1.0，可以重置MAX_COST为无限制。

   COST_OVERCOMMIT boolean

   如果一个资源队列是基于查询成本而限制的，那么管理员可以允许COST_OVERCOMMIT（COST_OVERCOMMIT=TRUE， 默认值）。这意味着如果一个查询超过了允许的成本阈值那么它只允许它在系统空闲的时候运行。如果指定COST_OVERCOMMIT=FALSE，超过代价限制的查询会被拒绝，决不允许运行。

   MIN_COST float

   在这个代价限制之下的查询不用排队，可以直接运行。代价是以预取的磁盘页单元来衡量的；1.0等于一个顺序读取的磁盘页。MIN_COST的值可以指定为一个浮点数（比如100.0）或者也可以指定为一个科学计数法表示的数（比如1e+2）。输入-1.0，可以重置MIN_COST为无限制。

   PRIORITY={MIN|LOW|MEDIUM|HIGH|MAX}

   设置资源队列里的查询的优先级。队列中的高优先级的语句或者查询在争用资源的时候可以获得较大份额的可用CPU资源。当高优先级的查询在执行的时候，低优先级的查询会被延迟。

   **注解**

   加入角色（用户）到资源队列，请使用CREATE ROLE或者ALTER ROLE。

   **示例**

   改变资源队列的活跃查询限制：

   ALTER RESOURCE QUEUE myqueue WITH (ACTIVE_STATEMENTS=20);

   改变资源队列的内存限制：

   ALTER RESOURCE QUEUE myqueue WITH （MEMORY_LIMIT=’2GB’）;

   重置资源队列的最大和最小查询代价为无限制：

   ALTER RESOURCE QUEUE myqueue WITH (MAX_COST=-1.0, MIN_COST=-1.0);

   重置查询代价限制到310（或者30000000000.0），并且不允许过量使用：

   ALTER RESOURCE QUEUE myqueue WITH (MAX_COST=3e+10,COST_OVERCOMMIT=FALSE);

   重置查询队列的查询优先级到最小水平：

   ALTER RESOURCE QUEUE myqueue WITH (PRIORITY=MIN);

   取消资源队列中的MAX_COST和MIN_COST限制：

   ALTER RESOURCE QUEUE myqueue WITHOUT (MAX_COST, MEMORY_LIMIT);

   **兼容性**

   ALTER RESOURCE QUEUE语句是Greenplum数据库的扩展。标准PostgreSQL中没有这个命令。

   **参见**

   CREATE RESOURCE QUEUE, DROP RESOURCE QUEUE, CREATE ROLE, ALTER ROLE

2.17 ALTER ROLE
-----------------
   
   改变数据库角色（用户或组）。

   **语法**

   ALTER ROLE name RENAME TO newname

   ALTER ROLE name SET config_parameter {TO | =} {value | DEFAULT}

   ALTER ROLE name RESET config_parameter

   ALTER ROLE name RESOURCE QUEUE {queue_name | NONE}

   ALTER ROLE name [ [WITH] option [ ... ] ]

   这里的option是：

   SUPERUSER | NOSUPERUSER

   | CREATEDB | NOCREATEDB

   | CREATEROLE | NOCREATEROLE

   | CREATEEXTTABLE | NOCREATEEXTTABLE

   [ ( attribute='value'[, ...] ) ]

   where attributes and value are:

   type='readable'|'writable'

   protocol='gpfdist'|'http'

   | INHERIT | NOINHERIT

   | LOGIN | NOLOGIN

   | CONNECTION LIMIT connlimit

   | [ENCRYPTED | UNENCRYPTED] PASSWORD 'password'

   | VALID UNTIL 'timestamp'

   | [ DENY deny_point ]

   | [ DENY BETWEEN deny_point AND deny_point]

   | [ DROP DENY FOR deny_point ]

   **描述**

   ALTER ROLE修改Greenplum数据库角色的属性。这个命令有几个变体：

   * RENAME——修改角色的名字。数据库超级用户可以重命名任何角色。有CREATEROLE权限的角色可以重命名非超级用户角色。当前会话用户不能被重命名（要重命名一个角色请用其他角色登陆）。因为MD5加密的密码使用角色名为密码盐，如果密码是MD5加密的，那么重命名角色将会清除它的密码。

   * SET | RESET——修改特定配置参数的角色会话默认值。无论这个角色接下来何时开启一个新的会话，指定的值都会成为会话的默认值，这会覆盖服务器配置文件（postgresql.conf）中设置。对没有LOGIN权限的角色，会话默认值没有影响。普通角色可以改变自己的会话默认值。超级用户可以修改任何人的会话默认值。拥有CREATEROLE权限的角色可以改变非超级用户角色的默认值。关于所有用户可以设置的配置参数，参见Greenplum Database Administrator Guide。

   * RESOURCE QUEUE——把角色指定给一个工作负载管理资源队列。这个用户在发出查询的时候会受限于资源队列的限制。指定NONE可以把角色赋给默认资源队列。一个角色只能属于一个资源队列。对于一个没有LOGIN权限的角色，资源队列没有限制。详见CREATE RESOURCE QUEUE。

   * WITH option——修改很多可以在CREATE ROLE中指定的角色属性。这个命令中没有提到的属性会保留之前的设置。数据库超级用户可以为任何角色做任何设置。拥有CREATEROLE权限的角色可以为非超级用户角色修改这些设置中的任何一个。普通用户仅仅只能修改自己的密码。

   **参数**

   name

   将要修改数据的角色的名字。

   newname

   角色的新名字。

   config_parameter=value

   设置一个用户针对特定配置参数的会话默认值为给定的值。如果这个值是DEFAULT或者使用RESET，特定于角色的变量设置就会被移除，因此这个角色会在新的会话中继承系统默认设置。使用RESET ALL可以清除所有的角色相关设置。关于用户可设置的配置参数，详见SET和服务器配置参数。

   queue_name

   用户级别的角色将要赋给的资源队列的名字。只有拥有LOGIN权限的角色才可以赋给资源队列。如果要从一个资源队列取消角色或者把角色赋给默认的资源队列，请指定NONE。一个角色只能属于一个资源队列。

   SUPERUSER | NOSUPERUSER

   CREATEDB | NOCREATEDB

   CREATEROLE | NOCREATEROLE

   CREATEEXTTABLE | NOCREATEEXTTABLE [(attribute='value')]

   如果指定CREATEEXTTABLE，将要定义的角色就会被允许创建外部表。如果没有指定，那么默认的type是readable，默认的protocol是gpfdist。NOCREATEEXTTABLE（默认值）会取消角色创建外部表的能力。注意只有超级用户才能创建使用file或者execute协议的外部表

   INHERIT | NOINHERIT

   LOGIN | NOLOGIN

   CONNECTION LIMIT connlimit

   PASSWORD password

   ENCRYPTED | UNENCRYPTED

   VALID UNTIL 'timestamp'

   这些子句可以改变通常由CREATE ROLE设置的角色属性。

   DENY deny_point

   DENY BETWEEN deny_point AND deny_point

   DENY和DENY BETWEEN关键字设置基于时间的约束，这些约束在登陆的时候执行。DENY设置拒绝访问的日期或者日期和时间。DENY BETWEEN设置拒绝访问的时间间隔。这两者都使用如下格式的参数deny_point：

   DAY day [ TIME 'time' ]

   deny_point参数的两部分的格式如下：

   日期：

   {'Sunday' | 'Monday' | 'Tuesday' |'Wednesday' | 'Thursday' | 'Friday' |'Saturday' | 0-6 }

   时间：

   { 00-23 : 00-59 | 01-12 : 00-59 { AM | PM }}

   DENY BETWEEN子句使用连个deny_point参数。

   DENY BETWEEN deny_point AND deny_point

   关于基于时间的约束和示例的更多信息，详见《Greenplum数据库管理员指南》中的“管理角色和权限”。

   DROP DENY FOR deny_point

   DROP DENY FOR子句为角色移除基于时间的约束。它使用上面提到的deny_point参数。

   关于基于时间的约束和示例的更多信息，详见《Greenplum数据库管理员指南》中的“管理角色和权限”。

   **注解**

   使用GRANT和REVOKE添加和移除角色的成员关系。

   使用这个命令指定没有加密的密码的时候必须注意。这个密码将会以明文的形式发送给服务器，而且会被客户端的历史命令或者服务器端的日志记住。Psql命令行客户端有一个元命令\password，这个元命令可以很安全地修改角色的密码。

   也可以把一个会话默认值绑定到一个指定的数据库而不是一个角色。如果角色的设置和数据库的设置由冲突，那么角色的设置会覆盖数据库的设置。参见ALTER DATABASE。

   **示例**

   修改角色的密码：

   ALTER ROLE daria WITH PASSWORD 'passwd123';

   修改密码的过期日期：

   ALTER ROLE scott VALID UNTIL ‘May 4 12:00:00 2015 +1’;

   是一个密码永久有效：

   ALTER ROLE luke VALID UNTIL 'infinity';

   给一个角色创建其他角色和新数据库的能力：

   ALTER ROLE   joelle CREATEROLE CREATEDB;

   给角色的maintenance_work_mem参数一个非默认设置：

   ALTER ROLE admin SET maintenance_work_mem = 100000;

   把一个角色赋给一个资源队列：

   ALTER ROLE sammy RESOURCE QUEUE poweruser;

   给角色创建可写外部表的权限：

   ALTER ROLE load CREATEEXTTABLE (type=’writable’);

   限制角色在周日的登陆访问：

   ALTER ROLE user3 DENY DAY ‘Sunday’;

   移除限制角色在周日登陆访问的约束：

   ALTER ROLE user3 DROP DENY FOR DAY ‘Sunday’;

   **兼容性**

   ALTER ROLE语句是Greenplum数据库的扩展。

   **参见**

   CREATE ROLE, DROP ROLE, SET, CREATE RESOURCE QUEUE, GRANT, REVOKE

2.18 ALTER SCHEMA
------------------

   修改模式的定义。

   **语法**

   ALTER SCHEMA name RENAME TO newname

   ALTER SCHEMA name OWNER TO newowner

   **描述**

   ALTER SCHEMA修改模式的定义。

   你必须拥有模式才能使用ALTER SCHEMA。要想重命名一个模式，你必须在数据库上有CREATE权限。要修改所有者，你必须是所有者中的直接成员或者间接成员， 而且必须有数据库上的CREATE权限。注意超级用户自动拥有所有这些权限。

   **参数**

   name

   已有模式的名字。

   newname

   模式的新名字。这个新名字不能以pg_开头，因为这些名字是预留给系统模式的。

   newowner

   模式的新所有者。

   **兼容性**
  
   SQL标准中没有ALTER SCHEMA语句。

   **参见**

   CREATE SCHEMA, DROP SCHEMA

2.19 ALTER SEQUENCE
--------------------

   改变序列生成器的定义。

   **语法**

   ALTER SEQUENCE name [INCREMENT [ BY ] increment]

   [MINVALUE minvalue | NO MINVALUE]

   [MAXVALUE maxvalue | NO MAXVALUE]

   [RESTART [ WITH ] start]

   [CACHE cache] [[ NO ] CYCLE]

   [OWNED BY {table.column | NONE}]

   ALTER SEQUENCE name SET SCHEMA new_schema

   **描述**

   ALTER SEQUENCE修改已有的序列生成器的参数。如果在ALTER SEQUENCE命令中没有指定的参数会保留它们之前的设置。

   **参数**

   name

   要修改的序列的名字，可以用模式名限定。

   increment

   INCREMENT BY increment子句是可选的。一个正数可以产生一个升序的序列，负数可以产生一个降序的序列。如果不指定，那么就会的就会使用旧的增长值。

   minvalue

   NO MINVALUE

   可选子句MINVALUE minvalue用来决定序列能够产生的最小值。如果指定NO MINVALUE，升序和降序序列的默认最小值分别为1和-263-1。如果两者都不指定，则会保持当前的最小值。

   可选子句MAXVALUE maxvalue用来决定序列能够产生的最大值。如果指定NO MAXVALUE，升序和降序序列的默认最大值分别为263-1和-1。如果两者都不指定，则会保持当前的最大值。

   start

   可选子句RESTART WITH start改变序列的当前值。

   cache

   子句CACHE cache使得序列数能够提前分配并存储在内存中，以便快速访问。最小值是1（一次只能产生一个值，也就说，没有缓存）。如果不指定，则会保持旧的缓存值。

   CYCLE

   可选的CYCLE关键字用来在升序序列到达最大值或者降序序列达到最小值的时候使序列能够循环。如果达到极限，下一个产生的数将是相应的最小值或最大值。

   NO CYCLE

   如果指定NO CYCLE关键字，在序列到达最大值之后，任何对nextval的调用都会返回一个错误。如果CYCLE和NO CYCLE都不指定，将会保持旧的循环行为。

   OWNED BY table.column

   OWNED BY NONE

   OWNED BY选项使序列和一个特定表列联系起来，因此如果那一列（或者整个表）删除的时候，序列也会自动删除。如果指定，这个关联将会取代之前的任何关联。指定的表必须和序列有相同的所有者，并且在同一个模式中。指定OWNED BY NONE会移除任何已有的表列关联。

   new_schema

   序列的新模式。

   **注解**

   为了避免阻塞从同一个序列获取数据的并发事务，ALTER SEQUENCE对序列生成参数的影响从不回退；这些改变是立即生效，并且从不可逆。然后，OWNED BY和SET SCHEMA子句是普通分类升级，而且也是可以回退的。

   ALTER SEQUENCE在会话中不会立即影响nextval的结果，除了已经预先分配的序列值当前会话。在注意到序列生成参数改变之前，他们会用完所有缓存的值。当前会话会立即受到影响。

   一些ALTER TABLE的变体也会随着序列一起使用。比如，可以使用ALTER TABLE RENAME来重命名序列。

   **示例**

   在105重启名为serial的序列：

   ALTER SEQUENCE serial RESTART WITH 105;

   **兼容性**

   除了OWNED BY和SET SCHEMA子句是Greenplum数据库的扩展外，ALTER SEQUENCE符合SQL标准。

   **参见**

   CREATE SEQUENCE, DROP SEQUENCE, ALTER TABLE
   
2.20 ALTER TABLE
-----------------

   修改表的定义。

   **语法**

   ALTER TABLE [ONLY] name RENAME [COLUMN] column TO new_column

   ALTER TABLE name RENAME TO new_name

   ALTER TABLE name SET SCHEMA new_schema

   ALTER TABLE [ONLY] name SET

   DISTRIBUTED BY (column, [ ... ] )

   | DISTRIBUTED RANDOMLY

   | WITH (REORGANIZE=true|false)

   ALTER TABLE [ONLY] name action [, ... ]

   ALTER TABLE name

   [ ALTER PARTITION { partition_name | FOR (RANK(number))

   | FOR (value) } partition_action [...] ]

   partition_action

   这里的action是：

   ADD [COLUMN] column_name type

   [column_constraint [ ... ]]

   DROP [COLUMN] column [RESTRICT | CASCADE]

   ALTER [COLUMN] column TYPE type [USING expression]

   ALTER [COLUMN] column SET DEFAULT expression

   ALTER [COLUMN] column DROP DEFAULT

   ALTER [COLUMN] column { SET | DROP } NOT NULL

   ALTER [COLUMN] column SET STATISTICS integer

   ADD table_constraint

   DROP CONSTRAINT constraint_name [RESTRICT | CASCADE]

   DISABLE TRIGGER [trigger_name | ALL | USER]

   ENABLE TRIGGER [trigger_name | ALL | USER]

   CLUSTER ON index_name

   SET WITHOUT CLUSTER

   SET WITHOUT OIDS

   SET (FILLFACTOR = value)

   RESET (FILLFACTOR)

   INHERIT parent_table

   NO INHERIT parent_table

   OWNER TO new_owner

   SET TABLESPACE new_tablespace

   这里的partition_action是：

   ALTER DEFAULT PARTITION

   DROP DEFAULT PARTITION [IF EXISTS]

   DROP PARTITION [IF EXISTS] { partition_name |

   FOR (RANK(number)) | FOR (value) } [CASCADE]

   TRUNCATE DEFAULT PARTITION

   TRUNCATE PARTITION { partition_name | FOR (RANK(number)) |

   FOR (value) }

   RENAME DEFAULT PARTITION TO new_partition_name

   RENAME PARTITION { partition_name | FOR (RANK(number)) |

   FOR (value) } TO new_partition_name

   ADD DEFAULT PARTITION name [ ( subpartition_spec ) ]

   ADD PARTITION [partition_name] partition_element

   [ ( subpartition_spec ) ]

   EXCHANGE PARTITION { partition_name | FOR (RANK(number)) |

   FOR (value) } WITH TABLE table_name

   [ WITH | WITHOUT VALIDATION ]

   EXCHANGE DEFAULT PARTITION WITH TABLE table_name

   [ WITH | WITHOUT VALIDATION ]

   SET SUBPARTITION TEMPLATE (subpartition_spec)

   SPLIT DEFAULT PARTITION

   { AT (list_value)

   | START([datatype] range_value) [INCLUSIVE | EXCLUSIVE]

   END([datatype] range_value) [INCLUSIVE | EXCLUSIVE] }

   [ INTO ( PARTITION new_partition_name,

   PARTITION default_partition_name ) ]

   SPLIT PARTITION { partition_name | FOR (RANK(number)) |

   FOR (value) } AT (value)

   [ INTO (PARTITION partition_name, PARTITION partition_name)]

   这里的partition_element是：

   VALUES (list_value [,...] )

   | START ([datatype] 'start_value') [INCLUSIVE | EXCLUSIVE]

   [ END ([datatype] 'end_value') [INCLUSIVE | EXCLUSIVE] ]

   | END ([datatype] 'end_value') [INCLUSIVE | EXCLUSIVE]

   [ WITH ( partition_storage_parameter=value [, ... ] ) ]

   [ TABLESPACE tablespace ]

   这里的subpartition_spec是：

   subpartition_element [, ...]

   subpartition_element是：

   DEFAULT SUBPARTITION subpartition_name

   | [SUBPARTITION subpartition_name] VALUES (list_value [,...] )

   | [SUBPARTITION subpartition_name]

   START ([datatype] 'start_value') [INCLUSIVE | EXCLUSIVE]

   [ END ([datatype] 'end_value') [INCLUSIVE | EXCLUSIVE] ]

   [ EVERY ( [number | datatype] 'interval_value') ]

   | [SUBPARTITION subpartition_name]

   END ([datatype] 'end_value') [INCLUSIVE | EXCLUSIVE]

   [ EVERY ( [number | datatype] 'interval_value') ]

   [ WITH ( partition_storage_parameter=value [, ... ] ) ]

   [ TABLESPACE tablespace ]

   这里的storage_parameter是：

   APPENDONLY={TRUE|FALSE}

   BLOCKSIZE={8192-2097152}

   ORIENTATION={COLUMN|ROW}

   COMPRESSTYPE={ZLIB|QUICKLZ|RLE_TYPE|NONE}

   COMPRESSLEVEL={0-9}

   FILLFACTOR={10-100}

   OIDS[=TRUE|FALSE]

   **描述**

   ALTER TABLE修改已有的表的定义。有几种子格式：

   * ADD COLUMN——为表增加一个新的列，使用和CREATE TABLE相同的语法。

   * DROP COLUMN——从表中删除一列。注意如果你要删除的这一列已经被用作Greenplum数据库的分布式键了，这个表的分布式策略会被改为DISTRIBUTED RANDOMLY。与此列相关的索引和表约束将会自动删除。如果表之外有任何东西依赖于这一列（比如视图），你需要写上CASCADE。

   * ALTER COLUMN TYPE——修改表中某列的数据类型。注意你不能修改已经被用作分布式键或者分区键的列的数据类型。通过重新解析简单提供的表达式，与此列相关的索引和简单的表约束被转换去使用新的表列。可选的USING子句指定如何从旧值计算新的列值。如果省略，默认的转换和从旧类型向新类型的赋值转一样。如果两者类型之间没有赋值转换也没有隐式转换，那么必须提供USING子句。

   * SET/DROP DEFAULT——设置或移除列的默认值。默认值只会应用到接下来的INSERT命令中。他们不会影响表的已有列。也可以为视图创建默认值，如果那样，在视图的ON INSERT规则被应用之前， 默认值会被插入到视图上的语句中去。

   * SET/DROP NOT NULL——改变一个列是允许NULL值还是拒绝NULL值。如果列中有非NULL值，那么你只能使用SET NOT NULL。

   * SET STATISTICS——设置随后的ANALYZE 操作的列统计信息收集目标。这个目标可以设置为0到100范围的数，或者设置为-1，以便使用系统默认的统计目标(default_statistics_target)。 

   * ADD table_constraint——使用和CREATE TABLE相同的语法为表（而不仅仅是一个分区）增加一个新的新的约束。

   * DROP CONSTRAINT——删除表上的指定约束。

   * DISABLE/ENABLE TRIGGER——启用或关闭表上的触发器。一个关闭的触发器系统依然是知道的，但是当它的触发事件发生的时候并不会执行。对一个延迟触发器来说，是在事件发生的时候检查它是否是启用状态，而不是在触发器函数实际执行的时候。可以启用或关闭指定名字的单一触发器，或者表上的所有触发器，或者仅仅是用户创建的触发器。启用或关闭约束触发器需要超级用户权限。

   注意：Greenplum数据库不支持触发器。一般而言，由于Greenplum数据库的并行机制，触发器只有有限的功能。

   * CLUSTER/SET WITHOUT CLUSTER——为将来的CLUSTER操作选择或移除默认索引。它并不会实际重聚集表。注意因为CLUSTER执行时间太长，所以CLUSTER并不是Greenplum数据库推荐的重新物理整理表的方式。最好使用CREATE TABLE AS重建表，并且用索引列整理它。

   * SET WITHOUT OIDS——移除表中的OID系统列。注意一旦OID被移除，没有任何ALTER TABLE的变体可以恢复。

   * SET ( FILLFACTOR = value) / RESET (FILLFACTOR)——修改表的装填因子。装填因子是一个10到100之间的百分数。100（完全装填）是默认值。当指定一个小点的装填因子，INSERT操作就只会把表页装填到指定的百分数。每页余下的空间被预留下来供更新行用。这给了UPDATE一个把更新后的行放在原来页上机会，这比放在另外一个页上效率更高。对一个条目从来不会更新的表来说，完全装填是最好的选择，但是对更新频率很大的表而言，小一些的装填因子会更为合适。注意这个命令不会立即修改表的内容。你需要重新写表，才能得到想要的效果。

   * SET DISTRIBUTED——改变表的分布策略。哈希分布策略的修改会导致表的数据在资源紧张的磁盘上重新分布。

   * INHERIT parent_table / NO INHERIT parent_table——添加或移除一个指定父表的子表。对父表的查询会包含子表的记录。为了添加成子表，目标表必须包含父表的全部列（它还可以有额外的列）。列的数据类型必须匹配，如果父表有NOT NULL约束，那子表也必须有NOT NULL约束。子表中必须有和父表匹配的CHECK约束。

   * OWNER——修改表、序列或视图的所有者为指定的用户。

   * SET TABLESPACE——修改表的表空间为指定的表空间，同时移动表的数据文件到新表空间。如果表上索引，索引是不移动的；但是索引可以使用额外的SET TABLESPACE命令来单独移动。参见CREATE TABLESPACE。如果改变一个分区表的表空间，所有的子分区也会被移动到新的表空间中去。

   * RENAME——修改表（索引、序列或视图）的名字或者表中单个列的名字。这对表中的数据没有影响。注意Greenplum数据库的分布式键列不能重命名。

   * SET SCHEMA——移动表到另一个模式中。表列相关的索引、约束和序列都会被移动。

   * ALTER PARTITION | DROP PARTITION | RENAME PARTITION | TRUNCATE PARTITION | ADD PARTITION | SPLIT PARTITION | EXCHANGE PARTITION | SET SUBPARTITION TEMPLATE——修改分区表的结构。大多数情况下，修改子表分区必须走遍整个父表。

   注意：如果给一个有子分区编码的表增加新的分区，新分区会继承子分区的存储指令。关于压缩设置的优先顺序的更多信息，参见《Greenplum数据库管理员指南》的“使用压缩”。

   你必须是表的所有者，才能使用ALTER TABLE。要修改表的模式，你必须在新的模式上有CREATE权限。要修改所有者，你必须是所有者的直接或者间接成员，并且角色必须有表模式上的CREATE权限。超级用户自动有这些权限。

   注意：

   如果一个表有很多分区，有压缩，或者表的块大小很大，那么内存的使用会显著增加。如果与表管理的关系数量很大，这种状况会使得表上的操作使用更多的内存。比如，如果一个表是CO表，有很多列，每一列是一个关系。像ALTER TABLE ALTER COLUMN这样的操作会在表相关的缓存上打开所有的列。如果一个CO表有40列和100个分区，并且列是压缩的，块大小为2MB（系统因素为3），系统会尝试分配24GB，也就是（40 * 100）* （2 * 3）MB或24GB。

   **参数**

   ONLY

   只在指定表名上执行操作。如果不适用ONLY关键字，这个操作会在表上执行，也会在与此表相关的子分区表上执行。

   name

   要修改的表的名字（可用模式名限定）。如果指定ONLY，只有这个表被改变。如果ONLY没有被指定，这个表和所有的子表也会更新。

   注意：约束只能添加到整个表上，不能添加到一个分区上。因为这个限制，name参数只能是一个表名，不能是一个分区名。

   column

   列的名字。注意Greenplum数据库的分布式键列必须特别对待。改变或删除这些列会改变表的分布式策略。

   new_column

   新的列名。

   new_name

   表的新名字。

   type

   新列的数据类型或者已有列的新的数据类型。如果改变一个Greenplum分布式键列的数据类型，只能修改为兼容的类型（比如，text改为varchar是可以的，但是text改为int是不行的）。

   table_constraint

   表的新约束。注意外键约束目前Greenplum数据库并不支持。一个表也只能有一个唯一约束，并且Greenplum数据库的分布式键也必须是唯一的。

   constraint_name

   要删除的约束名。

   CASCADE

   自动删除依赖于要删除的列或者约束的对象（比如，参考某一列的视图）。

   RESTRICT

   如果有依赖的对象，则拒绝删除列或者约束。这是默认行为。

   trigger_name

   要启动或关闭的触发器的名称。注意Greenplum数据库不支持触发器。

   ALL

   启动或者关闭属于表的触发器，包括约束相关的触发器。这需要超级用户权限。

   USER

   启动或关闭所有属于表的用户自定义触发器。

   index_name

   表被标记为聚集的列名。注意因为执行时间过长，所以CLUSTER不是Greenplum中推荐的用来物理上重新整理表的方式。最好使用CREATE TABLE AS重建表，并用索引列来整理它。

   FILLFACTOR

   设置表的装填因子百分比。

   value

   FILLFACTOR参数的新值，它是一个10到100的百分比，100是默认值。

   DISTRIBUTED BY (column) | DISTRIBUTED RANDOMLY

   指定表的分布式策略。哈希分布策略的修改会导致表的数据在资源紧张的磁盘上重新分布。如果你声明相同的哈希分布策略或者修改哈希策略为随机分布，数据不会重分布，除非你声明SET WITH(REORGANIZE=true)。

   REORGANIZE=true|false

   当哈希分布策略没有改变，或者从哈希改为随机分布的时候，可以使用REORGANIZE=true强制重分布数据。

   parent_table

   要和表关联或者解除关联的父表

   new_owner

   表的新所有者的角色名。

   new_tablespace

   表将要移去的表空间的名字。

   new_schema

   表要移去的目标模式名。

   parent_table_name

   修改分区表时，最高层的父表的名字。

   ALTER [DEFAULT] PARTITION

   如果修改一个比第一级分区更深的分区，ALTER PARTITION子句用来指定想要修改的子分区。

   DROP [DEFAULT] PARTITION

   删除一个指定的分区。如果这个分区有子分区，子分区也会自动删除。

   TRUNCATE [DEFAULT] PARTITION

   截短指定的分区。如果这个分区有子分区，子分区也会自动截短。

   RENAME [DEFAULT] PARTITION

   修改分区的名字（而不是关系的名字）。分区表使用名字约定来创建：<parentname>_<level>_prt_<partition_name>。

   ADD DEFAULT PARTITION

   增加一个默认的分区到已有的分区设计中。如果有数据和已有的分区不匹配，那么数据会被插入到默认分区中。没有默认分区的分区设计会拒绝一个不匹配已有分区的行的插入。默认分区必须有一个名字。

   ADD PARTITION

   partition_element——用表的已有分区类型定义即将添加的新分区的界限。

   name——新分区的名字。

   VALUES——对列表分区，用来定义分区包含的值。

   START——对范围分区，用来定义范围的起始值。默认一个情况下，起始值是INCLUSIVE。比如，如果你声明起始日期为‘2008-01-01’， 那么分区就会包含所有大于等于‘2008-01-01’的日期。通常，START表达式的数据类型和分区键列的数据类型相同。如果不相同，那你必须进行显式转换到想要的数据类型。

   END——对范围分区，用来定义范围的末尾值。默认一个情况下，末尾值是EXCLUSIVE。比如，如果你声明起始日期为‘2008-02-01’， 那么分区就会包含所有小于‘2008-02-01’的日期。通常，END表达式的数据类型和分区键列的数据类型相同。如果不相同，那你必须进行显式转换到想要的数据类型。

   WITH——为一个分区设置存储选项。比如，可以设置老一些的分区为追加优化表，而新一些的分区为常规的堆表。关于存储选项的描述，参见CREATE TABLE。

   TABLESPACE——要在其中创建分区的表空间的名字。

   subpartition_spec——仅在创建时没有子分区模板的分区设计中允许。为你正在添加的新分区声明子分区规范。如果分区表在开始定义的时候使用了子分区模板，那么在产生子分区的时候会自动使用这个模板。

   EXCHANGE [DEFAULT] PARTITION

   把另外一个表换到分区层次中已有的一个分区的位置。在一个多级分区设计中，你只能交换最低层次的分区（这些分区包含数据）。

   Greenplum数据库服务器配置参数gp_enable_exchange_default_partition控制EXCHANGE DEFAULT PARTITION子句是否可用。这个参数的默认值是off，这个子句不可用，如果在ALTER TABLE命令中指定这个字句，那么Greenplum数据库会返回一个错误。

   关于这个参数的更多信息，参见服务器配置参数。

   警告：在交换默认分区之前，你必须确保要交换的表中的数据、新的默认分区对默认分区而言是有效的。比如，新的默认分区中的数据一定不能包含在分区表的其他叶子分区中有效的数据。否则，对有交换默认分区的分区表的查询在被Pivotal查询优化器执行后会返回不正确的结果。

   WITH TABLE table_name——将要交换进入分区设计中的表名。你可以交换一个数据库中存有数据的表。比如，一个用CREATE TABLE命令创建的表。

   使用EXCHANGE PARTITION子句，你也可以把一个可读外部表（用create external table命令创建的）置换到分区层次中叶子分区的位置。如果你指定了一个可读外部表，你必须指定WITHOUT VALIDATION子句，以便跳过针对正在交换的分区上的CHECK约束的表验证。

   在下列情况下，外部表和叶子分区的交换是不支持的：

   * 分区表使用subpartition子句创建或者分区有子分区。

   * 分区表有包含check约束的列或者有NOT NULL约束的列。

   WITH | WITHOUT VALIDATION——验证表中的数据是否匹配正在交换的分区的CHECK约束。默认是检验数据是否符合CHECK约束。

   警告：如果你指定WITHOUT VALIDATION子句，你必须确保你要交换到叶子分区去的表中的数据对那个分区上的CHECK约束来讲是有效的。否则，分区表的查询会返回不正确的结果。

   SET SUBPARTITION TEMPLATE

   修改已有分区的子分区模板。一个新的子分区模板设置后，所有新创建的分区就会有新的子分区设计（已有的分区不会被修改）。

   SPLIT DEFAULT PARTITION

   拆分默认分区。在一个多层分区设计中，你只能拆分最底层的默认分区（这些分区可以包含数据）。拆分默认分区会创建新的包含指定值的分区，也会使默认分区只剩下那些不匹配已有分区的数据。

   AT——对列表分区而言的，指定一列值，用来作为拆分的标准。

   START——对范围分区而言的，为新分区制定一个起始值。

   END——对范围分区而言的，为新分区制定一个末尾值。

   INTO——允许为新分区指定一个名字。用INTO子句去拆分一个默认分区时，指定的第二个分区名字应该始终是一个已有的默认分区的名字。如果你不知道默认分区的名字，你可以使用pg_partitions视图来查看。

   SPLIT PARTITION

   拆分一个已有的分区为两个分区。在一个多层分区设计中，只能拆分最底层的分区（这些分区中含有数据）。

   AT——指定一个值作为拆分的标准。这个分区将会被拆分成两个新的分区，指定的拆分值会成为后一个分区的起始值。
   
   INTO——为拆分后的新分区指定名字。

   partition_name

   指定分区的名字。

   FOR（RANK(number)）

   排序分区中，分区的序号。

   FOR(‘value’)

   通过声明一个落在分区界限内的值来指定一个分区。如果FOR声明的值既匹配一个分区又匹配一个子分区（比如，这个值是一个日期，表先用月份分区，然后又用日期分区），那么FOR会优先在匹配到的第一层上起作用（比如，在月份分区上）。如果你想在一个子分区上操作，你必须做如下声明：ALTER TABLE name ALTER PARTITION FOR (‘2008-10-01’) DROP ARTITION FOR (‘2008-10-01’);

   **注解**

   ALTER TABLE命令中指定的表名不能是如下表名：

   * 表中的分区名。

   * CREATE EXTERNAL TABLE命令的LOG ERRORS INTO子句指定的表名。

   当要修改或删除的列是Greenplum数据库分布式键的时候，要格外小心，因为这会改变表的分布式策略。

   Greenplum数据库目前不支持外键约束。对Greenplum数据库中将要实施的唯一约束来说，表必须是哈希分布的（而不是DISTRIBUTED RANDOMLY），并且所有的分布式键列必须和唯一约束的初始列相同。

   添加CHECK或NOT NULL约束需要扫描全表，以验证现有的行是否满足约束。

   DROP COLUMN格式并不在物理上删除列，只是简单的让列对SQL操作不可见。后续对此列的插入或更新操作会在这一列存储一个NULL值。因此，删除一列很快，但是表占用的磁盘空间并不会立即减少，因为删除的列占用的空间并没有回收。随着已有的行的更新，空间会慢慢回收。

   ALTER TYPE需要重写整个表，这一点有时候是一个优势，因为重写过程会消除表中的死空间。比如，立即回收一个已经删除的列所占的空间，最快的方式：ALTER TABLE table ALTER COLUMN anycol TYPE sametype，这里的anycol是表中剩余的任意列，sametype是此列数据类型。这不会对表有任何语法上的改变，但是这个命令会强制重写，重写会消除不在使用的数据。

   如果一个表已经分区，或者有任何子表，不允许添加列、重命名列，或者在子表列的数据类型没有改变的情况先不允许修改父表的列的数据类型。这一点会确保子表的列永远和父表的列相匹配。

   查看分区表的结构，可以使用视图pg_partitions。这个视图会帮助指定任何你想修改的分区。

   一个递归DROP COLUMN操作会移除子表的列，这些子表的列没有从任何父表中继承，也没有独立的定义。

   一个非递归的DROP COLUMN（ALTER TABLE ONLY … DROP COLUMN）不会移除任何子列，但是会把他们从继承的标记为独立定义的。

   TRIGGER、CLUSTER、OWNER和TABLESPACE操作不会递归到子表；也就是说，他们的行为就好像指定了ONLY一样。添加约束时只有CHECK约束是递归的。

   当含有和外部表交换过的叶子分区的分区表的数据没有改变时，下面这些ALTER PARTITION操作才会被支持，否则，会返回错误。

   * 增加或删除一列。

   * 修改列的数据类型。

   当含有和外部表交换过的叶子分区的分区表的数据没有改变时，下面这些ALTER PARTITION操作不会被支持：

   * 设置一个子分区模板。

   * 修改分区属性。

   * 创建默认分区。

   * 设置分布式策略。

   * 设置或删除列上的NOT NULL约束。

   * 添加或删除约束。

   * 拆分外部分区。

   修改一个系统分类表的任何部分都是不允许的。
   
   **示例**

   为表增加一个列：

   ALTER TABLE distributors ADD COLUMN address varchar(30);

   重命名已有的列：

   ALTER TABLE distributors RENAME COLUMN address TO city;

   为列增加NOT NULL约束：

   ALTER TABLE distributors ALTER COLUMN street SET NOT NULL;

   为表增加check约束：

   ALTER TABLE distributors ADD CONSTRAINT zipchk CHECK

   (char_length(zipcode) = 5);

   移动表到另外一个模式：

   ALTER TABLE myschema.distributors SET SCHEMA yourschema;

   为分区表增加一个分区：

   ALTER TABLE sales ADD PARTITION

   START (date '2009-02-01') INCLUSIVE
   
   END (date '2009-03-01') EXCLUSIVE;

   为一个已有的分区设计增加默认分区：

   ALTER TABLE sales ADD DEFAULT PARTITION other;

   重命名一个分区：

   ALTER TABLE sales RENAME PARTITION FOR ('2008-01-01') TO jan08;

   删除范围分区中第一个分区（也是最老的分区）：

   ALTER TABLE sales DROP PARTITION FOR (RANK(1));

   把一个表换进分区设计中：

   ALTER TABLE sales EXCHANGE PARTITION FOR ('2008-01-01') WITH TABLE jan08;

   拆分默认分区（这里已有默认分区的名字是other），以便为2009年一月增加一个新的月份分区：

   ALTER TABLE sales SPLIT DEFAULT PARTITION

   START ('2009-01-01') INCLUSIVE

   END ('2009-02-01') EXCLUSIVE

   INTO (PARTITION jan09, PARTITION other);

   把一个月份分区拆分成两个，第一个包含一月1号到15号，第二个包含一月16号到31号：

   ALTER TABLE sales SPLIT PARTITION FOR ('2008-01-01')

   AT ('2008-01-16')  

   INTO (PARTITION jan081to15, PARTITION jan0816to31);

   **兼容性**

   ADD、DROP和SET DEFAULT格式符合SQL标准。其他的格式是Greenplum数据库扩展了SQL标准。在一个ALTER TABLE命令中指定多个操作也是一个扩展。

   ALTER TABLE DROP COLUMN可以用来删除表中的最后一列，生成一个0列的表。这是对SQL标准的扩展，SQL标准中不允许0列的表。

   **参见**

   CREATE TABLE，DROP TABLE

2.22 ALTER TYPE
-------------------

   修改数据类型的定义。

   **语法**

   ALTER TYPE name OWNER TO new_owner | SET SCHEMA new_schema

   **描述**

   ALTER TYPE改变已有类型的定义。可以修改一个类型的所有者和模式。

   你必须是类型的所有者才能使用ALTER TYPE。修改类型的模式，你必须在新的模式上有CREATE权限。要修改所有者，你必须是所有者中的直接或间接成员，而且角色必须在类型的模式上有CREATE权限。（有了这些规定，当你删除然后重建运算符类型做不了的事情，通过修改类型的拥有者也做不了。然而，超级用户无论如何都可以修改一个类型的拥有者。）

   **参数**

   name

   已有类型的名字。

   new_name

   类型的新所有者的名字。

   new_schema

   类型的新模式。

   **示例**

   修改用户自定义的类型email的所有者为joe：

   ALTER TYPE email OWNER TO joe;

   修改用户自定义的类型email的模式为customers：

   ALTER TYPE email SET SCHEMA customers;

   兼容性

   SQL标准中没有ALTER TYPE语句。

   **参见**

   CREATE TYPE, DROP TYPE

2.23 ALTER USER
---------------

   改变数据库角色（用户）的定义。

   **语法**

   ALTER USER name RENAME TO newname

   ALTER USER name SET config_parameter {TO | =} {value | DEFAULT}

   ALTER USER name RESET config_parameter

   ALTER USER name [ [WITH] option [ ... ] ]

   这里的option是：

   SUPERUSER | NOSUPERUSER

   | CREATEDB | NOCREATEDB

   | CREATEROLE | NOCREATEROLE

   | CREATEUSER | NOCREATEUSER

   | INHERIT | NOINHERIT

   | LOGIN | NOLOGIN

   | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'

   | VALID UNTIL 'timestamp

   **描述**

   ALTER USER是一个过时的命令，因为历史原因而继续接受。它是ALTER ROLE的别名。参加ALTER ROLE以获得更多信息。

   **兼容性**

   ALTER USER是Greenplum数据库的扩展。SQL标准把用户的定义留给了实现。

   **参见**

   ALTER ROLE

2.24 ANALYZE
---------------

   收集数据库的统计数据。

   **语法**

   ANALYZE [VERBOSE] [ROOTPARTITION [ALL] ] [table [ (column [, ...] ) ]]

   **描述**

   ANALYZE收集数据库中表内容的统计数据，并把结果存在系统表pg_statistic中。随后，Greenplum数据库用这些统计数据来帮助决定查询的效率最高的执行计划。

   如果没有参数，ANALYZE收集当前数据库的每个表的数据。可以指定一个表名，以便只收集那一个表的统计数据。也可以指定一组列名，这样，只有这些列的统计数据会被收集。

   ANALYZE不会收集外部表的统计数据。

   重要：如果你想在Pivotal查询优化器开启的情况下，在分区表上执行查询，你必须使用ANALYZE ROOTPARTITION命令收集分区表的根分区上的统计数据。关于Pivotal查询优化器的更多信息，参见《Greenplum数据库管理员指南》的“查询数据”。

   注意：你可以使用Greenplum数据库工具analyzedb去更新表的统计数据。analyzedb工具可以并发更新多个表的统计数据。仅在统计数据不是当前或者不在的时候，这个工具也可以检查表的统计数据和更新统计数据。关于这个工具的更多信息，参加《Greenplum数据库工具指南》。

   **参数**

   ROOTPARTITION [ALL]

   仅在分区表的根分区上收集统计数据。当你指定ROOTPARTITION时，你必须指定ALL或者分区表的名字。

   如果在指定ROOTPARTITION的时候指定了ALL，Greenplum数据库收集数据库中所有分区表的根分区的统计信息。如果数据库中没有分区表，就会返回一条信息，表示没有分区表。对于不是分区表的表，它们的统计信息就不会收集。

   如果随着ROOTPARTITION指定了表的名字，而且这个表不是分区表，那不会为此表收集统计信息，而且会返回一条警告信息。

   对VACUUM ANALYZE来说，ROOTPARTITION子句是无效的。VACUUM ANALYZE R ROOTPARTITION命令会返回一个错误。

   刷新根一级的统计数据的代价和分析一个叶子分区的代价相当。

   对分区表sales_curr_yr而言，这条示例命令仅会收集分区表的根分区的统计信息。ANALYZE ROOTPARTITION sales_curr_yr;

   下面这条示例命令会收集数据库中所有的分区表的根分区的统计信息。

   ANALYZE ROOTPARTITION ALL;

   VERBOSE

   使进度信息显示出来。一旦指定，ANALYZE会生成如下信息：

   * 要处理的表。

   * 执行之后产生示例表的查询。

   * 将姚收集其统计信息的列。

   * 用来收集单一列的不同统计信息的查询。

   * 将要产生的统计信息。

   table

   指定要分析的表的名字。默认是当前数据库的所有表。

   column 

   要分析的列的名字。默认是所有列。

   **注解**

   定期或者仅仅在对表内容做了重大修改之后运行ANALYZE是一个好主意。精确的统计信息会帮助Greenplum数据库选择最合适的查询计划，从而可以提高查询处理的速度。常用的策略是在一天中使用率比较低的时段一天运行一次VACUUM和ANALYZE。

   ANALYZE仅需要目标表上的一个读锁，以便它可以和目标表上的其他活动并行运行。

   对分区表来说，如果有很大数量的分区要分析并且只有很少叶子子表修改过，指定那一部分来分析，是根分区还是子分区（叶子子表），是很有用的。

   * 当你在根分区表上运行ANALYZE，所有的叶子子表的统计信息都会收集（Greenplum数据库创建的子表层级中最低一级的表）。

   * 当你在叶子子表上运行ANALYZE，只有那个叶子子表的统计信息会被收集。当你在一个不是叶子子表的子表上运行ANALYZE时，不会收集统计信息。

   比如，你可以创建一个分区表，含有2000到2010年的分区，并且每一年的每一月都有一个子分区。当你在2005年的子分区上运行ANALYZE时，不会收集统计信息。如果你在2005年3月的叶子子表上运行ANALYZE，只会收集那个叶子子表的统计信息。

   注意：当你使用CREATE TABLE命令 创建分区表的时候，Greenplum数据库会不仅会创建你指定的表（根分区或父表），而且还会基于你指定的分区层级创建表的层级结构（子表）。分区表、子表和他们的继承关系被记录在系统视图pg_partitions中。

   对于一个含有和外部表交换过的叶子子分区的分区表来说，ANALYZE不会为外部表分区收集统计信息：

   * 如果运行ANALYZE [ROOTPARTITION]，外部表分区不会被分析，根表的统计信息不包含外部表分区的统计信息。

   * 如果在一个外部表分区上运行ANALYZE，这个分区不会被分析。

   * 如果指定VERBOSE子句，会显示一条通知消息：skipping external table。

   由ANALYZE收集的统计信息一般包括每一列中最常用的值和一个显示每一列的合适数据分布的直方图。如果ANALYZE认为他们没有意义或者列的数据类型不支持合适的运算符，这两者之一或者两者都可能被忽略。

   对大表来说，ANALYZE会检查表内容中一块随机数据，而不是表中的每一行。这样每一个大表都可以在很短时间内分析完。注意，这里的统计信息是近似值，而且会在每次运行ANALYZE时有微小的改变，即使表的内容没有任何改变。这会导致EXPALIN显示的执行计划中估计的代价有微小的变化。在极少数情况下，这种不确定会导致查询优化器为不同的ANALYZE选择不同的查询计划。为了避免这一情况，可以调整default_statistics_target配置参数或者用alter table…alter column … set statistics逐列设置各列的统计目标，从而提高ANALYZE收集的统计数据的数量。 目标值设置最常用的值的最大个数和直方图中的条目的最大值。默认目标值是10，但是这个值可以调高或调低，以便执行计划获得精确或粗糙 的ANALYZE时间和pg_statstics占用的空间。特别地，当统计目标设为0，就不会为那一列收集统计数据。对从来不会用作WHERE、GROUP BY、ORDER BY的列来说，这么做是有用的，因为执行计划不会使用这些列的统计数据。

   在将要分析的各列中最大的统计目标决定了准备统计数据所需要的表记录的个数。增加统计目标会成比例地增加ANALYZE需要的时间和空间。

   如果Greenplum在执行ANALYZE操作为表收集统计信息时发现所有的样本数据页都是空的（没有有效数据），那Greenplum数据库会显示一条信息，说明需要执行VACUUM FULL操作。如果样本页都是空的，那表的统计数据也会不准确。表有大幅改动后，页会变空，比如删除大量的行之后。VACUUM FULL操作会移除空页，这样ANALYZE操作就能收集到精确的数据了。

   **示例**

   收集表mytable的统计数据：

   ANALYZE mytable;

   **兼容性**

   SQL标准中没有ANALYZE语句。

   **参见**

   ALTER TABLE, EXPLAIN, VACUUM, 《Greenplum数据库工具指南》中的analyzedb工具。

2.25 BEGIN
--------------

   开启一个事务块。

   **语法**

   BEGIN [WORK | TRANSACTION] [transaction_mode]

   [READ ONLY | READ WRITE]

   这里transaction_mode是：

   ISOLATION LEVEL | {SERIALIZABLE | READ COMMITTED | READ UNCOMMITTED}

   **描述**

   BEGIN开启一个事务块，也就是，BEGIN命令只会的所有语句都会在一个单独的事务中执行，除非有一个显式的COMMIT或ROLLBACK。默认情况下（没有BEGIN），Greenplum数据库在自动提交模式下执行事务，每个语句都在自己的事务中执行，在语句结束时会隐式执行一个提交（如果执行成功，否则会执行回退）。

   在事务块中语句会执行的更快，因为事务开始/提交需要显著的CPU和磁盘活动。多条语句在一个事务中执行对确保多个相关改动的一致性有好处：其他会话将无法看到中间状态，其中并不是所有相关的更新都已经完成。

   如果指定隔离水平或读写模式，新的事务有这些特征，就像TRANSACTION被执行一样。

   **参数**

   WORK

   TRANSACTION

   可选关键字，没有效果。

   SERIALIZABLE

   READ COMMITTED

   READ UNCOMMITTED

   SQL标准定义了四个事务隔离水平：READ COMMITTED, READ

   UNCOMMITTED, SERIALIZABLE, 和 REPEATABLE READ。默认行为是一个语句在执行之前只能看到行提价（READ COMMITTED）。在Greenplum数据中READ UNCOMMITTED和READ COMMITTED一样。REPEATABLE READ不支持；如果需要，可以用SERIALIZABLE。SERIALIZABLE是最严格的隔离级别。这个级别会模拟连续事务执行，就好像事务一个接一个地顺序执行，而不是并发执行。应用程序使用这个级别，必须准备好由于序列失败导致的事务重来。

   READ WRITE

   READ ONLY

   决定事务是读写还是只读。读写是默认情况。当事务是只读时，下列SQL语句是不被允许的：INSERT、UPDATE、DELETE和COPY FROM，如果它们要写入的表不是临时表；所有的CREATE、ALTER和DROP命令；GRANT、REVOKE、TRUNCATE；如果EXPLAIN ANALYZE和EXECUTE要执行的命令在以上之列，那么EXPLAIN ANALYZE和EXECUTE也不能执行。

   **注解**

   START TRANSACTION和BEGIN有相同的功能。

   可以使用COMMIT和ROLLBAKC来终止一个事务块。

   在事务块中发出BEGIN会引起一个警告信息。事务状态不会受影响。如果想在事务块中嵌套事务，请使用保存点（参见SAVEPOINT）。

   **示例**

   开启一个事务块：

   BEGIN;

   使用序列隔离级别开启一个事务块：

   BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

   **兼容性**

   BEGIN是Greenplum数据库的语言扩展。它和SQL标准中的START TRANSACTION等价。

   偶尔，BEGIN关键字可在嵌入式SQL中用作另外的用途。建议你在移植数据库应用程序的时候小心事务的语法。

   **参见**

   COMMIT，ROLLBACK，START TRANSACTION，SAVEPOINT

2.26 CHECKPOINT
-----------------

   强制一个事务日志检查点。

   **语法**

   CHECKPOINT

   **描述**

   预写式日志(WAL)偶尔在事务日志中放置一个检查点。自动检查点间隔是通过Greenplum数据库的segment实例的服务器配置参数checkpoint_segments和checkpoint_timeout来设置的。CHECKPOINT命令强制在命令发出后立即放置检查点，无需等待安排的检查点。

   检查点是事务日志序列中的一个点，在这个点所有的数据文件都会更新以反映日志中的信息。所有的数据文件也会刷盘。

   只有超级用户才可以调用CHECKPOINT。这个命令不推荐在常规运行中使用。

   **兼容性**

   CHECKPOINT命令是Greenplum数据库的语言扩展。
   
2.27 CLOSE
--------------

   关闭一个游标。

   **语法**

   CLOSE cursor_name

   **描述**

   CLOSE会释放打开的游标占用的资源。游标关闭之后，后续不允许在它之上进行操作。游标应该在不需要的时候关闭。

   当一个事务被COMMIT或ROLLBACK终止后，每一个打开的不可用游标会被隐式关闭。如果创建游标的事务被ROLLBACK终结，那么可用游标也会关闭。如果创建游标的事务成功提交，可用游标依然是打开的，除非显式执行CLOSE，或者客户端断开。

   **参数**

   cursor_name

   将要关闭的打开的游标的名字。

   **注解**

   Greenplum数据库没有显式的OPEN游标的语句。声明一个游标它就会被认为是打开的。可以用DECLARE语句来声明（或打开）一个游标。

   **示例**

   关闭游标portala：

   CLOSE portala;

   **兼容性**

   CLOSE完全兼容SQL标准。

   **参见**

   DECLARE， FETCH， MOVE

2.28 CLUSTER
--------------

   根据索引物理地重排磁盘上堆存储的表。Greenplum数据库不推荐这个操作。

   **语法**

   CLUSTER indexname ON tablename

   CLUSTER tablename

   CLUSTER

   **描述**

   根据索引重排堆存储表。在追加优化存储表中不支持CLUSTER。聚集索引意味着磁盘上的记录会根据索引信息物理上重排。如果你需要的记录在磁盘是随机分布的，那数据库为了取得请求的记录必须找遍全盘。如果这些记录紧紧地存在一起，那么取记录就会是顺序的。关于聚集索引的一个例子是一个按照日期顺序排序的日期列。针对某一个日期范围的查询会取得磁盘重排后的记录，这会利用更快的顺序访问。

   聚集是一次性的操作：当表顺序更新后，它的变化不会被聚集。也就是说，不会根据他们的所有顺序，去存心的行或者更新之后的行。如果你希望这样，你可以定期通过这个命令重新聚集。

   当一个表用这个命令重新聚集后，Greenplum数据库会记住用来聚集的索引。CLUSTER tablename 会在之前聚集过的同一个列上重新聚集表。不带参数的CLUSTER会重聚集当前数据库中调用此命令的用户所拥有的所有已经聚集过的表，当超级用户调用该不带参数的CLUSTER的时候，会重聚集所有的表。这个格式的CLUSTER不能在事务块中执行。

   当一个表聚集之后，就会获得一个ACCESS EXCLUSIVE锁。这个其他的数据操作在这个表上运行，除非CLUSTER完成。

   **参数**

   indexname

   索引的名字。

   tablename

   表名。

   **注解**

   当你随机访问表中的单个行时，表中实际的数据顺序是不重要的。当你想更多地访问某一些数据，并且有索引把这些数据排序在一起的时候，你会受益于CLUSTER。当你在请求表中的一组索引值时，或者请求单个但是能匹配多行的索引值时，它匹配的所有其他的行可能已经在同一个表页上，因此，你可以节省磁盘访问并加速查询。

   在聚集操作期间，一个包含索引顺序的表数据的表的临时拷贝会被创建。表上的索引页会创建临时拷贝。因此，你需要至少释放表和索引所占空间之和的磁盘空间。

   因为查询分析器会记录关于表重排的统计信息，所以建议在新的聚集表上运行ANALYZE。否则，计划器会选择比较差的查询计划。

   有另外一种聚集数据的方法。CLUSTER命令使用你指定的索引扫描原表来重排表。在大表上这会很慢，因为记录是按照索引的顺序取的，如果表是乱序的，记录在随机页上，则每一个记录移动会取回一个磁盘页。（Greenplum数据库有一个缓存，但是大表的大部分是不会命中缓存的。）另外一种聚集表的方式会使用下面的语句：

   CREATE TABLE newtable AS SELECT * FROM table ORDER BY column;

   这会使用Greenplum数据库的排序码去产生想要的顺序，这通常比对无序数据的索引扫描更快。接下来你可以删除旧表，使用ALTER TABLE…RENAME去重命名旧表到新的名字，然后重建表的索引。这种方法最大的确定是，它不会保留OID、约束、赋给的权限和表的其他的辅助属性，所有这些必须手动重建。另外一个缺点是这种方式需要一个大小和表大小相同的排序的临时文件，因此磁盘使用的峰值是三倍的表大小而非两倍的表大小。

   注意：追加优化表不支持CLUSTER。

   **示例**

   根据索引emp_inx来聚集表employees：

   CLUSTER emp_ind ON emp;

   通过重建并以索引序加载数据来聚集一个大表：

   CREATE TABLE newtable AS SELECT * FROM table ORDER BY column;

   DROP table;

   ALTER TABLE newtable RENAME TO table;

   CREATE INDEX column_ix ON table (column);

   VACUUM ANALYZE table;

   **兼容性**

   SQL标准中没有CLUSTER语句。

   **参见**

   CREATE TABLE AS, CREATE INDEX

2.29 COMMENT
--------------

   定义或者改变一个对象的注释。
   
   **语法**

   COMMENT ON

   { TABLE object_name |

   COLUMN table_name.column_name |

   AGGREGATE agg_name (agg_type [, ...]) |

   CAST (sourcetype AS targettype) |

   CONSTRAINT constraint_name ON table_name |

   CONVERSION object_name |

   DATABASE object_name |

   DOMAIN object_name |

   FILESPACE object_name |

   FUNCTION func_name ([[argmode] [argname] argtype [, ...]]) |

   INDEX object_name |

   LARGE OBJECT large_object_oid |

   OPERATOR op (leftoperand_type, rightoperand_type) |

   OPERATOR CLASS object_name USING index_method |

   [PROCEDURAL] LANGUAGE object_name |

   RESOURCE QUEUE object_name |

   ROLE object_name |

   RULE rule_name ON table_name |

   SCHEMA object_name |

   SEQUENCE object_name |

   TABLESPACE object_name |

   TRIGGER trigger_name ON table_name |

   TYPE object_name |

   VIEW object_name }

   IS 'text'

   **描述**
   
   COMMENT会保持数据库对象的注释。对同一个对象发出一个新的COMMENT可以修改注释。一个对象只能保持一个注释字符串。在text字符串的位置写上NULL，可以删除注释。对象删除的时候注释会自动删除。

   注释可以很容易地用psql的元命令\dd，\d+和\l+返回。其他返回注释的用户接口可以建立在psql使用的相同内建函数上，也就是obj_description，col_description和shobj_description。
   
   **参数**

   object_name

   table_name.column_name

   agg_name

   constraint_name

   func_name

   op

   rule_name

   trigger_name

   要添加注释的对象的名字。表、聚合、域、函数、索引、运算符、运算符类、类型、和视图的名字。

   注意：Greenplum不支持触发器。

   agg_type

   聚集函数运行于其上的数据类型。在输入数据类型的位置上写上*可以引用无参的聚集函数。

   sourcetype

   转换的源数据类型。

   targettype

   转换的目标数据类型。

   argmode

   函数的参数模式：IN、OUT或INOUT。如果省略，默认为IN。注意COMMENT ON FUNCTION并不在意OUT参数，因为在决定函数的身份的时候只需要输入参数。所以列出IN和INOUT参数就够了。

   argname

   函数参数的名字。注意COMMENT ON FUNCTION并不在意参数名字，因为在决定函数的身份的时候只需要参数类型。

   argtype

   函数参数的数据类型。

   large_object_oid

   大对象的OID。

   PROCEDURAL

   这是一个噪音词。

   text

   新的注释，写成一个字符串字面值；或者NULL去删除注释。

   **注解**

   目前注释没有安全机制：任何登陆到数据库的用户都可以看到数据库中所有对象的注释（只有超级用户才可以修改不是他们的对象的注释）。共享对象，比如，数据库、角色、表空间等的注释是全局存储的，任何连接到任一数据库的用户都可以看到共享对象的注释。因此，不要给注释添加安全相关的信息。

   **示例**

   为表mytable添加一个注释：

   COMMENT ON TABLE mytable IS ‘This is my table’;

   删除刚才添加的注释：

   COMMENT ON TABLE mytable IS NULL;

   **兼容性**

   SQL标准中没有COMMENT语句。
   
2.30 COMMIT
-------------

   提交当前事务。

   **语法**

   COMMIT [WORK | TRANSACTION]

   **描述**

   COMMIT会提交当前事务。所有对此事务做的操作都会对其他用户可见，而且如果发生崩溃，也会保证持久。

   **参数**

   WORK

   TRANSACTION

   可选关键字，无作用。

   **注解**

   使用ROLLBACK可以终止一个事务。

   在事务之外发出COMMIT没有任何害处，但是会引发一条警告信息。

   **示例**

   提交当前事务，使所有的修改持久化：

   COMMIT；

   **兼容性**

   SQL标准指定了两种形式COMMIT和COMMIT WORK，这个命令是完全符合标准的。

   **参见**

   BEGIN, END, START TRANSACTION, ROLLBACK

2.31 COPY
------------

   在文件和表之间拷贝数据。

   **语法**

   COPY table [(column [, ...])] FROM {'file' | STDIN}

   [ [WITH]

   [OIDS]

   [HEADER]

   [DELIMITER [ AS ] 'delimiter']

   [NULL [ AS ] 'null string']

   [ESCAPE [ AS ] 'escape' | 'OFF']

   [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']

   [CSV [QUOTE [ AS ] 'quote']

   [FORCE NOT NULL column [, ...]]

   [FILL MISSING FIELDS]

   [[LOG ERRORS [INTO error_table] [KEEP]

   SEGMENT REJECT LIMIT count [ROWS | PERCENT] ]

   COPY {table [(column [, ...])] | (query)} TO {'file' | STDOUT}

   [ [WITH]

   [OIDS]

   [HEADER]

   [DELIMITER [ AS ] 'delimiter']

   [NULL [ AS ] 'null string']

   [ESCAPE [ AS ] 'escape' | 'OFF']

   [CSV [QUOTE [ AS ] 'quote']

   [FORCE QUOTE column [, ...]] ]

   [IGNORE EXTERNAL PARTITIONS ]

   **描述**

   COPY在Greenplum数据库表和标准文件系统文件之间移动数据。COPY TO把表内容拷贝到文件，而COPY FROM会把数据从文件拷贝到表中（追加数据到表中已有的数据后面）。COPY TO也可以拷贝SELECT查询的结果。

   如果指定一些列，那么COPY只会将这些列的数据在文件和表之间拷贝。对于没有指定的列，COPY FROM会插入默认值。

   带一个文件名的COPY命令会让Greenplum数据库master主机直接读取或写入文件。这个文件对master主机必须是可访问的，文件名必须从master主机的角度去指定。如果指定STDIN和STDOUT，数据会通过客户端和master之间的连接传输。

   如果使用SEGMENT REJECT LIMIT，那么COPY FROM操作会在单行错误隔离模式运行。这次发布中，单行错误隔离模式仅会应用到输入文件有格式错误的情况，比如，多余的或者缺失的属性，列的数据类型错误或者不合法的客户端编码序列。像违反NOT NULL, CHECK或UNIQUE约束等约束错误依然会用全有或全无输入模式处理。用户可以指定可接受的错误行数（在每一个Segment上），超过可接受的错误行数，整个COPY FROM操作可能会终止，不会再加载行。注意错误行数是按Segment算的，不是按整个加载操作算的。如果每个segment的拒绝限制没有达到，那没有错误的行将会加载。如果限制没有达到，所有好的行会加载，而任何错误行都会被丢弃。如果你想保留错误行，以便将来的检查，你可以用LOG ERRORS INTO子句声明一个错误表。之后，有格式错误的行就会被记录到指定的错误表中。

   输出

   如果成功结束，COPY命令可以返回一个如下格式的命令标签，这里的count是已经拷贝的行数：

   COPY count

   如果在单行错误隔离模式运行COPY FROM命令，当有因为格式错误而没有加载的行时，会返回一条如下的通知信息，这里的count是拒绝的行数：

   NOTICE： Rejected count badly formatted rows.

   **参数**

   table

   已有表的名字。

   column

   要拷贝的列。如果不指定列，将会拷贝表中表中所有的列。

   query

   一条SELECT或VALUES命令，它们的结果会被拷贝。注意查询周围的括号是必须的。

   file

   输入或输出文件的绝对路径。

   STDIN

   指定输入来自客户应用。

   STDOUT

   指定输出到客户应用。

   OIDS

   指定拷贝每一行的OID。（如果为一个没有OID的行指定OIDS或者拷贝一条查询时指定OIDS将会引发错误。）

   分隔符

   分割表中每行的各个列的单个ASCII字符。默认字符模式是tab字符，CSV模式是逗号。

   null string

   代表null值的一个字符串。默认，文本模式是\N（斜杠-N），CSV模式是无引号的空值。当你不想区分null和空字符串的时候，你可能会希望在文本模式用空字符串。使用COPY FROM时，任何匹配这个字符串的数据项都会被存储成一个空值，因此你应该确保你使用的字符串和用COPY TO时用的字符串相同。

   escape

   指定用作C转义字符序列的单个字符（比如\n，\t，\100等），或者指定用作行或列分隔符的数据字符。确保选取的转义字符不会用在实际的数据中。默认的转义字符是文本模式是\（反斜杠），CSV文件中是”（双引号），当然也可以指定其他的字符代表转义字符。把转移值指定为OFF，可以取消文本格式文件中的转义。这对web日志数据很有用，因为这些数据中有很多的反斜杠，这些反斜杠不是用来转义的。

   NEWLINE

   指定数据文件中的换行符，LF（换行， 0x0A），CR（回车， 0x0D）， 或回车换行（回车换行，0x0D 0x0A）。如果不指定，Greenplum数据库segment会检测第一行数据中的换行符，并使用第一个遇到的换行符。

   CSV

   选择逗号分隔值（CSV）的模式。

   HEADER

   指定文件包含有列名的头行。在输出时，第一行包含表中的列名，在输入时，第一行被忽略。
   
   quote

   指定CSV模式的引号字符。默认是双引号。

   FORCE QUOTE

   在CSV COPY TO模式，强制给指定列中的非NULL值加引号。NULL输入不会加引号。

   FORCE NOT NULL

   在CSV COPY FROM模式中，把每一个指定的列当做引号引起来的列来处理，因此就没有null值了。对CSV模式中的默认null字符串（两个分隔符之间没有任何东西），这会将缺失值当成0字节长度的字符串。

   FILL MISSING FIELDS

   对TEXT和CSV模式的COPY FROM来说，指定FILL MISSING FIELDS会将缺失最后一列的值设置为NULL（而不是报告一个错误）。空行，有NOT NULL约束的字段，和有尾部分隔符的行依然会报告错误。

   LOG ERRORS [INTO error_table] [KEEP]

   这是一个可选子句，它会在SEGMENT REJECT LIMIT子句之前记录格式错误的行的信息。当运行在单行错误隔离模式时，INTO error_table子句会指定一个错误表，用来记录有格式错误的行。

   如果没有指定INTO error_table子句，错误日志信息会存储在内部（而不是一个错误表中）。记录在内部的错误日志信息会被Greenplum数据库的的内置SQL函数gp_read_error_log()访问的。

   如果error_table已经存在，那么就会使用它。如果它不存在，就会创建它。如果error_table存在，而且没有随机分布（就是在创建表的时候没有指定DISTRIBUTED RANDOMLY子句），就会产生错误。

   如果这条命令生成了一个错误表，而实际没有错误产生，那么默认情况下会在操作结束后删除错误表，除非指定KEEP。如果生成了错误表，并且实际的错误超过了错误限额，整个事务就会回滚，错误表也不会保存。在这种情况下如果你想保留错误表，就得在运行COPY之前创建错误表。

   关于错误日志信息和查看和管理错误日志信息的内置函数，参见注解一节。

   注意：INTO error_table可选子句是过时的，不会再未来的发布中支持了。未来只会支持内部错误日志。

   SEGMENT REJECT LIMIT count [ROWS | PERCENT]

   在当行错误隔离模式运行COPY FROM操作。假如在加载操作期间，在任何一个Greenplum数据库的segment上拒绝限制数量没有达到，如果输入行中有格式错误，那么错误的行会被丢弃。拒绝限制数量可以指定为行数（默认值）或者总行数的一个百分比（1-100）。如果使用PERCENT，每个Segment仅会在参数gp_reject_percent_threshold指定的行数被处理之后开始计算坏行的百分比。gp_reject_percent_threshold的默认值是300行。比如像违反NOT NULL,CHECK和UNIQUE约束等约束错误还会在all-or-nothing输入模式处理。如果达到限值，所有的好的行被会加载，而错误的行会被丢弃。

   注意：Greenplum数据库限制初始的包含格式错误的行的个数，如果SEGMENT REJECT LIMIT没有被提前触发，或者没有指定。如果开头的1000行被拒绝，COPY操作会停止，然后回滚。

   初始拒绝行数的限制可以通过Greenplum数据库的服务器配置参数gp_initial_bad_row_limit修改。关于这个参数，参见服务器配置参数。

   IGNORE EXTERNAL PARTITIONS

   当从分区表拷贝数据时，不会从外部表的叶子子分区拷贝数据。当数据没有拷贝的时候，会在日志文件中增加一条消息。

   如果没有指定这个子句，Greenplum数据库尝试去从外部表的叶子子分区拷贝数据时，会发生错误。

   关于指定SQL查询从外部表的叶子子分区拷贝数据的信息请参考下一节的注解。

   **注解**

   COPY只能用在表上，不能用在外部表或视图上。然而，你可以写这样的COPY：

   COPY (SELECT * FROM viewname) TO …

   如果想从以外部表作为叶子子分区的分区表总拷贝数据，使用SQL查询拷数据。比如，如果表my_sales包含一个是外部表的叶子子分区，COPY my_sales TO stdout会返回错误。下面的命令会把数据发送到标准输出：

   COPY (SELECT * FROM my_sales) TO stdout

   BINARY关键字会使所欲的数据以二进制格式而不是文本模式存储或读取。这会比常规文本模式更快，但是二进制格式文件在Greenplum数据库各版本或机器架构之间的移植性比较差。如果是二进制格式，你也不能在单行错误隔离模式运行COPY FROM。

   在使用COPY TO时，必须在表上有SELECT权限。在使用COPY FROM时，必须在表上有INSERT权限。

   COPY命令中列出的文件是直接被数据库服务器而不是客户端应用读取或写入的。因此，它们必须放在Greenplum数据库的master机器上，或者能被master机器访问。它们必须能被Greenplum数据库的系统用户（服务器运行的用户ID）访问、读、写，而不是客户端。命名文件的COPY仅允许数据库超级用户操作，因为这会允许读或写任何服务器有权访问的文件。

   COPY FROM会出发目标表的触发器和检查约束。但是它不会引起重写规则。注意在这次发布中，单行错误隔离模式不会检查是否违反约束。

   COPY的输入和输出会受DataeStyle的影响。为了确保对使用非默认DateStyle设置的Greenplum数据库其他安装的可移植性，在使用COPY TO之前应用把DataStyle设置为ISO。

   默认情况下，在第一个错误发生时，COPY会停止操作。这在COPY TO发生时，不应该导致问题，但是在COPY FROM中目标表会受到更早的行。这些行将会是不可见或不可访问的，但是他们依然占用磁盘空间。如果失败刚好发生在一个较大的COPY FROM操作中，那么这些行可能会占用可观的浪费的磁盘空间。你也许想用VACUUM去回收这些浪费的空间。在单行错误隔离模式总，可以使用另外一个选项，以便在停止装在无错行时过滤错误行。

   当指定LOG ERRORS INTO error_table，Greenplum数据库会创建表error_table，以包含在读取外部表时的错误。这个表可以定义如下：

   CREATE TABLE error_table_name (cmdtime timestamptz, relname text, filename text, linenum int, bytenum int, errmsg text, rawdata text, rawbytea bytea) DISTRIBUTED RANDOMLY;

   可以用SQL命令查看表中的信息。

   没有指定INTO error_table时，内部存储的错误日志数据：

   * 使用内置SQL函数gp_read_error_log(‘table_name’)。这需要有表上的SELECT权限。下面的示例展示了用COPY命令加载数据到ext_expenses表的错误日志信息：

     SELECT * from gp_read_error_log(‘ext_expenses’);

     错误日志和错误表有相同的列。

     如果table_name不存在，这个函数会返回FALSE。

   * 如果指定表的错误日志数据存在，新的错误日志数据会追加到已有的错误日志数据上去。错误日志信息不会复制到镜像segment上。

   * 用内置SQL函数gp_trancate_error_log(‘table_name’)去删除表的错误日志数据。这需要表的所有者权限。下面的示例会删除移动数据到表ext_expenses时抓取的错误日志信息：

     SELECT gp_truncate_error_log(‘ext_expenses’);

     如果表名不存在函数会返回FALSE。

     指定*通配符会删除当前数据库中所有的表的错误日志信息。指定*.*字符串会删除所有数据中的错误日志信息，包括之前数据库问题而没有删除的错误日志信息。如果指定*，需要数据库的所有者权限。如果指定*.*，需要操作系统超级用户权限。

   Greenplum数据库的非超级用户运行COPY命令时，这个命令可以用一个资源队列来控制。这个资源队列可以通过ACTIVE_STATEMENTS参数来配置，这个参数指定可被此队列上指定的用户执行的最大查询数量。Greenplum数据库不会把一个代价值或内存值用在COPY命令中，带代价限制或内存限制的资源队列不会影响COPY命令的运行。

   非超级用户可以运行下列COPY命令之一：

   * 源是stdin的COPY FROM命令。

   * 目标是stdout的COPY TO命令。

   关于资源队列，可以参见《Greenplum数据库管理员指南》中的“用资源队列的负载管理”。

   文件格式

   COPY支持的文件格式。

   文本格式

   如果运行COPY时没有指定BINARY或CSV选项，读写的数据就是一个文本文件，文件中一行是表中的一行。行中的各列用delimiter字符来分割（默认是tab）。列本身的值是输出函数产生的字符串，或是输入函数可接受的字符串，这些字符串的类型是各列的数据类型。指定的null字符串会替换是null列的值。如果输入文件的任何一行有多余的列或缺少的列，COPY FROM都会产生错误。如果指定OIDS，则OID会在用户数据列之前被作为第一列写入或读取。

   数据文件有两个保留字符，这两个字符对COPY来说有特殊的意义：

   * 指定的分隔符（默认是tab），它被用来在数据文件中分割字段。

   * UNIX风格的换行（\n或0x0a），它被用来在数据文件中开启新行。强烈建议产生COPY数据的应用程序把数据中的换行转换成UNIX风格的换行，而不是微软Windows风格的回车换行（\r\n或\0x0a 0x0d）。

   如果你的数据中包含以上这些字符，你必须对它们进行转义，以便COPY把它们当成数据，而不是分隔符或换行符。

   默认情况下，文本文件中转义字符是\（反斜杠），在CSV文件中是”（双引号）。如果想使用另外的字符作为转义字符，可以使用ESCAPE AS子句。确保所选的转义字符不会用作数据文件中实际的数据值。你也可以用ESCAPE ‘OFF’来关闭文本文件中的转义。

   例如，假设你有一个表，其中有三列，你想用COPY加载如下的三个字段。

   * percentage sign = %

   * vertical bar = |

   * backslash = \

   你指定的分隔符是 | （管道字符），指定的转义字符是 *（星号）。数据文件中规范的行应该是这样：

   percentage sign = % | vertical bar = *| | backslash = \

   注意作为数据的一部分的管道符号是如何用星号转义的。还要注意这里我们不用对反斜杠进行转义，因为我们使用了另外一个字符作为转义字符。

   下列字符如果作为列值的一部分出现，必须在其前面加上转义字符：转义字符本身，换行字符，回车字符，和当前的分隔符。可以用ESCAPE AS子句另外指定一个转义字符。

   CSV格式

   这种格式用来导入导出许多程序使用的CSV文件格式，比如spreadsheets。除了Greenplum数据库的标准文本模式使用的转义字符，Greenplum数据库还产生并能识别普通的CSV的转义机制。

   字段中的值是被DELIMITER字符分割开的。如果值中包含分隔符字符，引号字符，转义字符（它默认是引号），NULL字符串，回车，或换行，那整个值要加引号前缀和后缀。也可以用FORCE QUOTE强制特定列的非NULL值加引号。

   CSV格式没有如何区分NULL值和空字符串的标准方法。Greenplum数据库的COPY用引号来处理这种情况。NULL值输出的时候是NULL字符串，且不加引号，而和NULL字符串匹配的数据值是引起来的。因此，使用默认设置，NULL写做不加引号的空字符串，空字符串写做双引号。读取的时候也遵循相同的规则。你可以使用FORCE NOT NULL去防止特订列的NULL的输入比较。

   因为反斜杠在CSV格式中不是特殊字符，数据结束标志\.也可以出现在数据值中。为了避免任何的错误解读，在输出时，如果数据值中某行单独出现的\.会被自动加引号，在输入时，如果加了引号，是不会被解释为数据结束标志的。如果你在加载另外一个应用程序生成的一个文件，其中有单个未加引号的列，而且有\.值，i也许需要在输出文件中为这个值加上引号。

   注意：在CSV模式中，所有的字符都是有意义的。空白所包围的引起来的值，或者DELIMITER外的任何字符都会包含这些字符。如果从一个用空白符填充CSV行到固定宽度的系统中导入数据，会引起错误。如果出现这种情况，必须在导入数据到Greenplum数据库之前，预处理一下CSV文件，去掉尾部的空白。

   注意：CSV模式会识别和生成CSV文件，这些文件中可以有用引号引起来的回车和换行值。因此这些文件不是像文本模式那样严格一行对应表中一行的。

   注意：有些程序会生成奇怪的偶尔违反常规的CSV文件，因此文件格式是一种约定而不是一种标准。因此你会遇到无法用这种机制导入的文件，而COPY也会生成其他程序无法处理的文件。

   二进制格式

   BINARY格式包含一个文件头，零或多个包含实际数据的元组，和一个文件尾。头部和数据都是网络字节序的。

   * 文件头——文件头包含15个字节的固定字段，后跟变长的头部扩展区域。固定字段是：

   * 签名——11个字节序列PGCOPY\n\377\r\n\0——注意零字节是签名的必须部分。（设计签名是为了容易识别文件。签名可以被行尾转换过滤器改变，丢掉零字节，丢掉高位字节，或其他的改变。）
    * 标志字段——32位整数掩码，表示文件格式的重要性。编号从0（LSB）到31（MSB）的二进制位。注意这些字段是以网络字节序存储的（大端优先），文件格式中的所有整数字节都是这样的。16-31位是保留位，用来表示关键文件格式；读取时如果发现这个范围的位有异常，应该中止读取。0-15位时为标识向后兼容格式而保留的；这个范围的为有异常，读取时可以忽略。目前只有一个标志是有定义的，其他的都必须为0，（第16位：如果数据有OIDS则为1，如果没有则为0）。
    * 头部扩展区域——32位整数，头部余下的字节长，不包括自身。目前，这里是0，随后跟第一个元组。将来的格式变化可能会允许头部保留额外的数据。如果读取时不知道该对头部扩展区域做什么时，应该悄悄地跳过这一区域。头部扩展区域本来是要包含身份识别块序列的。标志字段不是用来告诉读取程序头部扩展区域的内容的。头部扩展的内容的具体设计保留给后续的发布。

   * TUPLES——每条元组都以一个 16 位整数计数开头，该计数是元组中字段的数目。（目前，在一个表里的每条元组都有相同的计数，但可能不会永远这样。）然后后面不断出现元组中的各个字段，字段先是一个 32 位的长度字，后面跟着那么长的字段数据。（长度字并不包括自己，并且可以为零。）一个特例是：-1 表示一个 NULL 字段值。在 NULL 情况下，后面不会跟着数值字节。

     在数据域之间没有对齐填充或者任何其它额外的数据。

     目前，一个 COPY BINARY 文件里的所有数据值都假设是二进制格式的（格式代码为1）。预计将来的扩展可能增加一个头域，允许为每个字段声明格式代码。

     如果在文件中包括了 OID，那么该 OID 域立即跟在字段计数字后面。它是一个普通的字段，只不过它没有包括在字段计数。但它包括长度字——这样就允许我们不用花太多的劲就可以处理4字节和8字节的 OID，并且如果允许OID是可选的话，那么还可以把 OID 显示成空。

   * 文件尾——文件尾包括保存着-1的一个16位整数字。这样就很容易与一条元组的域计数字相区分。如果一个域计数字既不是-1也不是预期的字段的数目，那么读者应该报错。这样就提供了对丢失与数据的同步的额外的检查。

   **示例**

   使用竖线|作为字段分隔符来把一个表拷贝到客户端：

   COPY country TO STDOUT WITH DELIMITER '|';

   把以A开头的国家名拷贝到一个文件中：

   COPY (SELECT * FROM country WHERE country_name LIKE 'A%') TO

   '/home/usr1/sql/a_list_countries.copy';

   创建一个名为err_sales的错误表来使用但行错误隔离模式：

   CREATE TABLE err_sales ( cmdtime timestamptz, relname text,filename text, linenum int, bytenum int, errmsg text, rawdata text, rawbytes bytea ) DISTRIBUTED RANDOMLY;

   用单行错误隔离模式拷贝一个文件的数据到sales表中：

   COPY sales FROM '/home/usr1/sql/sales_data' LOG ERRORS INTO err_sales SEGMENT REJECT LIMIT 10 ROWS;

   **兼容性**

   SQL标准中没有COPY语句。

   **参见**

   CREATE EXTERNAL TABLE

2.32 CREATE AGGREGATE
----------------------

   定义一个新的聚集函数。

   **语法**

   CREATE [ORDERED] AGGREGATE name (input_data_type [ , ... ])

   ( SFUNC = sfunc,

   STYPE = state_data_type

   [, PREFUNC = prefunc]

   [, FINALFUNC = ffunc]

   [, INITCOND = initial_condition]

   [, SORTOP = sort_operator] )

   **描述**

   CREATE AGGREGATE定义一个新的聚集函数。一些基础常用的聚集函数比如count, min, max, sum, avg等Greenplum数据库已经提供了。如果你需要定义一个新类型或需要一个还没有提供的聚集函数，这时便可用 CREATE AGGREGATE 来提供我们所需要的特性。

   一个聚集函数是用它的名字和输入数据类型来标识的。 同一模式中如果两个聚集处理的输入数据不同，它们可以有相同的名字。 一个聚集函数的输入数据类型必须和所有同一模式中的普通函数的名字和输入类型不同。

   一个聚集函数是由一个、两个或三个普通函数组成的（这些普通函数必须是IMUUTABLE函数）：

   * 一个状态转换函数sfunc

   * 一个可选的初始的segment级的计算函数 prefunc

   * 一个可选的终计算函数 ffunc

   这些函数可以这样使用：

   sfunc( internal-state, next-data-values ) ---> next-internal-state

   prefunc( internal-state, internal-state ) ---> next-internal-state

   ffunc( internal-state ) ---> aggregate-value

   你可以指定PREFUNC做为优化聚集函数执行的方法。通过指定PREFUNC，聚集函数会首先在segment上并行执行，然后在master上执行。两级执行运行时，SFUNC在segment上执行，以生成初始聚集结果，PREFUNC在master上执行，用来聚集来自segment的初始结果。一级执行运行时，所有行会发送给master，然后sfunct会处理这些行。

   一级执行和两级执行在执行策略上是相当的。两种聚集都可以在一个查询计划中实现。在实现prefunc和sfunc函数时，要确保先在segment上调用sfunc后在master上调用prefunc产生的结果和把所有行发送给master然后执行sfunc函数的一级聚集的结果相同。

   Greenplum数据库创建一个类型为 stype的临时变量，它保存这个聚集的当前内部状态。 对于每个输入数据条目， 聚集参数值都会被计算，用当前的状态值和新的参数值去调用状态转换函数，以计算内部状态值的新数值。 在处理完所有数据后，调用一次最终处理函数以计算聚集的返回值。 如果没有最终处理函数，那么将最后的状态值当做返回值。

   一个聚集函数还可以提供一个可选的初始条件，也就是说，所用的该内部状态值的初始值。 这个值是作为一个类型为 text 的字段存储在数据库里的，不过它们必须是状态值数据类型的合法的外部表现形式的常量。 如果没有提供状态，那么状态值初始化为 NULL。

   如果该状态转换函数被定义为 "strict"， 那么就不能用 NULL 输入调用它。这个时候，带有这样的转换函数的聚集执行起来的现象如下所述。 NULL 输入的值被忽略（不调用此函数并且保留前一个状态值）。如果初始状态值是 NULL，那么由第一个非 NULL 值替换该状态值， 而状态转换函数从第二个非 NULL 的输入值开始调用。这样做让我们比较容易实现象 max 这样的聚集。 请注意这种行为只是当 state_data_type 与 input_data_type 相同的时候才表现出来。 如果这些类型不同，你必须提供一个非 NULL 的初始条件或者使用一个非strict的状态转换函数。

   如果状态转换函数不是 strict（严格）的， 那么它将无条件地为每个输入值调用， 并且必须自行处理 NULL 输入和 NULL 转换值， 这样就允许聚集的作者对聚集中的空值有完全的控制。

   如果终转换函数定义为"strict"，则如果最终状态值是 NULL 时就不会调用它； 而是自动输出一个NULL的结果。（当然，这才是 strict 函数的正常特征。） 不管是那种情况，终处理函数可以选择返回 NULL。比如， avg 的终处理函数在零输入记录时就会返回 NULL。

   行为类似 MIN 或者 MAX 的聚集有时候可以优化为使用索引， 而不用扫描每个输入行。如果这个聚集可以如此优化，则用一个排序操作符标识它。 这里基本的要求是聚集必须以操作符归纳出来的排序顺序生成第一个元素；换句话说

   SELECT agg(col) FROM tab;

   必须等于

   SELECT col FROM tab ORDER BY col USING sortop LIMIT 1;

   更多的假设是聚集忽略空值输入，并且只有在输入没有非空的数值的时候，它才生成空值结果。通常，数据类型的 < 操作符是 MIN 的适用排序操作符， 而 > 是 MAX 的适用操作符。请注意， 除非声明的操作符是 btree 索引操作符表（opclass）的"小于"或者"大于"策略号， 否则这种优化将不会生效。

   排序聚集

   如果出现可选的条件ORDERED，那么创建的聚集函数是一个排序聚集。在这种情况下，初始聚集函数prefunc不能指定。

   用如下的语法形式来调用排序聚集函数：

   name ( arg [ , ... ] [ORDER BY sortspec [ , ...]] )

   如果可选的ORDER BY被忽略，会使用系统定义的排序。在一个segment上，用指定顺序的调用输入参数聚集函数的转换函数。pg_aggregate表中有一个新列aggordered，这一列用来表示聚集函数被定义为排序函数。

   **参数**

   name

   聚集函数的名字。

   input_data_type

   本聚集函数要处理的基本数据类型。对于0参数的聚函数集来说，在输入数据类型列表上写上“*”。count(*)就是这样的一个函数。

   sfunc

   用于处理源数据列里的每一个输入数据的状态转换函数名称。对一个N参的聚集函数来说，sfunc必须有N+1个参数，第一个参数的类型是 state_data_type ，其余的参数匹配聚集函数声明的输入数据类型。此函数必须返回一个类型为 state_data_type的值。这个函数接受当前状态值和当前输入数据条目，而返回下个状态值。

   state_data_type

   聚集的状态值的数据类型。

   perfunc

   初始聚集函数的名字。这是一个双参数的函数，两者的类型都是state_data_type。它必须返回一个state_data_type类型的值。一个初始函数用两个转换状态值，返回一个新的转换状态值，这个值代表联合聚集。在Greenplum数据库中，如果一个聚集函数的结果是以segmented方式被计算的，那么初始聚集函数就会在单一内部状态上调用，以便把它们联合成一个最终的内部状态。

   注意这个函数在一个segment中会以哈希聚集模式调用。因此，如果你在没有初始函数的情况下调用这个函数，哈希聚集是绝不会被选的。因为哈希聚集只在定义了初始函数的情况下才有效。

   ffunc

   在转换完所有输入域/字段后调用的最终处理函数，它计算聚集的结果。 此函数必须接受一个类型为 state_data_type 的参数。 聚集的输出数据类型被定义为此函数的返回类型。如果没有声明 ffunc，则使用聚集结果的状态值作为聚集的结果，而输出类型为 state_data_type。

   initial_condition

   状态值的初始设置（值）。它必须是一个数据类型 state_data_type 可以接受的文本常量值。如果没有声明，状态值初始为 NULL。

   sort_operator

   用于 MIN 或者 MAX 类型的聚集的相关的排序操作符。 这个只是一个操作符名（可以有模式修饰）。这个操作符假设接受和聚集函数（这个聚集函数必须是单个参数的聚集函数）一样的输入数据类型。

   **注解**

   用来定义聚集函数的普通函数必须提前定义。注意在这次发布的Greenplum数据库中，用来定义聚集函数的sfunc、ffunc和prefunc函数必须被定义为IMMUTABLE。

   如果要将用户自定义的聚集函数用在窗口表达式中，那么这个聚集函数必须有prefunc函数。

   如果Greenplum数据库的服务器配置参数gp_enable_multiphase_agg是off，那么只有一级聚集会执行。

   在Greenplum数据库集群中（master和所有的segment上）任何自定义函数的编译代码（共享库文件）必须在同一个位置。这个位置必须在LD_LIBRARY_PATH中，以便服务器能够定位到这些文件。

   **示例**

   下列示例创建一个计算两列之和的聚集函数。

   在创建聚集函数之前，创建两个函数，分别用作SFUNC和PREFUNC函数。

   这个函数在聚集函数中被用作SFUNC。

   CREATE FUNCTION mysfunc_accum(numeric, numeric, numeric)

   RETURNS numeric

   AS 'select $1 + $2 + $3'

   LANGUAGE SQL

   IMMUTABLE

   RETURNS NULL ON NULL INPUT;

   这个函数在聚集函数中被用作PREFUNC。

   CREATE FUNCTION mypre_accum(numeric, numeric )

   RETURNS numeric

   AS 'select $1 + $2'

   LANGUAGE SQL

   IMMUTABLE

   RETURNS NULL ON NULL INPUT;

   这个CTEATE AGGREGATE命令创建计算两列之和的聚集函数。

   CREATE AGGREGATE agg_prefunc(numeric, numeric) (

   SFUNC = mysfunc_accum,

   STYPE = numeric,

   PREFUNC = mypre_accum,

   INITCOND = 0 );
   
   下列命令创建一个表，插入一些行，并且运行聚集函数。

   create table t1 (a int, b int) DISTRIBUTED BY (a);

   insert into t1 values

   (10, 1),

   (20, 2),

   (30, 3);

   select agg_prefunc(a, b) from t1;

   这个EXPLAIN命令展示了两阶段聚集。

   explain select agg_prefunc(a, b) from t1;

   QUERY PLAN

   --------------------------------------------------------------------------

   Aggregate (cost=1.10..1.11 rows=1 width=32)

   -> Gather Motion 2:1 (slice1; segments: 2) (cost=1.04..1.08 rows=1

   width=32)

   -> Aggregate (cost=1.04..1.05 rows=1 width=32)

   -> Seq Scan on t1 (cost=0.00..1.03 rows=2 width=8)

   (4 rows)

   **兼容性**

   CREATE AGGREGATE是Greenplum数据库的语言扩展。SQL标准中不提供用户自定义聚集函数的功能。

   **参见**

   ALTER AGGREGATE， DROP AGGREGATE, CREATE FUNCTION

2.33 CREATE CAST
-----------------

   定义一个转换。

   **语法**

   CREATE CAST (sourcetype AS targettype)

   WITH FUNCTION funcname (argtypes)

   [AS ASSIGNMENT | AS IMPLICIT]

   CREATE CAST (sourcetype AS targettype) WITHOUT FUNCTION

   [AS ASSIGNMENT | AS IMPLICIT]

   **描述**

   CREATE CAST定义一个新的转换。转换说明了如何在两种数据类型之间转换。

   比如：

   SELECT CAST(42 AS text);

   通过调用一个预先定义的函数把整数常量42转换成text类型，这个例子中的函数是text(int4)。如果没有定义合适的转换，这里的转换会失败。

   两种类型可以是二进制兼容的，这意味着它们可以在不调用函数的情况下相互转换。这要求对应的值使用相同的内部表示。比如，text和varchar是二进制兼容的。

   默认情况下，只有显式请求转换时，才会调用一个转换，也就是有显式的CAST(x AS typename)或x::typename结构。

   如果转换被标记为AS ASSIGNMENT，在给类型为目标数据类型的列赋值的时候会隐式调用这个转换。比如，假设foo.f1是一个text类型的列，那么：

   INSERT INTO foo(f1) VALUES (42);

   会被允许执行，如果从integer到text类型的转换被标记为AS ASSIGNMENT，否则不允许执行。术语赋值转换一般用来描述这种转换。

   如果转换被标记为AS IMPLICIT，那么在任何上下文中都可以隐式调用，无论是赋值还是内部表达式。术语隐式转换一般用来描述这种转换。比如，因为||需要text操作数，只有在从timestamp到text的转换被标记为AS IMPLICIT的时候，

   SELECT ‘The time is ‘ || now();

   才会允许执行。否则，显式写出转换就是必须的了，比如

   SELECT ‘The time is ‘ || CAST(now() AS text);

   保守一些把转换标记为隐式的是一个明智的做法。过多的隐式转换路径会导致Greenplum数据库选择令人惊讶的命令解释，或者因为有太多可能的解释而根本无法解析命令。一个好的经验是，只有在同一个通用类型表里面的那些可以保留转换信息的类型之间才标记为可隐含调用转换。比如，从int2到int4的转换可以合理地标记为隐式转换，但是从float8到int4的转换可能应该只标记为赋值转换。跨类型表的转换，比如text到int4，最好做成只能显式转换。

   要想创建一个转换，你必须拥有源或目的数据类型。要创建一个二进制兼容的转换，你必须是超级用户。

   **参数**

   sourcetype

   转换的源数据类型。

   targettype

   转换的目标数据类型。

   funcname(argtypes)

   用于执行转换的函数。这个函数名可以是用模式名修饰的。 如果它没有用模式名修饰，那么该函数将从模式搜索路径中找出来。 函数的结果数据类型必须匹配转换的目标类型。

   转换实现函数可以有一个到三个的参数。第一个参数的类型必须和转换的源数据类型完全相同。第二个参数，如果有，那么必须为integer；它接受和目标类型关联的类型修饰符，或-1，如果没有类型修饰符。第三个参数，如果有，必须是boolean类型；如果转换是显式转换，它接受true，否则它接受false。在某型情况下，SQL规范需要显式和隐式转换的不同行为。这个参数为必须实现这种转换的函数提供。不推荐你用这种方式设计你自己的数据类型。

   一般而言，一个转换必须有不同的源和目的数据类型。如果一个转换的转换函数有多于一个的参数，那么这个转换可以用完全相同的源和目标数据类型。这被用来在系统分类中表示特定类型的长度强制函数。这个命名函数用来强制一个这种类型的值到第二个参数给定的类型的值。（因为目前的语法只允许某些内置数据类型有类型修饰符，所以这个特性对用户自定义的目标类型无效。）

   如果一个转换有不同的源和目标数据类型，同时有多于一个参数的转换函数，它表示把一种数据类型转换为另一种数据类型，在一步中应用长度强制。如果没有这样的条目可用，用类型描述符强制转换一种类型需要两步，第一步在两种数据类型之间转换，第二步应用类型转换符。

   WITHOUT FUNCTION

   表示源类型和目标类型是二进制兼容的，所以不需要函数来执行转换。

   AS ASSIGNMENT

   表示在赋值上下文中，转换可以隐式调用。

   AS IMPLICIT

   表示转换可以在任何上下文中隐式调用。

   **注解**

   注意在这次Greenplum数据库的发布中，用在用户自定义转换中的用户自定义函数必须被定义为IMMUTABLE的。自定义函数的任何编译代码（共享库文件）必须放在Greenplum数据库集群（master和所有segment）中每台主机的相同位置。这个位置必须在LD_LIBRARY_PATH中，以便服务器能够找到这些文件。

   记住如果你想用两种方式来转换类型，那么你必须显式用两种方式声明转换。

   推荐你遵循这样的约定：在目标数据类型之后命名转换实现函数，就是内置转换实现函数那样命名。很用用户习惯于用函数风格的表示法来转换数据的类型，也就是typename(x)。

   **示例**

   创建一个转换，用int(text)函数把text转换为int4（这个转换已经在系统中预定义了。）：

   CREATE CAST (text AS int4) WITH FUNCTION int4(text);

   **兼容性**

   CREATE CAST命令符合SQL标准，但是SQl不为二进制兼容类型做准备，额不为实现函数提供额外的参数。AS IMPLICIT也是Greenplum数据库的一个扩展。

   **参见**

   CREATE FUNCTION, CREATE TYPE, DROP TYPE

2.34 CREATE CONVERSION
--------------------------

   定义一个新的编码转换。

   **语法**

   CREATE [DEFAULT] CONVERSION name FOR source_encoding TO

   dest_encoding FROM funcname

   **描述**

   CREATE CONVERSION在字符集编码之间定义一个新的转换。转换的名字可以用在转换函数中，以便指定一个特定的编码转换。标记为DEFAULT的转换可以用来在客户和服务器之间自动进行编码转换。为此，必须定义从编码A到编码B和从编码B到编码A的两个转换。

   想要定义过一个转换，你必须在函数上有EXECUTE权限，在目标模式上有CREATE权限。

   **参数**

   DEFAULT

   表示这个转换是特定的源编码向目标编码转换的默认转换。在同一个模式中的两个编码之间只能有一个默认转换。

   name

   转换的名字。这个转换的名字可以用模式名来限定。如果没有指定模式名，那么转换是定义在当前模式中的。同一个模式中的转换必须是唯一的。

   source_encoding

   源编码名字。

   dest_encoding

   目标编码名字。

   funcname

   用来执行转换的函数。函数名可以用模式名来修饰。如果没有指定模式名，会在path中搜寻函数。函数必须有如下签名：

   conv_proc(

   integer, -- source encoding ID

   integer, -- destination encoding ID

   cstring, -- source string (null terminated C string)

   internal, -- destination (fill with a null terminated C string)

   integer -- source string length

   ) RETURNS void;

   **注解**

   注意在这次Greenplum数据库的发布中，用在用户自定义转换中的用户自定义函数必须定义为IMMUTABLE。自定义函数的任何编译代码（共享库文件）必须放在Greenplum数据库集群（master和所有segment）中每台主机的相同位置。这个位置必须在LD_LIBRARY_PATH中，以便服务器能够找到这些文件。

   **示例**

   定义一个转换，用myfunc把UTF8转换成LATIN1：

   CREATE FUNCTION myconv FOR ‘UTF8’ TO ‘LATIN1’ FROM myfunc;

   **兼容性**

   SQL标准中没有CREATE CONVERSION语句。

   **参见**

   ALTER CONVERSION, CREATE FUNCTION, DROP CONVERSION

2.35 CREATE DATABASE
-----------------------

   创建一个新的数据库。

   **语法**

   CREATE DATABASE name [ [WITH] [OWNER [=] dbowner]

   [TEMPLATE [=] template]

   [ENCODING [=] encoding]

   [TABLESPACE [=] tablespace]

   [CONNECTION LIMIT [=] connlimit ] ]

   **描述**

   CREATE DATABASE创建一个新的数据库。想要创建一个新的数据库，你必须是超级用户或者有特殊的CREATE权限。

   默认情况下，创建者会成为新数据库的所有者。超级用户可以通过使用OWNER子句来创建其他人所有的数据库。他们甚至可以创建所有者是没有特殊权限的用户的数据库。有CREATEDB权限的非超级用户只能创建自己拥有的数据库。

   默认情况下，可以通过复制标准的系统数据库template1来创建数据库。通过TEMPLATE name可以另外指定模板。特别地，通过写下TEMPLATE template0，你可以创建一个仅包含Greenplum数据库预定义的标准对象的干净数据库。如果你不想要已经加到template1中的与安装有关的对象，这么做是有用的。

   **参数**

   name

   将要创建的数据库的名字。

   dbowner

   数据库用户的名字，他会拥有新的数据库，或通过DEFAULT来用默认用户（执行这个命令的用户）。

   template

   用来创建新数据库的模板数据库的名字，或者通过DEFAULT来用默认的模板（template1）。

   encoding

   新数据库中用的字符集编码。指定一个字符串常量（比如’SQL_ASCII’），一个整型编码数，或通过DEFAULT来用默认的编码。更多信息，请看字符集支持。

   tablespace

   与新数据库关联的表空间的名字，或者通过DEFAULT来使用模板数据库的表空间。这个表空间将是在此数据库中创建的对象的默认表空间。

   connlimit

   可能的最大并发连接数。默认-1，表示没有限制。

   **注解**

   CREATE DATABASE不能在一个事务块中执行。

   当你想通过指定一个数据库的名字为模板名来拷贝一个数据库时，其他的会话在拷贝期间是不能连接到这个模板数据库的。模板数据库的新连接也会被锁住，知道CREATE DATABASE完成。

   CONNCTION LIMIT必须用超级用户来指定。

   **示例**

   创建一个数据库：

   CREATE DATABASE gpdb;

   创建一个数据库sales，所有者为salesapp，默认表空间为salesspace：

   CREATE DATABASE sales OWNER salesapp TABLESPACE salesspace;

   创建一个数据库，支持ISO-8859-1字符集：

   CREATE DATABASE music ENCODING ‘LATIN1’;

   **兼容性**

   SQL标准中没有CREATE DATABASE语句。数据库和分类相当，分类的创建时有实现定义的。

   **参见**

   ALTER DATABASE, DROP DATABASE

2.36 CREATE DOMAIN
---------------------

   定义一个域。

   **语法**

   CREATE DOMAIN name [AS] data_type [DEFAULT expression]

   [CONSTRAINT constraint_name

   | NOT NULL | NULL

   | CHECK (expression) [...]]

   **描述**

   CREATE DOMAIN创建一个域。一个域基本上就是一个有可选约束的数据类型（关于允许的值集的限制）。定义域的用户成为其所有者。域名字必需在其所在模式中的现有类型和域中唯一。域可以便于我们把不同表之间的公共域抽取到一个位置进行维护。比如，一个电子邮件地址字段可能在多个表中使用，所有的列都需要相同的CHECK约束来检查地址的语法。 更容易的办法是我定义并使用一个域而不是分别设置每个还有邮件列的表的列约束。

   **参数**

   name

   将要创建的域的名字，可以用模式名修饰。

   data_type

   域的下层数据类型。它可以包含数组声明字。

   DEFAULT expression

   DEFAULT 子句为域数据类型的字段声明一个缺省值。 该值是任何不含变量的表达式（但不允许子查询）。 缺省表达式的数据类型必需匹配域的数据类型。如果没有声明缺省值， 那么缺省值就是空值。

   缺省表达式将用在任何不为该字段声明数值的插入操作。 如果为特定的字段声明了缺省值，那么它覆盖任何和该域相关联的缺省值。 反过来，域的缺省覆盖任何与下层数据类型相关的缺省。

   CONSTRAINT constraint_name

   一个约束的可选名称。如果没有声明，系统生成一个名字。

   NOT NULL

   这个域的数值不允许为 NULL。

   NULL

   这个域的数值允许为空。它是缺省情况。这个子句只是用于和非标准的 SQL 数据库兼容用。 我们不建议在新的应用中使用它。

   CHECK (expression)

   CHECK 子句声明完整性约束或者是测试，域的数值必须满足这些测试。 每个约束必须是一个生成一个布尔结果的表达式。它应该使用关键字 VALUE 来引用被测试的数值。目前，CHECK表达式既不能包含子查询也不能引用除了VALUE之外的变量。

   **示例**

   CREATE DOMAIN us_zip_code AS TEXT 

   CHECK ( VALUE ~ '^\\d{5}$' OR VALUE ~ '^\\d{5}-\\d{4}$' );

   **兼容性**

   CREATE DOMAIN符合SQL标准。

   **参见**

   ALTER DOMAIN, DROP DOMAIN

2.37 CREATE EXTERNAL TABLE
------------------------------

   定义一个新的外部表。

   **语法**

   CREATE [READABLE] EXTERNAL TABLE table_name

   ( column_name data_type [, ...] | LIKE other_table )

   LOCATION ('file://seghost[:port]/path/file' [, ...])

   | ('gpfdist://filehost[:port]/file_pattern[#transform]'

   | ('gpfdists://filehost[:port]/file_pattern[#transform]'

   
   [, ...])

   | ('gphdfs://hdfs_host[:port]/path/file')

   FORMAT 'TEXT'

   [( [HEADER]

   [DELIMITER [AS] 'delimiter' | 'OFF']

   [NULL [AS] 'null string']

   [ESCAPE [AS] 'escape' | 'OFF']

   [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']

   [FILL MISSING FIELDS] )]

   | 'CSV'

   [( [HEADER]

   [QUOTE [AS] 'quote']

   [DELIMITER [AS] 'delimiter']

   [NULL [AS] 'null string']

   [FORCE NOT NULL column [, ...]]

   [ESCAPE [AS] 'escape']

   [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']

   [FILL MISSING FIELDS] )]

   | 'AVRO'

   | 'PARQUET'

   | 'CUSTOM' (Formatter=<formatter specifications>)

   [ ENCODING 'encoding' ]

   [ [LOG ERRORS [INTO error_table]] SEGMENT REJECT LIMIT count

   [ROWS | PERCENT] ]

   CREATE [READABLE] EXTERNAL WEB TABLE table_name

   ( column_name data_type [, ...] | LIKE other_table )

   LOCATION ('http://webhost[:port]/path/file' [, ...])

   | EXECUTE 'command' [ON ALL

   | MASTER

   | number_of_segments

   | HOST ['segment_hostname']

   | SEGMENT segment_id ]

   FORMAT 'TEXT'

   [( [HEADER]

   [DELIMITER [AS] 'delimiter' | 'OFF']

   [NULL [AS] 'null string']

   [ESCAPE [AS] 'escape' | 'OFF']

   [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']

   [FILL MISSING FIELDS] )]

   | 'CSV'

   [( [HEADER]

   [QUOTE [AS] 'quote']

   [DELIMITER [AS] 'delimiter']

   [NULL [AS] 'null string']

   [FORCE NOT NULL column [, ...]]

   [ESCAPE [AS] 'escape']

   [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']

   [FILL MISSING FIELDS] )]

   | 'CUSTOM' (Formatter=<formatter specifications>)

   [ ENCODING 'encoding' ]

   [ [LOG ERRORS [INTO error_table]] SEGMENT REJECT LIMIT count

   [ROWS | PERCENT] ]

   CREATE WRITABLE EXTERNAL TABLE table_name

   ( column_name data_type [, ...] | LIKE other_table )

   LOCATION('gpfdist://outputhost[:port]/filename[#transform]'

   | ('gpfdists://outputhost[:port]/file_pattern[#transform]'

   [, ...])

   | ('gphdfs://hdfs_host[:port]/path')

   FORMAT 'TEXT'

   [( [DELIMITER [AS] 'delimiter']

   [NULL [AS] 'null string']

   [ESCAPE [AS] 'escape' | 'OFF'] )]

   | 'CSV'

   [([QUOTE [AS] 'quote']

   [DELIMITER [AS] 'delimiter']

   [NULL [AS] 'null string']

   [FORCE QUOTE column [, ...]] ]

   [ESCAPE [AS] 'escape'] )]

   | 'AVRO'

   | 'PARQUET'

   | 'CUSTOM' (Formatter=<formatter specifications>)

   [ ENCODING 'write_encoding' ]

   [ DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY ]

   CREATE WRITABLE EXTERNAL WEB TABLE table_name

   ( column_name data_type [, ...] | LIKE other_table )

   EXECUTE 'command' [ON ALL]

   FORMAT 'TEXT'

   [( [DELIMITER [AS] 'delimiter']

   [NULL [AS] 'null string']

   [ESCAPE [AS] 'escape' | 'OFF'] )]

   | 'CSV'

   [([QUOTE [AS] 'quote']

   [DELIMITER [AS] 'delimiter']

   [NULL [AS] 'null string']

   [FORCE QUOTE column [, ...]] ]

   [ESCAPE [AS] 'escape'] )]

   | 'CUSTOM' (Formatter=<formatter specifications>)

   [ ENCODING 'write_encoding' ]

   [ DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY ]

   **描述**

   关于外部表的详细信息，请参考《Greenplum数据库管理员指南》中的“加载和卸载数据”。

   CREATE EXTERNAL TABLE或者CREATE EXTERNAL WEB TABLE在Greenplum数据库中创建一个可读外部表的定义。可读外部表经常用来快速、并行加载数据。一旦定义了外部表，你可以直接用SQL命令并行查询数据。比如，你可以查询、联合或排序外部表的数据。你也可以为外部表创建视图。DML操作（UPDATE,INSERT, DELETE, 或者TRUNCATE）不允许在外部表上操作，也不能为可读外部表创建索引。

   CREATE WRITABLE EXTERNAL TABLE或者CREATE WRITABLE EXTERNAL WEB TABLE在Greenplum数据库中创建一个可写外部表的定义。可写外部表通常用来从数据库中卸载数据到一组文件中或者命名管道中。可写的外部网络表也可以用来输出数据到一个可执行程序。可写外部表也可以用作Greenplum并行MapReduce计算的输出目标。一旦定义了一个可写外部表，就可以从数据库表中选取数据，然后插入到可写外部表中。可写外部表仅允许INSERT操作，SELECT, UPDATE, DELETE或TRUNCATE是不允许的。

   常规外部表和网络外部表的最大区别是它们的数据源。常规可读外部表访问静态的普通文件，而网络外部表访问动态数据源——一个web服务器或通过执行OS命令或脚本。

   FORMAT子句用来描述外部表是如何格式化的。有效的文件格式是分割的文本（TEXT）和CSV格式，这和PostgreSQL中的COPY命令的格式选项类似。如果文件中的数据没有使用默认的列分隔符，转义字符，null字符串等，你必须指定其他的格式选项，以便外部文件中的数据被Greenplum数据库正确读取。关于使用自定义格式的信息，参见《Greenplum数据库管理员指南》中的“加载和卸载数据”。

   对于gphdfs协议，你可以在FORMAT子句中指定AVRO或PARQUET，以读写Avro或Parquet格式的文件。关于Avro和Parquet格式的文件的更多信息，请参考HDFS文件格式对gphdfs协议的支持。

   **参数**

   READABLE | WRITABLE

   指定外部表的类型，默认是可读的。可读外部表用来加载输入进入Greenplum数据库。可写外部表用来卸载数据。

   WEB

   在Greenplum数据库中创建可读或可写的网络外部表。有两种格式的可读网络外部表——通过http://一些访问文件的网络外部表和通过执行操作系统命令来访问数据的网络外部表。可写网络外部表输出数据到一个可接受输入数据流的可执行程序。网络外部表在查询执行期间是不可以再扫描的。

   table_name

   新外部表的名字。

   column_type

   外部表定义中将要创建的列的名字。不像常规的表，外部表不能有列约束或默认值，因此不需要指定这些。

   LIKE other_table

   LIKE子句指定一个表，新的外部表会自动从这个表中拷贝所有的列名，数据类型和Greenplum分布策略。如果原始表声明了列的约束或列的默认值，那么这些不会被拷贝到新的外部表的定义中。

   data_type

   列的数据类型。

   LOCATION (‘protocol://host[:port]/path/file’ [, …])

   对可读外部表，指定外部数据源的URI，用来存放外部表或网络表。常规的可读外部表允许gpfdist或file协议。网络外部表允许http协议。如果省略port，http和gpfdist协议的默认端口是8080，gphdfs协议的默认端口是9000。如果使用gpfdist协议，path是相对于gpfdist服务的目录的（当你开启gpfdist程序的时候指定的目录）。gpfdist也可以使用通配符（或C风格的模式匹配）以表示同一个目录中的多个文件。比如：

   'gpfdist://filehost:8081/*'

   'gpfdist://masterhost/my_load_file'

   'file://seghost1/dbfast1/external/myfile.txt'

   'http://intranet.mycompany.com/finance/expenses.csv'

   如果你在用gphdfs协议使用MapR集群，你可以指定一个特定的集群和文件：

   * 想指定默认的集群，在MapR配置文件/opt/mapr/conf/mapr-clusters.conf的第一个条目中用如下语法指定你表的位置：

     LOCATION ('gphdfs:///file_path')

     file_path是文件的路径。

   * 想指定列在配置文件中的另一个MapR集群，用如下的语法指定文件：

     LOCATION ('gphdfs:///mapr/cluster_name/file_path')

     cluster_name是配置文件中指定的集群的名字，file_path是文件的路径。

   对可写外部表而言，指定gpfdist进程的URI位置，这会从Greenplum segment中收集输出数据，并把数据写入到命名文件中。path是一个相对路径，相对于gpfdist提供文件的目录（开启gpfdist程序时指定的目录）。如果列出多个gpfdist位置，发送数据的segment会在可用的输出位置平均分配。比如：

   'gpfdist://outputhost:8081/data1.out',

   'gpfdist://outputhost:8081/data2.out'

   如上示例列出了两个gpfdist位置，一半的segment会发送它们的输出数据到data1.out文件，另一半segment会发送它们的输出数据到data2.out文件。

   如果你用gpfdist协议读写文件到HDFS，你可以通过指定带AVRO或PARQUET的FORMAT子句来读取或写入Avro或Parquet格式的文件。

   当指定Avro或Parquet文件位置的选项的信息，请参考HDFS对gphdfs协议的文件格式支持。

   EXECUTE ‘command’ [ON …]

   只对可读网络外部表或可写外部表允许。对可读网络外部表而言，是指定被segment实例执行的操作系统命令。这个command可以是一条操作系统命令或者一个脚本。ON子句用来指定执行给定命令的segment实例。

   * ON ALL是默认情况。这条命令会被Greenplum数据库系统中的所有的segment主机上的每个活跃（主要的）segment实例执行。如果这个命令执行一个脚本，这个脚本在所有的segment主机上必须位于相同的位置，而且也必须是Greenplum超级用户（gpadmin）可执行的。

   * ON MASTER只在master主机上运行这个命令。

     注意：当使用ON MASTER子句的时候，网络外部表不支持日志。

   * ON number意味着这个命令会被指定数量的segment执行。Greenplum数据库系统会在运行时随机选取特定的segment。如果这个命令执行一个脚本，这个脚本在所有的segment主机上必须位于相同的位置，而且也必须是Greenplum超级用户（gpadmin）可执行的。

   * HOST意味着每个segment主机上将会有一个segment实例来执行这条命令，不管segment主机上有多少个活跃的segment实例。

   * HOST segment_hostname意味着这条命令会被指定的segment主机上的所有活跃segment实例执行。

   * SEGMENT segment_id意味着这条命令会被指定的segment执行一次。查看系统分类表gp_segment_configuration中的content数字，可以确定一个segment实例的ID。Greenplum数据库的master的contentID永远是-1。

     对可写外部表，在EXECUTE子句中指定的命令必须准备接收流入它的数据。因为所有要发送数据的segment会把它们的输出写到指定的命令或程序，所以ON子句中唯一可用的选项使ON ALL。

   FORMAT ‘TEXT | CSV | AVRO | PARQUET’ (options)

   指定外部或网络表数据的格式——普通文本（TEXT）或CSV格式。

   AVRO和PARQUET格式只在gphdfs协议中支持。

   关于指定AVRO和PARQUET文件格式的选项的更多信息，请参考HDFS对gphdfs协议的文件格式支持。

   DELIMITER

   指定一个ASCII字符，它会在每行之内分割列。默认情况下，TEXT模式是tab字符，CSV模式是逗号。文本模式的可读外部表，特殊情况下，可以将分隔符设置为OFF，这时非结构化的数据会被加载到表的一列中。

   NULL

   指定代表NULL值的字符串。默认情况下，文本模式是\N（反斜杠-N），CSV模式是没有引号的空值。在TEXT模式中，如果不想区分NULL值和空字符串的时候，可以用空字符串。当使用外部表和网络表的时候，任何匹配这个字符串的数据项将会被认为是NULL值。

   text格式的一个例子，FORMAT子句用来指定两个单引号的（’’）字符串是一个NULL值。

   FORMAT 'text' (delimiter ',' null '\'\'\'\'' )

   ESCAPE

   指定一个字符，用来做为C转义序列（比如\n, \t, \100等）或用来转义被用作行或列分隔符的数据字符。请确保你选的转义字符没有在实际数据中使用。默认情况下，文本格式的文件中转义字符是\（反斜线），csv格式文件中是“（双引号），当然可以设置另外的字符来代表转义字符。也可以把转义值设为OFF来关闭文本文件中的转义。这对文本格式的网络日志数据时非常有用的，网络日志数据中有很多不用转义的反斜线。

   NEWLINE

   执行数据文件中的换行符——LF（换行, 0x0A）,CR（回车，0x0D）或者CRLF（回车加换行，0x0D 0x0A）。如果不指定，Greenplum数据库segment会在接收到的数据的第一行检测换行类型，会将它遇到的第一个换行类型作为换行符。

   HEADER

   对可读外部表，指定数据文件的第一行为头部行（包含列名），不应该被当成数据。如果有多个数据源文件，所有的文件都必须有一个头部行。

   QUOTE

   指定CSV模式的引号字符。默认是双引号（”）。

   FORCE NOT NULL

   在CSV模式中，给每一列加上引号，这样就没有NULL值了。对CSV模式中的默认的null字符串（在两个分隔符之间的空白）来说，这会使缺失的值被当成空字符串。

   FORCE QUOTE

   对CSV模式的可写外部表，强制所有的指定列的非NULL值加引号。NULL输出值不会加引号。

   FILL MISSING FIELDS

   对TEXT和CSV模式的可读外部表，指定FILL MISSING FIELD时，当一行的行尾缺失数据项时，会把缺失数据的尾部列的值设置为NULL（而不是报错）。

   ENCODING ‘encoding’

   外部表的字符集编码。指定一个字符串常量（比如’SQL_ASCII’），一个整数编码数字，或DEFAULT以便使用默认的客户端编码。参见字符集支持。

   LOG ERRORS [INTO error_table]

   这是一个可选子句，能放在SEGMENT REJECT LIMIT子句之前，用来记录有格式错误的行的信息。可选子句INTO error_table子句指定一个错误表，用来记录运行在单行错误隔离模式的有格式错误的行的信息。

   如果不指定INTO error_table子句，错误日志信息会存在内部（而不是存在错误表中）。存储在内部的错误日志信息可以用Greenplum数据库内置的SQL函数gp_read_error_log()来访问。

   如果指定的错误表已经存在了，那么就会直接使用它。如果不存在，就会创建它。如果错误表存在，但是没有随机分布（在创建表的时候没有指定DISTRIBUTED RANDOMLY子句），会返回一个错误。

   关于错误日志信息和查看和管理错误日志信息的内置函数，请参考注解。

   这个错误表不能用ALTER TABLE命令来修改。

   注意：可选子句INTO error_table已经过时，下一个主要分布中将不会再支持。只有内部错误日志会被支持。

   SEGMENT REJECT count [ROWS | PERCENT]

   在单行错误隔离模式运行一个COPY FROM操作。假如在加载操作期间拒绝限值在每一个Greenplumsegment实例上都没有达到，那么有格式错误的行会被丢弃。拒绝限值可以指定为行数（默认情况），或者总行数的百分比（1-100）。如果使用PERCENT，每个segment只会在超过参数gp_reject_percent_threshold指定的行数时开始计算坏行的百分比。gp_reject_percent_threshold的默认值是300行。像违反NOT NULL，CHECK，UNIQUE约束等约束错误依然会在all-and-nothing输入模式处理。如果没有超过限值，所有格式良好的行会被加载，而含有错误的行会被丢弃。

   注意：当读取外部表时，如果SEGMENT REJECT LIMIT没有被首先触发或者没有指定，Greenplum数据库会限制有格式错误的行的初始数量。如果最先的100行被拒绝，COPY操作会停止并回退。

   可以用Greenplum数据库服务器配置参数gp_initial_bad_row_limit修改初始的拒绝行数。关于这个参数的信息参见服务器配置参数。

   DISTRIBUTED BY (column, […])

   DISTRIBUTED RANDOMLY

   为可写外部表声明Greenplum数据库分布式策略。默认情况下，可写外部表时随机分布的。如果数据源表有哈希分布策略，为可写外部表定义相同的列分布会提升卸载性能，因为需要交叉移动行的情况减少了。当你发出一个卸载命令，比如INSERT INTO wex_table SELECT * FROM source_table，如果两个表有相同的哈希分布策略，卸载的行会被直接发到segment的输出位置。

   **示例**

   开启gpfdist文件服务程序后台运行，端口号8081，服务文件放在/var/data/staging:

   gpfdist -p 8081 -d /var/data/staging -l /home/gpadmin/log &

   用gpfdist协议和gpfdist目录中的全部文本文件创建一个可读外部表，命名为ext_customer。文件用管道符号|作为列分隔符，用空白作为NULL。在单行错误隔离模式访问外部表：

   CREATE EXTERNAL TABLE ext_customer

   (id int, name text, sponsor text)

   LOCATION ( 'gpfdist://filehost:8081/*.txt' )

   FORMAT 'TEXT' ( DELIMITER '|' NULL ' ')

   LOG ERRORS INTO err_customer SEGMENT REJECT LIMIT 5;

   创建一个和上面的定义一样的外部表，但是用CSV格式的文件：

   CREATE EXTERNAL TABLE ext_customer

   (id int, name text, sponsor text)

   LOCATION ( ‘gpfdist://filehost:8081/*.csv’ )

   FORMAT ‘CSV’ ( DELIMITER ‘,’);

   用file协议和几个有头部行的CSV格式文件创建一个可读外部表，命名为ext_expenses：

   CREATE EXTERNAL TABLE ext_expenses (name text, date date, amount float4, category text, description text)

   LOCATION (

   'file://seghost1/dbfast/external/expenses1.csv',

   'file://seghost1/dbfast/external/expenses2.csv',

   'file://seghost2/dbfast/external/expenses3.csv',

   'file://seghost2/dbfast/external/expenses4.csv',

   'file://seghost3/dbfast/external/expenses5.csv',

   'file://seghost3/dbfast/external/expenses6.csv'

   )

   FORMAT ‘CSV’ ( HEADER’);

   创建一个可读网络外部表，它会在每个segment主机上执行一段脚本：

   CREATE EXTERNAL WEB TABLE log_output (linenum int, message text) EXECUTE ‘/var/load_scripts/get_log_data.sh’ ON HOST 

   FORMAT ‘TEXT’ ( DELIMITER ‘|’);

   创建一个可写外部表，命名为sales_out，它会用gpfdist把输出数据写入到一个名为sales.out的文件中。这个文件用管道符号|作为列分隔符，用空白作为NULL：

   CREATE WRITABLE EXTERNAL TABLE sales_out (LIKE sales)

   LOCATION ('gpfdist://etl1:8081/sales.out')

   FORMAT 'TEXT' ( DELIMITER '|' NULL ' ')

   DISTRIBUTED BY (txn_id);

   创建一个可写外部网络表，它会把segment收到的输出数据输出到一个名为to_adreport_etl.sh的可执行脚本：

   CREATE WRITABLE EXTERNAL WEB TABLE campaign_out (LIKE campaign) 

   EXECUTE ‘/var/unload_scripts/to_adreport_etl.sh’

   FORMAT ‘TEXT’ (DELIMITER ‘|’);

   用上面定义的可写外部表卸载选定的数据：

   INSERT INTO campaign_out SELECT * FROM campaign WHERE customer_id=123;

   **注解**

   Greenplum数据库能记录错误表或内部有格式错误的行的信息。当你指定LOG ERRORS INTO error_table，Greenplum数据库会创建error_table表，用来存放在读取外部表时发生的错误。这个表可以定义如下：

   CREATE TABLE error_table_name ( cmdtime timestamptz, relname text, 

   filename text, linenum int, bytenum int, errmsg text,

   rawdata text,  rawbytes bytea ) DISTRIBUTED RANDOMLY；

   你可以用SQL命令来查看这个表的信息。

   如果没有指定INTO error_table， 错误日志数据会记录在内部：

   * 用内置SQL函数gp_read_error_log(‘table_name’)。这需要表上SELECT权限。

     这个例子展示了拷贝进ext_expenses表的数据的错误日志信息：

     SELECT * FROM gp_read_error_log(‘ext_expenses’);

     错误日志和错误表的列相同。

     如果table_name不存在，这个函数会返回FALSE。

   * 如果指定表的错误日志数据存在，新的错误日志信息会追加到已有的错误日志数据后边。错误日志信息不会复制到镜像segment。

   * 用内置SQL函数gp_truncate_error_log(‘table_name’)可以删除表table_name的错误日志数据。这需要表所有者权限。下面的例子会删除移动数据到表ext_expenses时抓取的错误日志信息：

     SELECT  gp_truncate_error_log(‘ext_expenses’);

     如果table_name不存在，这个函数会返回FALSE。

   指定通配符*字符会删除当前数据库中所有表的错误日志信息。指定*.*会删除所有数据库的错误日志信息，包括由于之前的数据库问题而没有成功删除的错误日志信息。如果指定*，则需要数据库所有者权限。如果指定*.*，则需要超级用户权限。

   HDFS对gphdfs协议的文件格式支持

   如果你指定gphdfs协议去读取或写入文件到一个HDFS，你可以读取或写入Avro或Parquet格式的文件，通过用FORMAT子句指定文件格式。

   想要读取或写入数据到Avro或Parquet文件，你可以用CREATE EXTERNAL TABLE命令创建外部表，在LOCATION子句中指定Avro文件的位置，在FORMAT子句中指定’AVRO’。这个例子是从Avro文件中读取数据的可读外部表：

   CREATE EXTERNAL TABLE tablename (column_spec) LOCATION ( ‘gphdfs://location’ ) FORMAT ‘AVRO’

   这里的location可以是一个文件名或是包含一组文件的一个目录。对文件名来说，你可以指定通配符*来匹配任意个数的字符。如果location在读取文件的时候有多个文件，Greenplum数据库会用读到的第一个文件的模式作为其他文件的模式。

   作为location参数的一部分，你可以指定读取或写入文件的选项。在文件名之后，你可以用HTTP请求字符串的格式来指定参数，HTTP请求字符串以？开头，用&分割键值对。

   下面例子中的location参数，这个URI为Avro格式可写外部表设置了压缩参数。

   'gphdfs://myhdfs:8081/avro/singleAvro/array2.avro?

   compress=true&compression_type=block&codec=snappy' FORMAT 'AVRO'

   关于用外部表读取和写入Avro和Parquet格式文件的信息，请参考《Greenplum数据库管理员指南》中的“加载和卸载数据”。

   Avro文件

   对可读外部表来说，唯一合法的参数是schema。当读取多个Avro文件时，你可以指定一个含有Avro模式的文件。参见Greenplum数据库的“Avro模式重写”。

   对可写外部表来说，你可以指定schema，namespace和压缩参数。

   表3：Avro格式外部表location参数
   
   ================================ =============================== ================================ ======================================================================
   参数                               值                               可读/可写                        默认值
   ================================ =============================== ================================ ======================================================================
   Schema                           URL_to_schema_file              读和写                           没有。对可读外部表：* 指定的模式会覆盖Avro文件中的模式。参见“Avro模式重写”。* 如果不指定，Greenplumn数据库用Avro文件的模式。对可写外部表：* 当创建Avro文件的时候使用指定的模式。* 如果不指定，KingbaseAnalyticsDB根据外部表的定义来创建一个模式。
   namespace                        Avro_namespace                  只写                             public.avro 如果指定，合法的Avro命名空间。
   compress                         true或false                     只写                             false
   compression_type                 block                           只写                             可选。对avro格式，compression_type格式必须是block如果compress是true。
   codec                            deflate或snappy                 只写                             deflate
   codec_level (只在deflate codec)  1-9之间的整数                   只写                             6 控制速度和压缩之间的平衡。合法值是1到9，你是最快，9是最高压缩。
   ================================ =============================== ================================ ======================================================================
   
   下面的参数设置指定了snappy压缩：

   'compress=true&codec=snappy'

   这两个设置指定了deflate压缩，效果相当：

   'compress=true&codec=deflate&codec_level=1'

   'compress=true&codec_level=1'

   Parquet文件

   对外部表而言，你可以在location指定的文件之后增加参数。这个表列出了合法的参数和值。

   表4：Parquet格式外部表location参数
   ============================= ================================== ============================= ================================================================= 
   选项                          值                                 可读/可写                     默认值
   ============================= ================================== ============================= =================================================================
   schema                        URL_to_schema                      只写                          没有。如果没有指定，Greenplum数据库会根据外部表的定义创建模式。
   pagesize                      >1024 Bytes                        只写                          1MB
   rowgroupsize                  >1024 Bytes                        只写                          8MB
   version                       v1，v2                             只写                          v1
   codec                         UNCOMPRESSED,GZIP, LZO, snappy     只写                          UNCOMPRESSED
   dictionaryenable              true, false                        只写                          false
   dictionarypagesize            >1024 Bytes                        只写                          512KB
   ============================= ================================== ============================= =================================================================

   注意：

   1. 创建一个内部字典。如果文本列有类似或重复的数据，打开字典会提升Parquet文件的压缩。

   **兼容性**

   CREATE EXTERNAL TABLE是Greenplum数据库的扩展。SQL标准中没有外部表的条款。

   **参见**

   CREATE TABLE AS, CREATE TABLE, COPY, SELECT INTO, INSERT

2.38 CREATE FUNCTION
-----------------------

   定义函数。

   **语法**

   CREATE [OR REPLACE] FUNCTION name

   ( [ [argmode] [argname] argtype [, ...] ] )

   [ RETURNS { [ SETOF ] rettype

   | TABLE ([{ argname argtype | LIKE other table }

   [, ...]])

   } ]

   { LANGUAGE langname

   | IMMUTABLE | STABLE | VOLATILE

   | CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT | STRICT

   | [EXTERNAL] SECURITY INVOKER | [EXTERNAL] SECURITY DEFINER

   | AS 'definition'

   | AS 'obj_file', 'link_symbol' } ...

   [ WITH ({ DESCRIBE = describe_function

   } [, ...] ) ]

   **描述**

   CREATE FUNCTION定义一个新的函数。CREATE OR REPLACE FUNCTION创建新函数，或者替换已有的函数。

   函数的名字不能和同一模式下的已有的有相同参数类型的函数重名。但是，不同参数类型的函数可以共享同一个名字（重载）。

   想要更新已有的函数的定义，使用CREATE OR REPLACE FUNCTION。用这种方式不能修改一个函数的名字或者参数的类型（这会创建一个新的不同的函数）。CREATE OR REPLACE FUNCTION也不会让你修改已有函数的返回值。要修改函数的返回值，你必须先删除然后重建这个函数。如果你删除然后重建函数，你必须删除引用此函数的已有对象（规则，视图，触发器等）。使用CREATE OR REPLACE FUNCTION会改变函数的定义，而不用破坏引用函数的对象。

   关于创建函数的更多信息，请参看PostgreSQL文档的用户定义函数一节。

   VOLATILE和STABLE函数的有限使用

   想要阻止数据变的在Greenplum数据库的各个segment之间不同步，如果函数包含SQL或者修改数据库，任何VOLATILE或STABLE的函数不能在Segment级别执行。比如，像random()和timeofday()这样的函数是不允许在Greenplum数据库的分布式数据上执行的，因为这些函数有可能会引起各个segment实例之间的数据不一致。

   为了确保数据的一致性，VOLATILE和STABLE函数能够安全地用在在master上解析和执行的语句中。比如，下列语句永远会在master上执行（没有FROM子句的语句）：

   SELECT setval(‘myseq’, 201);

   SELECT foo();

   当语句中有还有分布式表的FROM子句并且FROM子句中用的函数只简单地返回一组行，这样的语句可以在segment上执行：

   SELECT * FROM foo();

   这个规则的一个例外是返回表引用的函数或者使用refCursor数据类型的函数。注意在Greenplum数据库中不能从任何类型的函数中返回refCursor。

   **参数**

   name

   要要创建的函数的名字。

   argmode

   参数的类型：IN、OUT或INOUT。如果省略，默认是IN。

   argname

   参数的名字。一些语言（目前只有PL/pgSQL）允许你在函数体中用这个名字。对其他语言来说，输入参数的名字仅仅只是额外的文档。但是输出参数的名字是很重要的，因为它定义了返回行类型的列名。（如果你省略了输出参数的名字，系统会选择一个默认的列名）。

   argtype

   函数参数的数据类型。参数类型可以是基础、组合或域类型，或者引用表列的类型。

   也许允许指定伪类型比如cstring，这取决于语言的实现。伪类型表示实参类型要不没有指定完整，要么不在普通的SQL数据类型中。

   一个字段的类型是用 tablename.columnname%TYPE 表示的；使用这个东西可以帮助函数独立于表定义的修改。

   rettype

   返回数据类型。输出类型可以声明为一个基本类型，复合类型，域类型， 或者引用一个表的现有字段。根据实现语言的不同，我们还可以在这上面声明 "伪类型"， 比如 cstring。如果一个函数没有返回值，可以指定void作为其返回值。

   如果存在 OUT 或者 INOUT 参数，则可以省略 RETURNS 子句。 如果出现了，那么它必须和输出参数隐含的结果类型兼容：如果有多个输出参数，必须是 RECORD， 如果只有一个输出参数，则与其相同。

   SETOF 修饰词表示该函数将返回一套条目， 而不是一条条目。

   一个字段的类型是通过写 tablename.columnname%TYPE 来引用的。

   langname

   用以实现函数的语言的名字。可以是 SQL，C， internal，或者是用户定义的过程语言名字。Greenplum数据库支持的过程语言，可以参看创建语言。为了保持向后兼容，该名字可以用单引号包围。

   IMMUTABLE

   STABLE

   VOLATILE

   这些属性会把函数的行为告诉查询优化器。最多只能指定一个选项。如果任何一个都没有出现，那么VOLATILE 是缺省假设。因为Greenplum数据库目前只有有限的使用了VOLATILE函数，如果一个函数真的是IMMUTABLE，你必须声明它，以便能够无限制地使用它。

   IMMUTABLE 表示该函数不能修改数据库，并且在给出同样的参数值时总是返回相同的结果。也就是说，它不做数据库查找或者是使用那些并没有直接出现在其参数列表里面的信息。如果给出这个选项，那么任何带着全部是常量参数对该函数的调用都将立即替换为该函数的值。

   STABLE 表示这个函数不能修改数据库，并且在一次表扫描里，对相同参数值，该函数将稳定返回相同的值，但是它的结果可能在不同 SQL 语句之间变化。这个选项对那些结果倚赖数据库查找、参数值（比如当前时区）等的函数是很合适的。还要注意 current_timestamp 族函数是stable (稳定)的，因为它们的值在一次事务中不会变化。

   VOLATILE 表示该函数值甚至可以在一次表扫描内改变， 因此不会做任何优化。很少数据库函数在这个概念上是易变的；一些例子是 random()，currval()， timeofday()。请注意任何有副作用的函数都必需列为易变类，即使其结果相当有规律也应该这样，这样才能避免它被优化；一个例子就是 setval()。

   CALLED ON NULL INPUT

   RETURNS NULL ON NULL INPUT

   STRICT

   CALLED ON NULL INPUT （缺省）表明该函数在自己的某些参数是空值的时候还是可以按照正常的方式调用。剩下的事情是函数的作者必须负责检查空值以及相应地做出反应。

   RETURNS NULL ON NULL INPUT 或 STRICT 表明如果它的任何参数是 NULL，此函数总是返回 NULL。如果声明了这个参数，则如果存在 NULL 参数时不会执行该函数；而只是自动假设一个 NULL 结果。

   [EXTERNAL] SECURITY INVOKER

   [EXTERNAL] SECURITY DEFINER

   SECURITY INVOKER（默认情况） 表明该函数将带着调用它的用户的权限执行。SECURITY DEFINER 声明该函数将以创建它的用户的权限执行。

   关键字 EXTERNAL 的目的是和 SQL 兼容， 但是它是可选的，因为和 SQL 不同，这个特性适用于所有的函数，而不仅仅适用于外部的函数。

   definition

   一个定义函数的字串常量；含义取决于语言。它可以是一个内部函数名字， 一个指向某个目标文件的路径，一个 SQL 命令，或者一个用过程语言写的文本。

   obj_file, link_symbol

   这个形式的 AS 子句用于在函数的 C 源文件里的函数名字和 SQL 函数的名字不同的时候可动态装载 C 语言函数。字串 obj_file 是包含可动态装载的对象的文件名，而 link_symbol 是是该函数在 C 源文件里的名字。如果省略了链接符号，那么就假设它和被定义的 SQL 函数同名。推荐要么通过$libdir（它位于$GPHOME/lib）的相对径路定位动态库，要么通过动态库路径（通过dynamic_library_path服务器配置参数设置的）。如果新的安装在另外的地方，这么做会简化版本升级。

   describe_function

   当一条调用了这个函数的查询被解析时，要执行的回调函数的名字。

   这个回调函数返回一个元组描述符，表示返回类型。

   **注解**

   自定义函数的任何编译代码（动态库文件）必须放在Greenplum数据集群（master和所有的Segment）中每台主机的相同位置。这个位置也必须在LD_LIBRARY_PATH中，以便服务器能定位到文件。推荐在Greenplum集群的所有的master和segment实例中，要么通过$libdir（它位于$GPHOME/lib）的相对径路定位动态库，要么通过动态库路径（通过dynamic_library_path服务器配置参数设置的）。

   完整的 SQL 类型语法允许用于输入参数和返回值。不过，有些类型声明的细节（比如，numeric 类型的精度域）是由下层函数实现负责的，并且不再被CREATE FUNCTION识别或强制。

   Greenplum数据库允许函数重载。同一个函数名可以用于几个不同的函数，只要它们有不同的参数类型。不过，所有函数的 C 名字必须不同，因此你必须给予重载的 C 函数不同的 C 名字（比如，使用参数类型作为 C 名字的一部分）。

   如果两个函数有同样的名字，并且输入参数类型相同，那么就认为这两个函数是一样的，忽略所有 OUT 参数。 因此，下面的声明是冲突的：

   CREATE FUNCTION foo(int) ...

   CREATE FUNCTION foo(int, out text) ...

   如果重复调用 CREATE FUNCTION，并且都指向同一个目标文件， 那么该文件只装载一次。要卸载和装载该文件，你可以使用 LOAD 命令。

   为了能够定义一个函数，用户必须拥有语言上的USAGE权限。

   通常来说，使用美元符包围来书写函数定义字串， 而不是使用普通的单引号包围语法会方便很多。如果不使用美元符包围， 那么函数定义里面的任何单引号或反斜杠都必须用书写双份的方法逃逸。美元符包围的字符串常量包括一个美元符（$），一个可选的零个或多个字符的标签，另一个美元符，组成字符串内容的任意字符序列，一个美元符，开头的标签，和一个美元符。在美元符包围的字符串内，单个引号，反斜线，或任何其他的字符都可以不用转义而使用。字符串的内容总是以字面值来写。比如，下面用两种不同的方式来用美元包围写字符串“Dianne’s horse”：

   $$Dianne's horse$$

   $SomeTag$Dianne's horse$SomeTag$

   在分布数据上查询时使用函数

   在某些情况下，Greenplum数据库不支持在查询中使用函数，这些查询中的FROM子句中指定的表的数据时分布在Greemplum数据库的segment里的。比如，这个SQL查询有函数func()：

   SELECT func(a) FROM table1;

   如果下列所有的情况都遇到的时候，函数是不能在查询中使用的：

   * table1的数据分布在Greenplum数据库的segment中。

   * unc()会读取会修改分布表中的数据。

   * 函数func()返回多于一行，或者它需要一个来自table1的参数（a）。

   如果以上任何一个条件都没有遇到，函数是支持的。特别地，如果下列所有的条件都适用，那么函数是支持的：

   * 函数func()不访问来自分布表的数据，或只访问在Greenplum数据库的master上的数据。

   * 表table1是一个只在master上的表。

   * 函数func()值返回一行，或者只需要常量参数值。如果函数修改之后不需要输入参数，那么这个函数会被支持。

   **示例**

   一个非常简单的加法函数：

   CREATE FUNCTION add(integer, integer) RETURNS integer

   AS ‘select $1 + $2;’

   LANGUAGE SQL

   IMMUTABLE

   RETURN NULL ON NULL INPUT;

   用 PL/pgSQL 自增一个整数，利用参数名：

   CREATE OR REPLACE FUNCTION increment(i integer) RETURNS

   integer AS $$
      
	  BEGIN
      
	  RETURN i + 1;

      END;

   $$ LANGUAGE plpgsql;

   返回一个包含多个输出参数的记录：

   CREATE FUNCTION dup(in int, out f1 int, out f2 text)
    
	AS $$ SELECT $1,  CAST($1 AS text) || ' is text' $$
    
	LANGUAGE SQL;

   SELECT * FROM dup(42);

   你可以通过命名明确的复合类型的方法冗长地干同样的事情：

   CREATE TYPE dup_result AS (f1 int, f2 text);

   CREATE FUNCTION dup(int) RETURNS dup_result
    
	AS $$ SELECT $1, CAST($1 AS text) || ' is text' $$

    LANGUAGE SQL;

   SELECT * FROM dup(42);

   **兼容性**

   CREATE FUNCTION在SQL:1999和其后版本中定义了。Greenplum数据库的和它类似但是不兼容。这个属性是不可移植的，可以使用的不同语言也是如此。

   为了和一些其他的数据库系统兼容，argmode 可以在 argname 之前或者之后写，但是只有第一种方法是标准兼容的。

   **参见**

   ALTER FUNCTION,  DROP FUNCTION,  LOAD 
   
2.39 CREATE GROUP
---------------------

   定义一个新的数据库角色。

   **语法**

   CREATE GROUP name [ [WITH] option [ ... ] ]

   option是：

   SUPERUSER | NOSUPERUSER

   | CREATEDB | NOCREATEDB

   | CREATEROLE | NOCREATEROLE

   | CREATEUSER | NOCREATEUSER

   | INHERIT | NOINHERIT

   | LOGIN | NOLOGIN

   | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'

   | VALID UNTIL 'timestamp'

   | IN ROLE rolename [, ...]

   | IN GROUP rolename [, ...]

   | ROLE rolename [, ...]

   | ADMIN rolename [, ...]

   | USER rolename [, ...]

   | SYSID uid

   **描述**

   从Greenplum数据库2.2开始，CREATE GROUP已经被CREATE ROLE取代，尽管为了向后兼容，依然可以接受。

   **兼容性**

   SQL标准中没有CREATE GROUP语句。

   **参见**

   CREATE ROLE
   
2.40 CREATE INDEX
---------------------

   定义一个新的索引。

   **语法**

   CREATE [UNIQUE] INDEX name ON table

   [USING btree|bitmap|gist]

   ( {column | (expression)} [opclass] [, ...] )

   [ WITH ( FILLFACTOR = value ) ]

   [TABLESPACE tablespace]

   [WHERE predicate]

   **描述**

   CREATE INDEX在指定表上构建一个索引。索引主要用来增强数据库性能（但是使用不当会导致性能变慢）。

   索引的键字段可以通过列名来指定，也可以用卸载括号中的表达式来指定。如果索引方法支持多列索引，就可以指定多个列。

   索引字段可以是从一个或多个表列中计算出来的表达式。这个功能可以用来根据基础数据的一些转换快速访问数据。比如，有一个用upper(col)计算出来的索引，它就可以用在WHERE upper(col) = ‘JIM’子句中。

   Greenplum数据库提供了B-tree索引方法、bitmap索引方法和GiST索引方法。用户也可以自定义索引方法，不过自定义索引方法非常复杂。

   当有WHERE子句出现时，会创建部分索引。部分索引只包含表中的一部分条目，通常是那些对索引更加有用的条目。比如，你有一个表，其中包含已付费订单和未付费订单，未付费订单只占表的一小部分，但是用户会经常查询未付费订单，你就可以只在未付费订单上建立索引来调高性能。

   在 WHERE 子句里用的表达式只能引用下层表的字段，但是它可以使用所有字段，而不仅仅是被索引的字段。目前，子查询和聚集表达式也不能出现在WHERE里。索引字段是表达式时，也不能用在WHERE里。

   索引定义里的所有函数和操作符都必须是immutable。它们的结果必须只能依赖于它们的输入参数，而决不能依赖任何外部的影响（比如另外一个表的内容或者一个参数值）。 这个约束确保该索引的行为是定义完整的。要在一个索引表达式上或在WHERE子句里使用用户定义函数，请记住在你创建的时候把它标记为immutable的函数。

   **参数**

   UNIQUE

   令系统在索引创建时和每次添加数据时检测表中是否有重复值。重复值会导致系统错误。唯一索引仅适用于B-tree索引。在Greenplum数据库中，只有在索引列和Greenplum分布式键相同，或者索引列是Greenplum分布式键的子集时，才允许创建唯一索引。在分区表上，唯一索引只在单个分区内支持，不能跨分区创建唯一所以。

   name

   索引的名字。索引只能创建在父表的模式下。

   table 

   要创建索引的表名。

   btree | bitmap | gist

   索引方法的名字。选项使btree, bitmap和gist。默认方法是btree。

   column

   要创建索引的列名。只有B-tree、bitmap和GiST索引方法支持多列索引。

   expression

   包含一列或多列的表达式。这个表达式必须用括号括住。但是，如果表达式中有函数调用，括号可以省略。

   opclass

   操作符类的名字。运算符类指定了用在索引中的运算符。比如，四字节整数上的B-tree索引会使用int4_ops类（这个运算符类包含四字节整数的比较函数）。实际上，列的数据类型的默认运算符类通常是足够的。运算符类的关键点是某些数据类型可能有多个有意义的排序。例如，我们对复数类型排序时有可能以绝对值或者以实部。 我们可以通过为该数据类型定义两个操作符类，然后在建立索引的时候选择合适的类来实现。

   FILLFACTOR

   索引的装填因子是一个百分比，它决定了索引方法压缩索引页的程度。对B数索引来说，叶子页在初始创建索引的时候就用这个百分比填好了，当从右边扩展索引的时候也会用这个百分比填好的。如果叶子页接下来完全满了，这会导致索引的效率下降，所以它们就会被分拆。

   B数据索引的默认装填因子是90，但是它值可以是10到100的任意值。如果表是静态的，那么装填因子最好设为100，以便让索引的物理大小达到最小。但是对于一个经常更新的表，更小的装填因子更好，因为这样可以减少索引的拆分。另外一个索引方法用另一种不同但大体类似的方法使用装填因子。不同方法的默认装填因子不同。

   tablespace

   创建的索引所在的表空间。如果没有声明，则使用默认的表空间， 

   predicate

   为一个部分索引定义约束表达式。

   **注解**

   当在一个分区表上创建索引时，索引会传播到所有Greenplum数据库创建的子表中去。在一个分区表使用的表上创建索引是不支持的。

   只有在索引列和Greenplum分布式键相同，或者索引列是Greenplum分布式键的子集时，才允许创建唯一索引。

   UNIQUE索引在只追加表上也是不允许的。

   UNIQUE索引可以创建在分区表上。但是，只能保证在一个分区上的唯一性，在分区之间不保证唯一性。比如，一个分区表有基于年的分区和基于季度的子分区，只能保证每个季度分区的唯一性。在季度分区之间是不能保证唯一性。

   默认情况下，IS NULL子句不适用索引。在这种情况下，使用索引的最好办法是创建一个使用IS NULL约束的部分索引。

   bitmap索引在100到100，000无重复值的列上性能最好。如果一个列有超过100，000个无重复值，bitmap索引的性能和空间效率都会下降。bitmap索引的大小是和表中总行数成比例的，是列中无重复值的倍数。

   少于100个无重复值的列通过不能从任何索引中受益。比如，一个性别列，只有两个无重复值，男和女，是不适合建索引的。

   以前的Greenplum数据库的发布版中还有一种R-tree索引方法。这种方法已经移除了，因为它和GiST方法相比没有显著的提升。如果指定USING rtree，CREATE INDEX会把它解析为USING gist。

   关于GiST索引类型的更多信息，请参考PostgreSQL文档。

   Greenplum数据库已经不支持hash和GIN索引了。

   **示例** 

   为films表的title列创建B-tree索引：

   CREATE UNIQUE INDEX title_idx ON films (title);

   为employee表的gender列创建bitmap索引：

   CREATE INDEX gender_bmp_idx ON employee USING bitmap (gender);

   在表达式lower(title)上创建索引，允许有效的忽略大小写的查找：

   CREATE INDEX lower_title_idx ON films ((lower(title)));

   创建索引，指定装填因子：

   CREATE UNIQUE INDEX title_idx ON films (title) WITH (fillfactor = 70);

   为表films的code列创建索引，并指定表空间indexspace：

   CREATE INDEX code_idx ON films(code) TABLESPACE indexspace;

   **兼容性**

   CREATE INDEX是Greenplum数据库的语言扩展。SQL标准中没有索引的相关规定。

   Greenplum数据库不支持并发创建索引（CONCURRENTLY关键字是不支持的）。

   **参见**

   ALTER INDEX, DROP INDEX, CREATE TABLE, CREATE OPERATOR CLASS


2.41 CREATE LANGUAGE
-------------------------

   定义一个新的过程语言。

   **语法**

   CREATE [PROCEDURAL] LANGUAGE name

   CREATE [TRUSTED] [PROCEDURAL] LANGUAGE name

   HANDLER call_handler [VALIDATOR valfunction]

   **描述**

   CREATE LANGUAGE在Greenplum数据库中注册一个新的过程语言。之后，就可以用这种新的语言来定函数和触发过程了。注册新的语言必须是超级用户。默认情况下，PL/pgSQL语言已经注册在所有的数据库中了。

   CREATE LANGUAGE实际上会把一个语言名字关联到一个调用句柄上，这个调用句柄负责执行用这种语言写的函数。对于用过程语言（非C和SQL的语言）写的函数，数据库服务器内部是不知道如何解释函数代码的。这个任务会转给一个知道语言细节的专门的句柄。这个句柄要么自己所所有解析工作，语法分析，执行等，要么会把Greenplum数据库和已有程序设计语言实现联系起来。这个句柄本身是一个C语言函数，编译到一个共享对象，像其他C函数一样按需加载。目前Greenplum数据库发布版中包含四个过程语言包：PL/pgSQL, PL/Perl, PL/Python和PL/Java。PL/R语言的句柄也可以加入进来，但是PL/R语言包没有预装在Greenplum数据库中。关于使用这些过程语言开发函数的更多信息，请参见PostgreSQL文档中的过程式语言相关的主题。

   PL/Perl, PL/Java和PL/R库需要正确版本的Perl, Java,和R。

   在RHEL和SUSE平台上，从Pivotal Network上下载合适的扩展，然后用Greenplum Package Manager(gppkg)工具安装扩展，以确保扩展的所有依赖都正确安装。关于gppkg的详细信息请参见《Greenplum数据库工具指南》。

   CREATE LANGUAGE命令有两种格式。第一种格式，用户指定想要的语言的名字，Greenplum数据库服务器会用pg_pltemplate系统分类决定正确的参数。第二种格式，用户指定语言参数和名字。你可以用第二种格式来创建一个没有在pg_pltemplate中定义的语言。

   如果服务器在pg_pltemplate分类中发现了给定的语言名，即使这个命令包含了语言参数，它依然会使用分类数据。这种行为简化了旧的dmp文件的加载，这些旧的dmp文件很有可能包含了过时的语言支持函数。

   **参数**

   TRUSTED

   如果服务器在pg_pltemplate中有给定的语言名字，这个参数会被忽略。

   表示语言的调用句柄是安全的，不会给未授权用户任何通过访问限制的功能。如果在注册语言的时候忽略这个关键字，只有有超级用户权限的用户才可以使用这种语言来创建新的函数。

   PROCEDUAL

   这是一个噪音词。

   name

   新的过程语言的名字。这个语言名字是大小写敏感的。数据库中的语言名必须唯一。plpgsql, plperl, plpython, plpythonu是内建支持的，Greenplum数据库中默认已经安装了plr.plpgsql。

   HANDLER call_handler

   如果服务器在pg_pltemplate中有给定的语言名字，这个参数会被忽略。

   提前注册好的一个函数，会调用它来执行过程语言函数。过程语言的调用句柄必须是用编译型语言比如C写的，有版本1的调用约定。过程调用句柄是在Greenplum数据库中注册为一个没有参数且返回language_handler类型的函数，language_handler是一个占位符类型，仅用来表示这个函数是一个调用句柄。

   VALIDATOR valfunction

   如果服务器在pg_pltemplate中有给定的语言名字，这个参数会被忽略。

   valfunction是一个预先注册的函数名字，当用这种语言创建一个函数时会调用valfunction，去校核新的函数。如果没有指定校核函数，那么新函数创建时不会检查。校核函数必须有一个类型为oid的参数，它是将要创建的函数的OID，并且会返回类型void。

   校核函数通常检查函数体的语法正确性，而且还会检查函数的其他属性，比如，如果语言不能处理某些参数类型。校核函数应该使用ereport()去报告错误。这个函数的返回值会被忽略。

   **注解**

   Greenplum数据库默认安装PL/pgSQL语言。

   系统分类pg_language记录目前已安装语言的信息。

   想用过程语言创建函数，用户必须有这种语言的USAGE权限。默认情况下，对于信任语言，USAGE赋给PUBLIC（每一个人）。如果需要这会被调用。

   对单个数据库来说，过程语言是局部的。然而，一种语言可以被安装到template1数据库中，这样后续创建的数据库会自动拥有这种语言。

   如果一种语言没有在服务器的pg_pltemplate中，那么必须提前定义调用句柄函数和校核函数。但是如果这种语言在pg_pltemplate中，那么调用句柄函数和校核函数不必提前存在。如果数据库中没有这两个函数，则会自动定义它们。

   任何实现了一种语言的共享库必须在Greenplum数据库集群中所有segment同一个位置，而且这个位置必须在LD_LIBRARY_PATH中。

   **示例**

   创建标准过程语言的方法：

   CREATE LANGUAGE plpgsql;

   CREATE LANGUAGE plr;

   不在pg_pltemplate分类中的语言：

   CREATE FUNCTION plsample_call_handler() RETURNS

   language_handler

   AS ‘$libdir/plsample’

   LANGUAGE C;

   CREATE LANGUAGE plsample HANDLER plsamle_call_handler;

   **兼容性**

   CREATE LANGUAGE 是Greenplum数据库的扩展。

   **参见**

   ALTER LANGUAGE, CREATE FUNCTION, DROP LANGUAGE
   
2.42 CREATE OPERATOR
-------------------------

   定义一个新的运算符。

   **语法**

   CREATE OPERATOR name (

   PROCEDURE = funcname

   [, LEFTARG = lefttype] [, RIGHTARG = righttype]

   [, COMMUTATOR = com_op] [, NEGATOR = neg_op]

   [, RESTRICT = res_proc] [, JOIN = join_proc]

   [, HASHES] [, MERGES]

   [, SORT1 = left_sort_op] [, SORT2 = right_sort_op]

   [, LTCMP = less_than_op] [, GTCMP = greater_than_op] )

   **描述**

   CREATE OPERATOR定义一个新的运算符。定义运算符的用户是运算符的所有者。

   运算符的名字是一个最长NAMEDATALEN-1（默认63）个字符的序列，运算符的名字可以使用的字符包括：+ - * / < > = ~ ! @ # % ^ & | ` ?。

   对运算符的名字有如下限制：

   * --和/*不能出现在运算符名中，因为它们是注释的开头。

   * 多字符的运算符名不能以+或-结尾，除非运算符名中有一个如下字符：~ ! @ # % ^ & | ` ?。

   例如，@-是合法的运算符名，但是*-不是合法运算符名。有了这些限制，Greenplum数据库可以在记号之间没有空白的情况下也能解析兼容SQL的命令。

   输入时!=会被映射为<>，因此这两个名字拥有时等价的。

   必须有LEFTARG和RIGHTARG之一。二元运算符必须有两者都有。对右一元运算符，只需要LEFTARG，而左一元运算符，只需要RIGHTARG。

   funcname过程必须提前定义，必须是IMMUTABLE，必须接受指定类型的正确数量的参数（一个或两个）。

   其他的子句都是可选的运算符优化子句。适合用这个运算符来加速查询的时候，应该提供这些子句。如果提供它们，则必须保证它们是正确的。优化子句使用不当会导致服务器进程崩溃，输出错误，或者其他难以预料的结果。如果你不确定，你可以在任何时候省去优化子句。

   **参数**

   name

   运算符的名字。如果运算符定义在不同的类型上，那么同一个模式中可以有两个同名的运算符。

   funcname

   用来实现运算符的函数名。

   lefttype

   运算符左运算数的类型。如果是左一元运算符，则可省略。

   righttype

   运算符右运算数的类型。如果是右一元运算符，则可省略。

   com_op

   可选的COMMUTATOR子句命名一个运算符为将要定义的运算符的交换运算符。如果对所有可能得输入x, y，（x A y）等于（x B y），那么我们说运算符A是运算符B的交换运算符。注意B也是A的交换运算符。例如，运算符<和>对某些数据类型来说它们是彼此的交换运算符，运算符+经常与自身交换。但是运算符-通常不和任何运算符交换。一个交换运算符的左操作数类型和这个交换运算符的右操作数是相同的，反之亦然。因此交换运算符的名字是需要在COMMUTATOR子句中提供的。

   neg_op

   可选的NEGATOR子句为将要定义的运算符指定负操作符。如果对任何可能得输入x,y，(x A y)等于(x B y)，返回布尔值，那么我们说A是B的负操作符。注意B也是A的负操作符。例如，<和>=对大多数的类型来说是一对负操作符。一个运算符的负操作符的左右运算数必须和该运算符的左右操作数类型一致，因此只需要在NEGATOR子句中指定运算符名字。

   res_proc

   此操作符的约束选择性计算函数。注意这是一个函数名字，而不是一个运算符名字。RESTRICT子句只对返回boolean值的二元操作符有效。

   限制选择性计算函数背后的思想是：猜测表中哪一部分行将会满足WHERE子句中如下形式的条件：

   column OP constant

   对当前的运算符和一个特定的常量值。它会告诉优化器where子句会排出多少行，因此会帮助优化器。

   对你自己的运算符来说，你通常只能使用下来系统标准计算函数之一：

   对=， eqsel

   对<>，neqsel

   对< or <=，scalarltsel

   对> or >=，scalargtsel

   join_proc

   此操作符连接选择性计算函数。注意这是一个函数名字，而不是一个运算符名字。JOIN子句只对返回boolean值的二元操作符有效。

   连接选择性计算函数背后的思想是：猜测表中哪一部分行将会满足WHERE子句中如下形式的条件：

   table1.column1 OP table2.column2

   对当前的运算符。它会告诉优化器多个可能得连接序列中的哪一个会占用最少的工作，因此会帮助优化器。

   对你自己的运算符来说，你通常只能使用下来系统标准连接选择计算函数之一：

   eqjoinsel for =

   neqjoinsel for <>

   scalarltjoinsel for < or <=

   scalargtjoinsel for > or >=

   areajoinsel for 2D area-based comparisons

   positionjoinsel for 2D position-based comparisons

   contjoinsel for 2D containment-based comparisons

   HASHES

   表明此操作符支持哈希（散列）连接。HASHES只对返回boolean值的二元操作符有效。哈希连接运算符只对有相同哈希码的左右值返回true。如果左值和右值的哈希码不同，连接运算符不会比较它们，也就说连接运算符的结果必须是false。因此对不代表相等的运算符指定HASHES是没有意义的。

   要指定HASHES，连接运算符必须出现在哈希索引运算符类中。如果没有这种运算符类，而去试图在哈希连接中使用这个运算符会在运行的时候失败。系统需要运算符类去为运算符的输入数据类型找数据类型相关的哈希函数。在创建运算符类之前必须提供合适的哈希函数。在准备哈希函数的时候必须注意，因为有依赖于机器的方式，这种方式在做正确事情的时候会失败。

   MERGES

   表明此操作符可以支持一个融合连接。MERGES只对返回boolean值的二元操作符有效。实际上，这个运算符对于一些数据类型或者数据类型对必须表示相等。

   融合连接是以这样的概念为基础的：对左边和右边的表进行排序，然后并发地扫描它们。所以，两种数据类型都必须是能够完全排序的，并且连接操作符必须只对那些落在排序顺序中的"某个位置"的数值对成功。实际上这意味着连接操作符必须表现得像等于。可以对两种不同数据类型进行融合连接，只要他们逻辑相等即可。例如，smallint 对 integer 的相等操作符是可以用融合连接的。只需要可以把两种数据类型排列成逻辑可比序列的排序操作符即可。

   融合连接的执行要求系统可以标识四种与融合连接相等性操作符相关的操作符：用于左操作数数据类型的小于比较，用于右操作数数据类型的小于比较，在两种数据类型之间的小于比较，以及在两种数据类型之间的大于比较。可以通过名字逐个声明这些操作符，分别是 SORT1, SORT2, LTCMP, GTCMP 选项。如果在声明了 MERGES 的同时却省略了其中的任何一个，那么系统将填充缺省的名字

   left_sort_op

   如果此操作符支持融合连接（join），排序此操作符左手边数据的小于操作符。如果不指定默认是<。

   right_sort_op

   如果此操作符支持融合连接（join），排序此操作符右手边数据的小于操作符。如果不指定默认是<。

   less_than_op

   如果这个操作符可以支持融合连接，那么这就是比较这个操作符的输入数据类型的小于操作符。如果不指定默认是<。

   greater_than_op

   如果这个操作符支持融合连接，那么这就是比较输入这个操作符的输入数据类型的大于操作符。如果不指定默认是>。

   要在可选参数里给出一个模式修饰的操作符名，使用 OPERATOR() 语法，比如

   COMMUTATOR = OPERATOR(myschema.===) ,

   **注解**

   任何用来实现操作符的函数必须定义为IMMUTABLE。

   **示例**

   这里是一个创建运算符的例子，这个运算符会求两个复数的和，假设我们预先定义了complex类型。首先定义求和的函数，然后定义运算符：

   CREATE FUNCTION complex_add(complex, complex)

   RETURNS complex

   AS 'filename', 'complex_add'

   LANGUAGE C IMMUTABLE STRICT;

   CREATE OPERATOR + (

   leftarg = complex,

   rightarg = complex,

   procedure = complex_add,

   commutator = +

   );

   在查询中使用这个操作符：

   SELECT (a + b) AS c FROM test_complex;

   **兼容性**

   CREATE OPERATOR是Greenplum数据库的语言扩展。SQL标准不提供用户自定义运算符。

   **参见**

   CREATE FUNCTION, CREATE TYPE, ALTER OPERATOR, DROP OPERATOR

2.43 CREATE OPERATOR CLASS
-----------------------------

   定义一个新的运算符类。

   **语法**

   CREATE OPERATOR CLASS name [DEFAULT] FOR TYPE data_type

   USING index_method AS

   {

   OPERATOR strategy_number op_name [(op_type, op_type)] [RECHECK]

   | FUNCTION support_number funcname (argument_type [, ...] )

   | STORAGE storage_type

   } [, ... ]

   **描述**

   CREATE OPERATOR CLASS创建一个新的运算符类。一个操作符类定义一种特定的数据类型如何与一种索引一起使用。操作符类声明特定的操作符可以为这种数据类型以及这种索引方法填充特定的角色或者"策略"。操作符类还声明索引方法在为一个索引字段选定该操作符类的时候要使用的支持过程。所有操作符类使用的函数和操作符都必须在创建操作符类之前定义。任何用来实现运算符类的函数必须定义为IMMUTABLE。

   CREATE OPERATOR CLASS 既不检查这个类定义是否包含所有索引方法需要的操作符以及函数，也不检查这些操作符和函数是否形成一个自包含的集合。定义一个合法的操作符类是用户的责任。

   要创建操作符类，必须是超级用户。

   **参数**

   name

   要创建的操作符类的名字，可以用模式名修饰。如果同一个模式中的两个操作符类有不同的索引方法，那么它们可以同名。

   DEFAULT

   表示该操作符类将成为它的数据类型的缺省操作符类。对于某个数据类型和访问方式而言，最多有一个操作符类是缺省的。

   data_type

   这个操作符类处理的字段的数据类型。

   index_method

   这个操作符类处理的索引方法的名字。可以是btree，bitmap和gist。

   strategy_number

   和操作符类关联的操作符用策略数来识别，策略数用来识别操作符类上下文中的每个运算符的语法。比如，B-tree在键上强加了一个严格排序，从小到大，因此像小于和大于或等于运算符对btree来说是感兴趣的。这些策略可以认为是一般的操作符。每个操作符类指定某种数据类型和索引语法解释的策略和实际的操作符的对应关系。每种索引方法的策略数如下：

   表5：B-tree和Bitmap策略
   =========== ================
   运算符      策略数
   =========== ================
   小于        1
   小于等于    2
   等于        3
   大于等于    4
   大于        5
   =========== ================

   表6：GiST二维策略（R-Tree）
   =============== =====================
   操作符          策略数
   =============== =====================
   严格地在左边    1
   不扩展到右边    2
   重叠            3
   不延伸到左边    4
   严格地在右边    5
   相同            6
   包含            7
   包含于          8
   不扩展到上面    9
   严格地在下面    10
   不扩展到下面	   12
   =============== =====================

   operator_name

   和操作符类关联的操作符名，可用模式名修饰。

   op_type

   操作符的操作数类型，NONE表示左一元操作符或右一元操作符。操作数的数据类型和操作符类的数据类型相同的情况下，操作数数据类型可以省略。

   RECHECK

   如果出现，那么索引对这个操作符是"有损耗的"，因此，使用这个索引检索的行必须重新检查，以保证它们真正满足和此操作符相关的条件子句。

   support_number

   索引方法为了工作需要额外的支持过程。这些过程是所有方法在内部使用的管理过程。就像策略一样，操作符类声明在一定的数据类型和语义解释的条件下，哪个特定函数对应这些角色中的哪一个。索引方法声明它需要的函数集，而操作符类通过给它们赋予如下的"支持函数编号"来标识要正确使用的函数

   表7：B-tree和Bitmap支持函数
   =============================================================================== =========
   函数                                                                            支持号
   =============================================================================== =========
   比较两个键，返回一个小于0,0或大于0的整数，表示第一个键小于，等于或大于第二个键  1
   =============================================================================== =========
   
   表8：GiST支持函数
   =============================================================================== =========
   函数                                                                            支持号
   =============================================================================== =========
   一致性——决定键是否满足查询限定符                                                1
   联合——计算一组键的联合                                                          2
   压缩——计算将要索引的键或值的压缩表示	                                           3
   解压缩——计算压缩建的解压缩表示                                                  4
   性能恶化——计算使用给定的子树的键向子树中插入新键的性能恶化(penalty)             5
   拆分——检测页面中的那个项将被移动到新页面并为结果页计算联合键                    6
   等于——比较两个键并在相等时返回真                                                7
   =============================================================================== =========
   
   funcname

   一个函数的名字(可以有模式修饰)，这个函数是索引方法对此操作符类的支持过程。

   argument_types

   函数参数的数据类型。

   storage_type

   实际存储在索引里的数据类型。通常它和字段数据类型相同，但是GiST索引方法允许它是不同的。除非索引方法允许使用一种不同的类型，否则必须省略 STORAGE 子句。

   **注解**

   因为索引机制不在使用函数前检查其访问机制，在操作符类中包含操作符或者函数等价于授权给所有人执行权限。这对于那些用于操作符类的函数通常不会导致什么问题。

   操作符不应该用 SQL 函数定义。一个 SQL 函数很可能是内联到调用它的查询里面，这样将阻止优化器识别这个查询是否可以使用索引。

   任何用来实现操作符类的函数必须定义为IMMUTABLE。

   **示例**

   下面的例子命令为数据类型 _int4(int4 数组)定义了一个 GiST 索引操作符类。

   CREATE OPERATOR CLASS gist__int_ops

   DEFAULT FOR TYPE _int4 USING gist AS

   OPERATOR 3 &&,

   OPERATOR 6 = RECHECK,

   OPERATOR 7 @>,

   OPERATOR 8 <@,

   OPERATOR 20 @@ (_int4, query_int),

   FUNCTION 1 g_int_consistent (internal, _int4, int4),

   FUNCTION 2 g_int_union (bytea, internal),

   FUNCTION 3 g_int_compress (internal),

   FUNCTION 4 g_int_decompress (internal),

   FUNCTION 5 g_int_penalty (internal, internal, internal),

   FUNCTION 6 g_int_picksplit (internal, internal),

   FUNCTION 7 g_int_same (_int4, _int4, internal);

   **兼容性**

   CREATE OPERATOR CLASS是一个Greenplum数据库的扩展。SQL标准中没有CREATE OPERATOR CLASS语句。

   **参见**

   ALTER OERATOR CLASS, DROP OPERATOR CLASS, CREATE FUNCTION

2.44 CREATE RESOURCE QUEUE
---------------------------------

   定义一个新的资源队列。

   **语法**

   CREATE RESOURCE QUEUE name WITH (queue_attribute=value [, ... ])

   这里的queue_attribute是：

   ACTIVE_STATEMENTS=integer

   [ MAX_COST=float [COST_OVERCOMMIT={TRUE|FALSE}] ]

   [ MIN_COST=float ]

   [ PRIORITY={MIN|LOW|MEDIUM|HIGH|MAX} ]

   [ MEMORY_LIMIT='memory_units' ]

   | MAX_COST=float [ COST_OVERCOMMIT={TRUE|FALSE} ]

   [ ACTIVE_STATEMENTS=integer ]

   [ MIN_COST=float ]

   [ PRIORITY={MIN|LOW|MEDIUM|HIGH|MAX} ]

   [ MEMORY_LIMIT='memory_units' ]

   **描述**

   为Greenplum数据库工作负载管理创建一个新的资源队列。资源队列必须或者有ACTIVE_STATEMENTS值或者有MAX_COST值（或者两个都有）。只有超级用户才能创建资源队列。

   ACTIVE_STATEMENTS资源队列设置了赋给队列的用户能够执行的最大查询数量。它限制了同时允许运行的活动查询数。ACTIVE_STATEMENTS的值必须是一个大于0的整数。

   MAX_COST资源队列限制了赋给队列的用户能够执行的查询的最大总代价。代价用Greenplum数据库的查询计划器的查询估计总代价来衡量（查询的EXPLAIN结果）。因此，为了给资源队列设置合适的代价阈值，管理员必须熟悉系统上经常执行的查询。代价用取磁盘页的单位来衡量；1.0等于一个顺序磁盘页读。MAX_COST的值可以设置为一个浮点数（比如100.0）或者设置为一个指数（比如1e+2）。如果资源队列是基于代价阈值而限制的，那么管理员可以允许COST_OVERCOMMIT=TRUE（默认值）。这意味着当一个查询超过了允许的代价阈值时，只有在系统闲的时候才会允许执行。如果指定COST_OVERCOMMIT=FALSE，超过代价阈值的查询总是被拒绝，绝不允许执行。通过指定MIN_COST的值，管理员可以为小的不用排队的查询定义一个代价。

   如果不指定ACTIVE_STATEMENTS或MAX_COST的值，默认它们会设置为-1（表示没有限制）。在定义个资源队列之后，你必须用ALTER ROLE或CREATE ROLE命令为队列指定用户。你也可以给资源队列指定可选的PRIORITY，以便控制与资源队列相关的查询使用的CPU份额。如果不指定PRIORITY值，与资源队列相关的查询会有一个默认优先级MEDIUM。

   有可选MEMORY_LIMIT阈值的资源队列会设置通过此资源队列提交的所有的查询在一个segment主机上能够使用的最大内存。这会决定单个segment主机上，一个查询执行时，所有工作进程能使用的最大内存量。Greenplum推荐MEMORY_LIMIT和ACTIVE_STATEMENTS连用，不和MAX_COST连用。在基于语句的队列中每个查询分配的默认内存量是：MEMORY_LIMIT/ACTIVE_STATEMENTS。基于代价的队列中每个查询分配的默认内存量是：MEMORY_LIMIT * (query_cost / MAX_COST)。

   如果MEMORY_LIMIT或max_statement_mem没有超过，默认的内存量可以在每一个查询中用statement_men服务器配置参数来重写。比如，为了给一个特定的查询分配更多的内存：

   => SET statement_mem='2GB';

   => SELECT * FROM my_big_table WHERE column='value' ORDER BY id;

   => RESET statement_mem;

   所有资源队列的MEMORY_LIMIT不应该超过segment主机的物理内存量。如果工作负载跨多个资源队列，内存可能会分配过多。然而，如果在gp_vmem_protect_limit中指定的segment主机的内存限制超过了，查询可以在执行中途取消。

   关于statement_men, max_statement和gp_vmem_protect_limit，请看服务器配置参数。

   **参数**

   name

   资源队列的名字。

   ACTIVE_STATEMENT integer

   ACTIVE_STATEMENTS资源队列设置了赋给队列的用户能够执行的最大查询数量。它限制了同时允许运行的活动查询数。ACTIVE_STATEMENTS的值必须是一个大于0的整数。

   MEMORY_LIMIT ‘memory_units’

   设置在这个资源队列中的所有用户发出的所有语句的总内存配额。内存单位可以是Kb,MB或GB。一个资源队列的最小内存配额是10MB。没有最大值，但是查询执行期间的上线是segment主机的物理内存。默认值是没有限制（-1）。

   MAX_COST float

   MAX_COST资源队列限制了赋给队列的用户能够执行的查询的最大总代价。代价用Greenplum数据库的查询计划器的查询估计总代价来衡量（查询的EXPLAIN结果）。因此，为了给资源队列设置合适的代价阈值，管理员必须熟悉系统上经常执行的查询。代价用取磁盘页的单位来衡量；1.0等于一个顺序磁盘页读。MAX_COST的值可以设置为一个浮点数（比如100.0）或者设置为一个指数（比如1e+2）。

   COST_OVERCOMMIT boolean

   如果资源队列是基于代价阈值而限制的，那么管理员可以允许COST_OVERCOMMIT=TRUE（默认值）。这意味着当一个查询超过了允许的代价阈值时，只有在系统闲的时候才会允许执行。如果指定COST_OVERCOMMIT=FALSE，超过代价阈值的查询总是被拒绝，绝不允许执行。

   MIN_COST float

   被认为是小查询的最小查询代价限制。代价低于这个限制的查询不会排队，而会立即执行。代价用Greenplum数据库的查询计划器的查询估计总代价来衡量（查询的EXPLAIN结果）。因此，为了给资源队列设置合适的代价阈值，管理员必须熟悉系统上经常执行的查询。代价用取磁盘页的单位来衡量；1.0等于一个顺序磁盘页读。MAX_COST的值可以设置为一个浮点数（比如100.0）或者设置为一个指数（比如1e+2）。

   PRIORITY={MIN|LOW|MEDIUM|HIGH|MAX}

   设置资源队列里的查询的优先级。资源队列里优先级高的查询在争用资源的时候会获得更大的可用CPU资源。当优先级高的查询在执行的时候优先级低查询会延迟。如果没有指定优先级，资源队列中的查询的默认优先级是MEDIUM。

   **注解**

   用gp_toolkit.gp_resqueue_status系统视图可以查看限制设置和资源队列的当前状态：

   SELECT * from gp_toolkit.gp_resqueue_status WHERE rsqname=’queue_name’;

   有另外一个系统视图pg_stat_resqueues，它显示了随着时间变化的资源队列的统计量。为了使用这个视图，你必须打开stats_queue_level服务器配置参数。关于资源队列的更多信息，参考《Greenplum数据库管理员指南》中的“管理工作负载荷资源”。

   CREATE RESOURCE QUEUE不能在一个事务中执行。

   **示例**

   创建一个活动查询限制为20的资源队列：

   CREATE RESOURCE QUEUE myqueue WITH (ACTIVE_STATEMENTS=20);

   创建一个资源队列，活动查询限制20，总内存限制2000MB（在执行期间，在segment主机上，每个查询会分到100MB的内存）

   CREATE RESOURCE QUEUE myqueue WITH (ACTIVE_STATEMENTS=20,

   MEMORY_LIMIT='2000MB');

   创建一个资源队列，查询代价限制3000.0：

   CREATE RESOURCE QUEUE myqueue WITH (MAX_COST=3000.0);

   创建一个资源队列，查询代价限制310（或30000000000.0），不允许超过，允许代价低于500的查询立即执行：

   CREATE RESOURCE QUEUE myqueue WITH (MAX_COST=3e+10,

   COST_OVERCOMMIT=FALSE, MIN_COST=500.0);

   创建一个资源队列，同时设置活动查询限制和查询代价限制：

   CREATE RESOURCE QUEUE myqueue WITH (ACTIVE_STATEMENTS=30,

   MAX_COST=5000.00);

   创建资源队列，活动查询限制5，最大优先级：

   CREATE RESOURCE QUEUE myqueue WITH (ACTIVE_STATEMENTS=5,

   PRIORITY=MAX);

   **兼容性**

   CREATE RESOURCE QUEUE是Greenplum数据库的扩展。SQL表中没有资源队列或工作负载管理。

   **参见**

   ALTER ROLE, CREATE ROLE, ALTER RESOURCE QUEUE, DROP RESOURCE QUEUE

2.45 CREATE ROLE
-----------------------

   定义一个新的数据库角色（用户或组）。

   **语法**

   CREATE ROLE name [[WITH] option [ ... ]]

   option是：

   SUPERUSER | NOSUPERUSER

   | CREATEDB | NOCREATEDB

   | CREATEROLE | NOCREATEROLE

   | CREATEEXTTABLE | NOCREATEEXTTABLE

   [ ( attribute='value'[, ...] ) ]

   where attributes and value are:

   type='readable'|'writable'

   protocol='gpfdist'|'http'

   | INHERIT | NOINHERIT

   | LOGIN | NOLOGIN

   | CONNECTION LIMIT connlimit

   | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'

   | VALID UNTIL 'timestamp'

   | IN ROLE rolename [, ...]

   | ROLE rolename [, ...]

   | ADMIN rolename [, ...]

   | RESOURCE QUEUE queue_name

   | [ DENY deny_point ]

   | [ DENY BETWEEN deny_point AND deny_point]

   **描述**

   CREATE ROLE增加一个新的角色到Greenplum数据库系统。角色是一个实体，它可以拥有数据库对象和数据库权限。一个角色可以是一个用户，一个组，或者两者都是，这取决于如何使用。使用这个命令，必须有CREATEROLE权限或是数据库超级用户。

   注意定义为系统级别的角色，对Greenplum数据库系统中的所有数据库都是有效的。

   **参数**

   name

   新角色的名字。

   SUPERUSER

   NOSUPERUSER

   如果指定SUPERUSER，角色将会是超级用户，它会重写数据库中的所有的访问限制。超级用户状态是危险的，只应该在真实需要的时候才使用。只有超级用户才能创建超级用户，NOSUPERUSER是默认值。

   CREATEDB

   NOCREATEDB

   如果指定CREATEDB，创建的角色运行创建新数据库。NOCREATEDB（默认）会否决角色创建数据库的能力。

   CREATEROLE

   NO CREATEROLE

   如果指定CREATEROLE，允许这个角色创建新的角色，修改其他的角色，删除其他的角色。NOCREATEROLE（默认值）不允许创建新角色，修改自己之外的角色。

   CREATEEXTTABLE

   NOCREATEEXTTABLE

   如果指定CREATEEXTTABLE，这个角色就可以创建外部表。默认的type是readable，默认protocol是gpfdist。NOCREATEEXTTABLE表示角色不能创建外部表。注意使用file和execute协议的外部表只能由超级用户创建。

   INHERIT

   NOINHERIT

   如果指定，INHERIT（默认情况）属性的角色可以自动使用已经赋与它直接或间接所在组的任何权限。没有INHERIT，其它角色的成员关系只赋与该角色 SET ROLE 成其它角色的能力。

   LOGIN

   NOLOGIN

   如果指定，LOGIN允许一个角色登陆到一个数据库中。有LOGIN属性的角色可以认为是一个用户。用NOLOGIN的角色（默认情况）用来管理数据库权限是很有用的，可以看做组。

   CONNECTION LIMIT connlimit

   这个角色可以并行发起的最大连接数。默认值是-1，表示没有限制。

   PASSWORD password

   设置有LOGIN属性的角色的用户密码。如果你不打算使用密码验证，你可以忽略这个选项。如果不指定密码，密码会设置为null，密码验证会一直失败。null密码可以显式写成PASSWORD NULL。

   ENCRYPTED

   UNENCRYPTED

   这些关键字控制存储在系统表里面的口令是否加密。如果没有指定，那么缺省的行为由配置参数 password_encryption 控制。因为系统无法对指定的口令字符串进行解密，所以如果目前的口令字符串已经是用 MD5 加密的格式，那么那就会继续照此存放，而不管是否声明了 ENCRYPTED 或 UNENCRYPTED 。这样就允许在转储/恢复的时候重新加载加密的口令。

   请注意老的客户端可能缺乏对 MD5 认证机制的支持，以密文形式存储的口令需要这个机制来运作。

   VALID NUTIL ‘timestamp’

   VALID UNTIL 子句设置角色的口令失效的时间戳。如果忽略了这个子句，那么口令将永远有效。

   IN ROLE rolename

   把新建的角色添加为命名角色的成员。请注意没有任何选项可以把新角色添加为管理员；必须使用独立的 GRANT 命令来做这件事情。

   ROLE rolename

   把指定的角色添加为这个新角色的成员，把新角色做成一个"组"。

   ADMIN rolename

   ADMIN 子句类似 ROLE，只是给出的角色被增加到新角色 WITH ADMIN OPTION ，给他们以把这个角色的成员权限赋与其它角色的权力。

   RESOURCE QUEUE queue_name

   资源队列的名字，新的用户级别的角色将要赋给的它。只有有LOGIN权限的角色才能赋给一个资源队列。特殊的关键字NONE意味着这个角色赋给默认资源队列。一个角色只能属于一个资源队列。

   DENY deny_point

   DENY BETWEEN deny_point AND deny_point

   DENY和DENY BETWEEN关键字设置基于时间的约束，这些约束在登陆的时候执行。DENY设置拒绝访问的日期或日期和时间。DENY BETWEEN设置一个拒绝登陆的时间段。两者都使用如下格式的deny_point:

   DAY day [ TIME ‘time’]

   deny_point参数的两部分格式如下：

   day：

   {'Sunday' | 'Monday' | 'Tuesday' |'Wednesday' | 'Thursday' | 'Friday' | 'Saturday' | 0-6 }

   time：

   { 00-23 : 00-59 | 01-12 : 00-59 { AM | PM }}

   DENY BETWEEN子句使用两个deny_point	参数：

   DENY BETWEEN deny_point AND deny_point

   关于基于时间的约束的更多信息和示例，参见《Greenplum数据库管理员指南》中的“管理角色和权限”。

   **注解**

   添加或移除角色成员（管理组）的更好的方式是用GRANT和REVOKE。

   The VALID UNTIL 子句只是为口令定义一个失效时限，而不是角色自身的失效时限。当以非口令为基础的认证方式登录的时候，这个失效时间将失去意义。

   INHERIT 属性管理那些可赋予的权限的继承关系，也就是数据库对象的访问权限和角色成员的关系。它并不适用于 CREATE ROLE 和 ALTER ROLE 设置的特殊角色属性。比如，做为一个带有 CREATEDB 权限的角色成员，并不直接拥有创建数据库的能力，即使设置了 INHERIT 也如此。

   INHERIT 属性是缺省的，原因是为了向下兼容。在以前的Greenplum数据库版本里，用户总是拥有他们所在组的所有权限。不过，NOINHERIT 提供了与 SQL 标准所定义的最接近的语意。

   要注意 CREATEROLE 权限。因为对于 CREATEROLE 权限不存在继承的概念。这意味着即使一个角色没有某一权限，但是允许创建其他的角色，它可以很容易地创建一个与自身权限不同的另外一个角色（除了不能创建超级用户权限的角色）。例如，如果一个角色拥有 CREATEROLE 权限而无 CREATEDB 权限，那么他可以创建带有 CREATEDB 权限的新角色。因此具有 CREATEROLE 权限的角色几乎相当于一个超级用户。

   CONNECTION LIMIT选项绝不对超级用户执行。

   在这个指令中指定未加密的口令时必须小心。口令将被以明文方式传递给服务器，同时还可能在客户端的命令历史或服务端日志中被记录。而 createuser 会将命令加密后传递给服务器。同样，psql 包含一个 \password 可以用来安全的修改口令。

   **示例**

   创建一个可以登录的角色，但是不给他设置口令：

   CREATE ROLE jonathan LOGIN;

   创建一个角色，它属于一个资源队列：

   CREATE ROLE jonathan LOGIN RESOURCE QUEUE poweruser;

   创建一个带口令的角色，口令有效期到 2009 年底(CREATE USER 和 CREATE ROLE 一样，只不过它隐含 LOGIN)。

   CREATE USER joelle WITH PASSWORD 'jw8s0F4' VALID UNTIL '2009-01-01';

   创建一个可以创建数据库和管理其他角色的角色：

   CREATE ROLE admin WITH CREATEDB CREATEROLE;

   创建一个角色，不允许它在星期日登陆访问：

   CREATE ROLE user3 DENY DAY 'Sunday';

   **兼容性**

   SQL标准定义了用户和角色的概念，但是把它们当成完全独立的概念，并且让数据库的实现去指定定义用户的命令。在Greenplum数据库中用户和角色统一为一个单一类型的对象。因此，角色有了比标准中更多的属性。

   在SQL标准中有CREATE ROLE，但是标准仅需要如下的语法：

   CREATE ROLE name [ WITH ADMIN rolename ]

   允许多个初始的管理员，CREATE ROLE其他所有的选项都是Greenplum数据库的扩展。

   SQL 标准里声明的行为非常接近于给予用户 NOINHERIT 属性，而给予角色 INHERIT 属性。

   **参见**

   SET ROLE, ALTER ROLE, DROP ROLE, GRANT, REVOKE, CREATE RESOURCE QUEUE

2.46 CREATE RULE
-----------------------

   定义一个新的重写规则。

   **语法**
   
   CREATE [OR REPLACE] RULE name AS ON event

   TO table [WHERE condition]

   DO [ALSO | INSTEAD] { NOTHING | command | (command; command

   ...) }

   **描述**
   
   CREATE RULE 定义一个适用于特定表或者视图的新规则。CREATE OR REPLACE RULE 要么是创建一个新规则，要么是替换一个表上的同名规则。

   Greenplum数据库规则系统允许在更新、插入、删除时执行一个其它的预定义动作。规则就是在指定表上执行指定动作的时候，将导致一些额外的动作被执行。规则还可以用于视图。很重要的是，规则实际上只是一个命令转换机制，或者说命令宏。这种转换发生在命令开始执行之前。规则不像触发器，不能独立对每个物理航进行操作。

   ON SELECT 规则必须是无条件的 INSTEAD 规则并且必须有一个由单独一条 SELECT 查询组成的动作。因此，一条 ON SELECT 规则有效地把表转成视图，它的可见内容是规则的 SELECT 查询返回的记录而不是存储在表中的内容。写一条 CREATE VIEW 命令比创建一个表然后在上面定义一条 ON SELECT 规则的风格要好。

   你可以创建一个允许更新的视图的幻觉，方法是在视图上定义 ON INSERT, ON UPDATE, ON DELETE 规则，用合适的对其它表的更新替换在视图上更新的动作。

   如果你想在视图更新上使用条件规则，那么这里就有一个补充：对你希望在视图上允许的每个动作，你都必须有一个无条件的 INSTEAD 规则。如果规则是有条件的或者它不是 INSTEAD ，那么系统仍将拒绝执行更新动作，因为它认为最终会在视图的虚拟表上执行这个动作。如果你想处理条件规则上的所有有用的情况，那只需要增加一个无条件的 DO INSTEAD NOTHING 规则确保系统明白它决不会被调用来更新虚拟表就可以了。然后把条件规则做成非 INSTEAD ；在这种情况下，如果它们被触发，那么它们就增加到缺省的 INSTEAD NOTHING 动作中。不过这种方法目前不支持 RETURNING 查询。

   **参数**

   name

   创建的规则名。它必须在同一个表上的所有规则名字中唯一。同一个表上的同一个事件类型的多个规则是按照字母顺序运行的。

   event

   SELECT, INSERT, UPDATE, DELETE 事件之一。

   table

   规则作用的表或者视图的名字(可以有模式修饰)。

   condition

   任意返回 boolean 的 SQL 条件表达式。条件表达式除了引用 NEW 和 OLD 之外不能引用任何表，并且不能有聚集函数。NEW和OLD在引用表中引用值。NEW在ON INSERT和ON UPDATE规则引用将要插入或更新的新行时是有效的。OLD在ON UPDATE和ON DELETE规则引用将要更新或删除的已有行时是有效的。

   INSEAD

   指示使用该命令代替最初的命令。

   ALSO

   该命令应该在最初的命令执行之后一起执行。

   如果既没有声明 ALSO 也没有声明 INSTEAD ，那么 ALSO 是缺省。

   command

   组成规则动作的命令。有效的命令是 SELECT, INSERT, UPDATE, DELETE语句。特殊的表名NEW和OLD会用来引用在已引用表中的值。NEW在ON INSERT和ON UPDATE规则引用将要插入或更新的新行时是有效的。OLD在ON UPDATE和ON DELETE规则引用将要更新或删除的已有行时是有效的。

   **注解**

   为了在表上定义或修改规则，你必须是该表的拥有者。

   小心避免计算规则是很重要的。递归规则在规则创建时是无效的，但是在规则执行时会报错。

   **示例**

   创建一个规则，当一个用户试图给分区父表rank插入数据时，会插入到子表b2001中：

   CREATE RULE b2001 AS ON INSERT TO rank WHERE gender='M' and

   year='2001' DO INSTEAD INSERT INTO b2001 VALUES (NEW.id,

   NEW.rank, NEW.year, NEW.gender, NEW.count);

   **兼容性**

   CREATE RULE是Greenplum数据库的语言扩展，它是整个的查询重写系统。

   **参见**

   DROP RULE, CREATE TABLE, CREATE VIEW

2.47 CREATE SCHEMA
--------------------

   定义一个新的模式。

   **语法**

   CREATE SCHEMA schema_name [AUTHORIZATION username]

   [schema_element [ ... ]]

   CREATE SCHEMA AUTHORIZATION rolename [schema_element [ ... ]]

   **描述**

   CREATE SCHEMA为当前数据库创建一个新的模式。模式名必须在当前数据库的已有模式中唯一。

   一个模式基本上就是一个名字空间：它包含命名对象（表，数据类型，函数，和运算符），这些命名对象的名字可以和其他模式中的对象重名。命名对象要么是通过用模式名作为前缀"修饰"进行访问，要么是通过设置一个搜索路径包含所需要的模式。一条带着无修饰对象名的 CREATE 命令都是在当前模式中创建的(在搜索路径最前面的模式；可以用 current_schema 函数来判断)。

   另外，CREATE SCHEMA 可以包括在新模式中创建对象的子命令。这些子命令和那些在创建完模式后发出的命令没有任何区别，只不过是如果使用了 AUTHORIZATION 子句，那么所有创建的对象都将被该用户拥有。

   **参数**

   schema_name

   要创建的模式名字。如果省略，则使用用户名作为模式名。这个名字不能以 ka_ 开头，因为这样的名字保留给系统模式使用。

   rolename

   将拥有该模式的用户名。如果省略，缺省为执行该命令的用户名。只有超级用户才能创建不属于自己的模式。

   schema_element

   一个 SQL 语句，定义一个要在模式里创建的对象。目前，只有 CREATE TABLE, CREATE VIEW, CREATE INDEX, CREATE SEQUENCE, CREATE TRIGGER, GRANT 是可以接受的子句。其它类型的对象可以在创建完模式之后的独立命令里创建。

   注意：Greenplum数据库不支持触发器。

   **注解**

   要创建模式，调用该命令的用户必需在当前数据库上有 CREATE 权限，是超级用户。

   **示例**

   创建一个模式：

   CREATE SCHEMA myschema;

   为角色joe创建一个模式（这个模式也命名为joe）：

   CREATE SCHEMA AUTHORIZATION joe;

   **兼容性**

   SQL标准允许在 CREATE SCHEMA 里面有一个 DEFAULT CHARACTER SET 子句以及比目前Greenplum数据库可以接受的更多的子命令。

   SQL 标准声明在 CREATE SCHEMA 里的子命令可以以任意顺序出现。目前 Greenplum数据库里的实现还不能处理所有子命令里需要提前引用的情况；有时候可能需要重排一下子命令的顺序以避免前向引用。

   在 SQL 标准里，模式的所有者总是拥有其中的所有对象。Greenplum数据库允许模式包含非模式所有者所有的对象。只有在模式所有者把CREATE权限赋给了其他人才可能出现。

   **参见**

   ALTER SCHEMA, DROP SCHEMA

2.48 CREATE SEQUENCE
----------------------------

   定义一个新的序列发生器。

   **语法**

   CREATE [TEMPORARY | TEMP] SEQUENCE name

   [INCREMENT [BY] value]

   [MINVALUE minvalue | NO MINVALUE]

   [MAXVALUE maxvalue | NO MAXVALUE]

   [START [ WITH ] start]

   [CACHE cache]

   [[NO] CYCLE]

   [OWNED BY { table.column | NONE }]

   **描述**

   CREATE SEQUENCE 将创建一个新的序列号生成器。包括创建和初始化一个新的特殊单行表。生成器将被使用此命令的用户所有。

   如果给出了一个模式名，那么该序列就在给定的模式中创建的。否则它会在当前模式中创建。临时序列存在于一个特殊的模式中，因此创建临时序列的时候不能给出模式名。序列名必需和同一模式中的其它序列、表、索引、视图的名字不同。

   在创建序列后，你可以使用 nextval, currval, setval 函数操作序列。比如，可以插入序列的下一值到表中的一行中：

   INSERT INTO distributors VALUES (nextval(‘myserial’), ‘acme’);

   你也可以使用函数setval去操作一个序列，但这仅适用于不操作分布数据的查询。比如，下列查询是允许的，因为它重置master上的序列发生器的值：

   SELECT setval(‘myserial’, 201);

   但是下列查询会被Greenplum数据库拒绝，因为它操作分布数据：

   INSERT INTO product VALUES (setval('myserial', 201), 'gizmo');

   在一个非分布式的常规数据库上，在序列上运行的函数去本地序列表取得需要的值。然而，在Greenplum数据库中，请记住，每个segment是自己单独的数据库进程。因此，segment为了序列值需要一个单点事实，以便所有的segment正确增加，序列在以正确的顺序向前移动。运行在master上的序列服务进程是Greenplum分布式数据库中序列的单点事实。运行时segment从master得到序列值。

   因为这种分布式序列设计，Greenplum数据库中的运行在序列上的函数有一些限制：

   * lastval和currval函数不支持。

   * setval只能设置master上的系列发生器的值，不用用在更新分布式数据记录的子查询中。

   * nextval有时候会从master得到一堆值让一个segment使用，这取决于查询。因此，如果这堆值在segment级别不需要，有时候序列中的值会跳过。注意一个常规的PostgreSQL数据库也会这样，因此这对Greenplum数据库来讲不是特殊情况。


   尽管你不能直接更新一个序列，但你可以使用

   SELECT * FROM sequence_name;

   检查一个序列的参数和当前状态。特别是序列的 last_value 字段显示了任意后端进程最后分配的数值。

   **参数**

   TEMPORARY 或 TEMP

   如果声明了这个修饰词，那么该序列对象只为这个会话创建，并且在会话结束的时候自动删除。在临时序列存在的时候，除非用模式修饰的名字引用，否则同名永久序列是不可见的(在同一会话里)。

   name

   将要创建的序列名(可以用模式修饰)

   increment

   为了得到新值而增加到当前值去的值。一个正数将生成一个递增的序列，一个负数将生成一个递减的序列。缺省值是 1 。

   minvalue

   NO MINVALUE

   指定序列的最小值。如果没有声明这个子句或者声明了 NO MINVALUE，就会使用默认值。递增序列的缺省为 1 ，递减序列的缺省为 -263-1 。

   maxvalue

   NO MAXVALUE

   指定序列的最大值。如果没有声明这个子句或者声明了 NO MAXVALUE，就会使用默认值。那么递增序列的缺省为 263-1 ，递减序列的缺省为 -1 。

   start

   允许序列从任意位置开始。缺省初始值对于递增序列为 minvalue ，对于递减序列为 maxvalue 。

   cache

   为快速访问而在内存里预先存储多少个序列号。最小值(也是缺省值)是 1 ，也就是说没有缓存。

   CYCLE

   NO CYCLE

   用于使序列到达 maxvalue 或 minvalue 时可循环并继续下去。也就是如果达到极限，生成的下一个数据将分别是 minvalue（递增序列） 或 maxvalue（递减序列） 。

   如果声明了 NO CYCLE ，那么在序列达到其最大值之后任何对 nextval 的调用都强返回一个错误。如果既没有声明 CYCLE 也没有声明 NO CYCLE ，那么 NO CYCLE 是缺省。

   OWNED BY table.column

   OWNED BY NONE

   将序列关联到一个特定的表字段上。这样，在删除那个字段或其所在表的时候将自动删除绑定的序列。指定的表和序列必须被同一个用户所拥有，并且在在同一个模式中。默认的 OWNED BY NONE 表示不存在这样的关联。

   **注解**

   序列是基于 bigint 运算的，因此其范围不能超过八字节的整数范围(-9223372036854775808 到 9223372036854775807)。在一些古老的平台上可能没有对八字节整数的编译器支持，这种情况下序列使用普通的 integer 运算范围(-2147483648 到 +2147483647)。

   尽管系统保证为多个会话分配独立的序列值，但是如果考虑所有会话，那么这个数值可能会丢失顺序。比如，如果 cache 为 10 ，那么会话 A 保留了 1..10 并且返回 nextval=1 ，然后会话 B 可能会保留 11..20 然后在会话 A 生成 nextval=2 之前返回 nextval=11 。因此，你应该假设 nextval 值总是保持唯一，却不按顺序生成。同样，last_value 将反映任何会话保留的最后数值，不管它是否曾被 nextval 返回。

   **示例**

   创建一个名为myseq的序列：

   CREATE SEQUENCE myseq START 101;

   将下一个值插入到表中：

   INSERT INTO distributors VALUES (nextval('myseq'), 'acme');

   重置master上的序列值：

   SELECT setval(‘myseq’, 201);

   Greenplum数据库中非法使用setval（在分布式数据上设置序列值）：

   INSERT INTO product VALUES (setval(‘myseq’, 201), ‘gizmo’);

   **兼容性**

   CREATE SEQUENCE 遵循 SQL ，只有下面的例外：

   * 还不支持标准的 AS data_type 表达式。

   * 使用 nextval() 函数而不是标准的 NEXT VALUE FOR 表达式获取下一个数值。

   * OWNED BY 子句是 Greenplum数据库的扩展。

   **参见**

   ALTER SEQUENCE, DROP SEQUENCE

2.49 CTEATE TABLE
-----------------------

   定义一个新表。

   注意：参照完整性语法（外键约束）可以接受但是不强制。

   **语法**

   CREATE [[GLOBAL | LOCAL] {TEMPORARY | TEMP}] TABLE table_name (

   [ { column_name data_type [ DEFAULT default_expr ]

   [column_constraint [ ... ]

   [ ENCODING ( storage_directive [,...] ) ]

   ]

   | table_constraint

   | LIKE other_table [{INCLUDING | EXCLUDING}

   {DEFAULTS | CONSTRAINTS}] ...}

   [, ... ] ]

   )

   [ INHERITS ( parent_table [, ... ] ) ]

   [ WITH ( storage_parameter=value [, ... ] )

   [ ON COMMIT {PRESERVE ROWS | DELETE ROWS | DROP} ]

   [ TABLESPACE tablespace ]

   [ DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY ]

   [ PARTITION BY partition_type (column)

   [ SUBPARTITION BY partition_type (column) ]

   [ SUBPARTITION TEMPLATE ( template_spec ) ]

   [...]

   ( partition_spec )

   | [ SUBPARTITION BY partition_type (column) ]

   [...]

   ( partition_spec

   [ ( subpartition_spec

   [(...)]

   ) ]

   )

   column_constraint 是:

   [CONSTRAINT constraint_name]

   NOT NULL | NULL

   | UNIQUE [USING INDEX TABLESPACE tablespace]

   [WITH ( FILLFACTOR = value )]

   | PRIMARY KEY [USING INDEX TABLESPACE tablespace]

   [WITH ( FILLFACTOR = value )]

   | CHECK ( expression )

   | REFERENCES table_name [ ( column_name [, ... ] ) ]

   [ key_match_type ]

   [ key_action ]

   列的storage_directive是:

   COMPRESSTYPE={ZLIB | QUICKLZ | RLE_TYPE | NONE}

   [COMPRESSLEVEL={0-9} ]

   [BLOCKSIZE={8192-2097152} ]

   表的storage_parameter 是:

   APPENDONLY={TRUE|FALSE}

   BLOCKSIZE={8192-2097152}

   ORIENTATION={COLUMN|ROW}

   CHECKSUM={TRUE|FALSE}

   COMPRESSTYPE={ZLIB|QUICKLZ|RLE_TYPE|NONE}

   COMPRESSLEVEL={0-9}

   FILLFACTOR={10-100}

   OIDS[=TRUE|FALSE]

   table_constraint是:

   [CONSTRAINT constraint_name]

   UNIQUE ( column_name [, ... ] )

   [USING INDEX TABLESPACE tablespace]

   [WITH ( FILLFACTOR=value )]

   | PRIMARY KEY ( column_name [, ... ] )

   [USING INDEX TABLESPACE tablespace]

   [WITH ( FILLFACTOR=value )]

   | CHECK ( expression )

   | FOREIGN KEY ( column_name [, ... ] )

   REFERENCES table_name [ ( column_name [, ... ] ) ]

   [ key_match_type ]

   [ key_action ]

   [ key_checking_mode ]

   where key_match_type is:

   MATCH FULL

   | SIMPLE

   key_action是:

   ON DELETE

   | ON UPDATE

   | NO ACTION

   | RESTRICT

   | CASCADE

   | SET NULL

   | SET DEFAULT

   key_checking_mode是:

   DEFERRABLE

   | NOT DEFERRABLE

   | INITIALLY DEFERRED

   | INITIALLY IMMEDIATE

   partition_type是:

   LIST

   | RANGE

   partition_specification是:

   partition_element [, ...]

   partition_element是:

   DEFAULT PARTITION name

   | [PARTITION name] VALUES (list_value [,...] )

   | [PARTITION name]

   START ([datatype] 'start_value') [INCLUSIVE | EXCLUSIVE]

   [ END ([datatype] 'end_value') [INCLUSIVE | EXCLUSIVE] ]

   [ EVERY ([datatype] [number | INTERVAL] 'interval_value') ]

   | [PARTITION name]

   END ([datatype] 'end_value') [INCLUSIVE | EXCLUSIVE]

   [ EVERY ([datatype] [number | INTERVAL] 'interval_value') ]

   [ WITH ( partition_storage_parameter=value [, ... ] ) ]

   [ TABLESPACE tablespace ]

   subpartition_spec或template_spec是:

   subpartition_element [, ...]

   and subpartition_element is:

   DEFAULT SUBPARTITION name

   | [SUBPARTITION name] VALUES (list_value [,...] )

   | [SUBPARTITION name]

   START ([datatype] 'start_value') [INCLUSIVE | EXCLUSIVE]

   [ END ([datatype] 'end_value') [INCLUSIVE | EXCLUSIVE] ]

   [ EVERY ([datatype] [number | INTERVAL] 'interval_value') ]

   | [SUBPARTITION name]

   END ([datatype] 'end_value') [INCLUSIVE | EXCLUSIVE]

   [ EVERY ([datatype] [number | INTERVAL] 'interval_value') ]

   [ WITH ( partition_storage_parameter=value [, ... ] ) ]

   [ TABLESPACE tablespace ]

   列的storage_parameter是:

   APPENDONLY={TRUE|FALSE}

   BLOCKSIZE={8192-2097152}

   ORIENTATION={COLUMN|ROW}

   CHECKSUM={TRUE|FALSE}

   COMPRESSTYPE={ZLIB|QUICKLZ|RLE_TYPE|NONE}

   COMPRESSLEVEL={1-9}

   FILLFACTOR={10-100}

   OIDS[=TRUE|FALSE]

   **描述**

   CREATE TABLE在当前数据库中创建一个初始为空的表。发出这个命令的用户会拥有这个表。

   如果你给出一个模式名，那么Greenplum会在指定的模式中创建表。否则，Greenplum会在当前模式中创建表。临时表处于特殊模式中，因此，当你想创建一个临时表时不能指定模式名。同一个模式中，表名必须和其他的表，外部表，序列，索引或视图的名字不同。

   可选的约束子句指定了完成insert或update操作时行必须满足的条件。约束时一个SQL对象，它用来各种情况下表中的合法值。约束适用于表，但不适用于分区。你不能为分区或子分区指定约束。

   可以接受参照完整性约束（外键）但是不强制。这个信息会保留在系统中，否则会被忽略。

   有两种定义约束的方式：表约束和列约束。列约束是列定义的一部分。表约束的定义不会绑定到一个具体的列上，它可以作用于多个列上。每一个列约束也可以写成一个表约束；当一个约束仅仅影响一列时，列约束只是提供了表示上的方便。

   创建表时，有额外的子句用来声明Greenplum数据库的分布式策略。如果不提供DISTRIBUTED BY或DISTRIBUTED RANDOMLY子句，Greenplum会为表指定一个哈希分布式策略，用主键或表的第一列作为分布式键。地理列或用户定义数据类型的列也可以作为表的分布式键。如果表中没有能作为分布式键的列，表中的记录会基于随机分布或循环分布来分布。为了保证Greenplum数据库系统中的数据分布均匀，你想选一个对每一个记录都是唯一的分布式键，如果这不可能做到，可以选择DISTRIBUTED RANDOMLY。

   PARTITION BY子句允许你把一个表分成多个子表（或部分），这些子表合起来组成了父表，并且和父表在同一模式下。尽管子表是独立存在的，Greenplum数据库在重要方面会限制它的使用。在内部，分区时作为继承的特殊形式来实现。每个字分区表创建的时候都会有一个CHECK约束，它会基于某些定义的标准来限制表中能够包含的数据。CHECK约束也会被查询优化器使用，用来决定在满足某一个查询谓词时应该扫描哪一个分区。这些分区约束是由Greenplum数据库自动管理的。

   **参数**

   GLOBAO | LOCAL

   这些关键字存在是为了SQL兼容性，在Greenplum数据库中没有效果。

   TEMPORARY | TEMP

   创建为临时表。临时表在会话结束或(可选)当前事务的结尾(参阅下面的 ON COMMIT)自动删除。除非用模式修饰的名字引用，否则现有的同名永久表在临时表存在期间，在本会话过程中是不可见的。任何在临时表上创建的索引也都自动是临时的。

   table_name

   要创建的表的名字(可以用模式修饰)。

   column_name

   在新表中要创建的字段名字

   data_type

   该字段的数据类型。它可以包括数组说明符。

   对包含文本数据的列来说，Pivotal推荐指定数据为VARCHAR或TEXT类型。不推荐指定为CHAR类型。在Greenplum数据库中，VARCHAR和TEXT会把填充到数据中的字符（添加到最后一个非空白字符后面的字符）当成重要字符，但是CHAR不会，参见注解。

   DEFAULT default_expr

   DEFAULT 子句给字段指定缺省值。该数值可以是任何不含变量的表达式(不允许使用子查询和对本表中的其它字段的交叉引用)。缺省表达式的数据类型必须和字段类型匹配。缺省表达式将被用于任何未声明该字段数值的插入操作。如果没有指定缺省值则缺省值为 NULL 。

   ENCODING ( storage_directive [, …])

   对列而言，ENCODING子句指定压缩类型和列数据的块大小。关于COMPRESSTYPE, COMPRESSLEVEL和BLOCKSIZE值，参见存储选项。

   这个子句只对追加优化和面向列的表有效。

   列压缩设置会从表级继承到分区级，也会继承到子分区级。最低级别的设置有有优先权。

   INHERITS

   可选的 INHERITS 子句声明一系列的表，这个新表自动从这一系列表中继承所有字段。使用 INHERITS 将在子表和其父表之间创建一个永久的关系。对父表结构的修改通常也会传播到子表。缺省时，扫描父表的时候也会扫描子表。

   在Greenplum数据库中，INHERITS子句不会用在创建分区表上。尽管在分区层级中也有继承的概念，一个分区表的继承结构是用PARTITION BY子句创建的。

   如果在多个父表中存在同名字段，那么就会报告一个错误，除非这些字段的数据类型在每个父表里都是匹配的。如果没有冲突，那么重复的字段在新表中融合成一个字段。如果列出的新表字段名和继承字段同名，那么它的数据类型也必须和继承字段匹配，并且这些字段定义会融合成一个。不过，新字段可以声明不同于同名继承字段的约束：所有继承过来的约束以及新声明的约束都融合到一起，并且同时作用于新表。如果新表为该字段明确声明了缺省值，那么此缺省值将覆盖任何继承字段的缺省值。否则，该字段的所有父字段缺省值都必须相同，否则就会报错。

   LIKE other_table [ { INCLUDING | EXCLUDING } { DEFAULTS | CONSTRAINTS } ]

   LIKE 子句声明一个表，新表自动从这个表里面继承所有字段名、数据类型、非空约束和分布式策略。存储属性像追加优化或分区结构不会继承。和 INHERITS 不同，新表与原来的表之间在创建动作完毕之后是完全无关的。

   字段缺省表达式只有在声明了 INCLUDING DEFAULTS 之后才会包含进来。缺省是不包含缺省表达式的，结果是新表中所有字段的缺省值都是 NULL 。

   非空约束将总是复制到新表中，CHECK 约束则仅在指定了 INCLUDING CONSTRAINTS 的时候才复制，而其他类型的约束则永远也不会被复制。此规则同时适用于表约束和列约束——当请求约束时，所有的检查约束会被复制。

   和 INHERITS 不同，被复制的列和约束并不使用相同的名字进行融合。如果明确的指定了相同的名字或者在另外一个 LIKE 子句中，那么将会报错。

   CONSTRAINT constraint_name

   可选的列约束或表约束的名字。如果约束本身是非法的，那么其名字将会出现在错误信息中，因此像 col must be positive 这样的名字可以用来向客户程序表达有用的约束信息。（如果约束名中含有空格则必须用双引号界定。）如果没有指定约束名，那么系统将会自动生成一个名字。

   注意：指定的constraint_name用于约束，但是系统产生的唯一名字用于索引名字。在之前的版本中，提供的名字既可以用于约束名，也可以用于索引名。

   NULL | NOT NULL

   指定此列是否允许包含null值。NULL是默认值。

   UNIQUE (column constraint)

   UNIQUE ( column_name [, ... ] ) (table constraint)

   UNIQUE 约束表示表里的一个或多个字段只能包含唯一值。表的唯一约束的行为和列的约束一样，只不过多了跨多行的能力。对于唯一约束而言，NULL 被认为是互不相等的。唯一列必须含有Greenplum分布式键的所有列。此外，如果表是分区表，<key>必须包含分区键的所有列。注意，分区表中的<key>约束和简单的UNIQUE INDEX是不同的。

   PRIMARY KEY ( column constraint )

   PRIMARY KEY ( column_name [, ... ] ) ( table constraint )

   主键约束表明表中的一个或者一些字段只能包含唯一(不重复)的非 NULL 值。从技术上讲，PRIMARY KEY 只是 UNIQUE 和 NOT NULL 的组合，不过把一套字段标识为主键同时也体现了模式设计的元数据，因为主键意味着可以拿这套字段用做行的唯一标识。要为某个表设置主键，表必须是哈希分布的（而不是随机分布），而且主机要包含Greenplum分布式键的所有列。如果表是分区表，<key>必须包含分区键的所有列。注意一个分区表中的<key>约束和简单的UNIQUE INDEX是不同的。

   CHECK ( expression )

   CHECK 子句声明一个生成布尔结果的表达式，每次将要插入或更新操作必须必须满足这个表达式才能成功。结果为真或未知的表达式能成功。如果插入或更新操作的任何一行产生一个FALSE的结果，系统会抛出一个异常并且插入和更新操作不会修改数据库。声明为字段约束的检查约束应该只引用该字段的数值，而在表约束里出现的表达式可以引用多个字段。目前，CHECK 表达式不能包含子查询也不能引用除当前行字段之外的变量。

   REFERENCES table_name [ ( column_name [, ... ] ) ]

   [ key_match_type ] [ key_action ]

   FOREIGN KEY ( column_name [, ... ] )

   REFERENCES table_name [ ( column_name [, ... ] )

   [ key_match_type ] [ key_action [ key_checking_mode ]

   REFERENCES和FOREIGN KEY子句指定了参照完整性约束（外键约束）。Greenplum接受PostgreSQL语法的参照完整性约束，但是不强制。关于参照完整性约束的更多信息，参见PostgreSQL文档。

   WITH ( storage_option = value )

   WITH子句用来为表或索引设置存储选项。注意你也可以通过在分区声明中指定WITH子句来为特定的分区或子分区设置存储参数。最低级别的设置有优先级。

   一些表的存储选项的默认值可以用服务器配置参数gp_default_storage_options来指定。关于设置存储选项默认值的更多信息，参见注解。

   可用的存储选项如下：

   APPENDONLY——设为TRUE表示创建的表是一个追加优化表。如果是FALSE或不声明，创建的表就是一个常规的堆存储表。

   BLOCKSIZE——设置表的每个数据库的大小，单位字节。BLOCKSIZE的大写必须介于8192字节和2097152字节之间，或者8192字节的倍数。默认值是32768。

   ORIENTATION——设置为column表示是面向列存储，设置为row（默认值）表示面向行的存储。这个选项只在APPENDONLY=TRUE时才有效。堆存储表只能是面向行的。

   CHECKSUM——这个选项只对追加优化表有效（APPENDONLY=TRUE）。默认值是TRUE，用CRC检查和来验证追加优化表。当数据库创建的时候系统会计算检查和，并把检查和存储在磁盘上。当读取块的时候，系统会验证检查和。如果读取时计算的检查和与存储的检查和不匹配，事务终止。如果你把这个值设置为FALSE，禁用检查和验证，系统不会检查磁盘上的数据是否正确。

   COMPRESSTYPE——压缩类型可以设置为ZLIB（默认值），RLE-TYPE或QUICKLZ。NONE值可以禁用压缩。QUICKLZ用更少的CPU，相比ZLIB，它压缩速度更快，压缩比更低。反过来讲，ZLIB压缩速度更低，压缩比更高。这个选项只在APPENDONLY=TRUE时有效。

   只有在ORIENTATION=column时，才支持RLE_TYPE，Greenplum数据库使用运行长度编码（RLE）压缩算法。当相同的数据出现在连续的行上时，RLE压缩算法比ZLIB和QUICKLZ更好。

   对类型为BIGINT,INTEGER, DATE, TIME或TIMESTAMP列，如果COMPRESSTYPE选项设置为RLE-TYPE压缩，增量压缩也是适用的。增量压缩算法是基于连续行中的列值的增量的，设计它是用来在数据以排好序的方式加载时提升压缩，或者用在有序列数据的压缩上。

   关于表压缩的更多信息，请参见《Greenplum数据库管理员指南》中的“选择表存储模型”。

   COMPRESSLEVEL——对于追加优化表的zlib压缩来说，这个值可以设置为1（最快压缩）到9（最高压缩比）之间的一个整数。QuickLZ压缩的压缩水平只能设为1。如果不声明这个值，默认是1。对RLE_TYPE来说，这个值可以设置为1（最快压缩）到4（最高压缩比）之间的一个整数。

   这个选项只在APPENDONLY=TRUE时有效。

   FILLFACTOR——关于这个索引存储选项的更多信息，参见CREATE INDEX。

   OIDS——指定了 OIDS=FALSE，系统不会为行分配 OID 。Greenplum强烈推荐在创建表的时候不要打开OIDS选项。在大表上，就像Greenplum数据库中的表一样，对表行使用OIDS会导致32-bit OID 计数器循环。一旦计数器发生循环之后 OID 将不能被视为唯一的，这不仅会大大降低 OID 的实用性，而且会在Greenplum数据库系统的系统表中引起问题。另外，排除了 OID 的表也为每条记录减小了 4 字节的存储空间，从而可以稍微提升一些性能。OIDS在分区表和追加优化的列存储表上是不允许的。

   ON COMMIT

   使用ON COMMIT，可以控制临时表在事务块结束时的行为。有三个选项：

   PRESERVE ROWS——在事务结尾不对临时表采取特殊行动。这是默认行为。

   DELETE ROWS——在每一个事务块的结尾会删除临时表中所有的行。事实上，在每次提交时会自动执行TRUNCATE。

   DROP——在当前事务块结尾会自动删除临时表。

   TABLESPACE tablespace

   创建新表的表空间。如果不指定，会使用数据库的默认表空间。

   USING INDEX TABLESPACE tablespace

   这个子句允许选择与一个 表空间，UNIQUE 或 PRIMARY KEY 约束相关的索引会创建在这个表空间。如果没有提供这个子句，那么将使用数据库的缺省表空间。

   DISTRIBUTED BY (column, [ ... ] )

   DISTRIBUTED RANDOMLY

   用来为表声明Greenplum数据库分布式策略。DISTRIBUTED BY使用哈希索引，一个或多个列会声明为分布式键。对大多数的均匀数据分布来说，分布式键应该是表的主键或唯一列（或列组）。如果不能设置DISTRIBUTED BY，那么你可以选择DISTRIBUTED RAMDOMLY，它会把数据循环发给每一个segment。

   如果在创建表时没有指定DISTRIBUTED BY子句，Greenplum数据库服务器的配置参数gp_create_table_random_default_distribution会控制表分布式策略的默认值。如果分布式策略没有指定，Greenplum会遵循这些规则来建表的。

   如果这个参数的值是off（默认情况），Greenplum数据库会基于这个命令来选择表的分布式策略。在创建表的命令中如果指定LIKE或INHERITS子句，创建的新表会使用和源表或父表相同的分布式策略。

   如果这个值设置为on，Greenplum数据库遵循下列规则：

   * 如果没有PRIMARY KEY或UNIQUE列，表是随机分布的（DISTRIBUTED RANDOMLY）。即使表的创建命令中有LIKE或INHERITS子句，表依然是随机分布的。

   * 如果指定了PRIMARY KEY或UNIQUE列，也必须指定DISTRIBUTED BY子句。如果在创建表的命令中没有指定DISTRIBUTED BY子句，这个命令会失败。

   关于这个参数的更多信息，请参考“服务器配置参数”。

   PARTITION BY

   声明一个或多个列来对表进行分区。

   当创建分区表时，Greenplum数据库用给定的表名创建根分区表（根分区）。Greenplum数据库也会创建一个表、子表组成的层级结构，子表是根据指定的分区选项创建的子分区。Greenplum数据库的系统视图pg_partition*包含子分区表的信息。

   对每一个分区的级别（表的每一个层级），分区表可以用最大32767个分区。

   注意：Greenplum数据库会把分区表的数据存储在叶子子分区上，也就是分区层级结构的最低一级的表上。

   partition_type

   声明了分区类型：LIST（一列值）或RANGE（一个数字或日期范围）。

   partition_specification

   声明要建的分区。每个分区可以单独定义，对范围分区，你可以用EVERY子句（包含START子句和可选的END子句）定义一个增量模式来创建单个分区。


   DEFAULT PARTITION name——声明默认分区。当数据不匹配任何已有的分区时，就会插入到默认分区中去。如果分区设计中没有默认分区，那么和已有的分区无法匹配的行就会被拒绝。

   PARTITION name——声明分区的名字。分区创建时遵循以下约定：parentname_level#_prt_givenname。

   VALUES——对列表分区来说，定义分区包含的值。

   START——对范围分区来说，定义分区开始的范围值。默认情况下，开始值是INCLUSIVE的。比如，你声明了起始日期是’2008-01-01’， 那么分区就会包含大于等于’2008-01-01’的所有日期。通常，START表达式的数据类型和分布式键列的数据类型相同。如果不相同，你必须显式转成想要的数据类型。

   END——对范围分区，定义分区的结束范围值。默认情况下，结束值是EXCLUSIVE的。比如，你声明了结束日期是’2008-02-01’， 那么分区就会包含小于等于’2008-02-01’的所有日期。通常，END表达式的数据类型和分布式键列的数据类型相同。如果不相同，你必须显式转成想要的数据类型。

   EVERY——对范围分区来说，定位为了创建每一个分区，如何从START往END增加值。通常，EVERY表达式的数据类型和分布式键列的数据类型相同。如果不相同，你必须显式转成想要的数据类型。

   WITH——为分区设置存储选项。比如，你也许希望老一些的分区时追加优化表，而新一些的分区是常规的堆表。

   TABLESPACE——指定创建分区的表空间。

   SUBPARTITION BY

   声明一列或多列用来对第一级的分区进行子分区。子分区的格式和上面的分区格式一致。

   SUBPARTITION TEMPLATE

   你可以指定一个子分区模板来创建子分区（低一级的叶子表），而不用一个一个定义子分区。这个子分区规范适用于所有的父分区。

   **注解**

   * 在Greenplum数据库中（一个基于PostgreSQL的系统）VARCHAR和TEXT会把文本数据的填充空白（添加到最后一个非空白符后的空白符）当成重要字符，但是CAHR不会这样处理。

   在Greenplum数据库中，CHAR类型会填充尾部空白到指定的宽度n。这个值会被存储和显示成空白。然而，这些填充空白不会当成语法上重要的字符。当这个值被分布时，尾部空白会被忽视。当比较两个CHAR类型的值也不会考虑尾部空白，当要把CHAR类型字符转成另外一种字符串类型时尾部空白会被移除。

   * 不建议在新应用中使用 OID ，可能情况下，更好的选择是使用一个 SERIAL 或者其它序列发生器做表的主键。如果一个应用使用了 OID 标识表中的特定行，那么建议在该表的 oid 字段上创建一个唯一约束，以确保该表的 OID 即使在计数器循环之后也是唯一的。要避免假设 OID 是跨表唯一的；如果你需要一个整个数据库范围的唯一标识，你可以用表OID 和行 OID 的组合来实现这个目的。

   * 关于Greenplum表中的分布式键列，Greenplum数据库对主键和唯一约束有特殊条件。对于Greenplum数据库中的唯一约束，表必须是哈希分布的（而不是DISTRIBUTED RANDOMLY），约束列必须和表的分布式键列相同。分布式键必须是正确排序的约束列的左子集。比如，如果主键是(a, b, c)，分布式键只能是下列之一：(a), (a, b), 或(a, b, c)。

   主键约束可以简单看成唯一约束和非空约束的结合。

   Greenplum数据库自动为每一个唯一约束或主键约束创建索引，以便强制唯一性。因此，不需要显式为主键列创建索引。

   Greenplum数据库不支持外键约束。

   对继承表，唯一约束，主键约束，索引和表权限在当前的实现中不会被继承。

   * 对追加优化表来说，UPDATE和DELETE不允许出现在顺序事务中，会导致事务中止。CLUSTER，DECLARE…FORUPDATE和触发器是追加优化表不支持的。

   * 要插入数据到分区表中，你必须指定根分区表，也就是用CREATE TABLE命令创建的那个表。你可以在INSERT命令中指定一个分区表的叶子子表。。如果对指定的叶子子表来说数据是不合法的，那么会返回错误。不支持在INSERT命令中指定非叶子子表。在分区表的任何子表上执行像UPDATE和DELETE这样的DML命令是不支持的。这些命令必须在根分区上执行。

   * 下列表的存储选项的默认值可以用服务器配置参数gp_default_storage_option来指定：

   	APPENDONLY

   	BLOCKSIZE

   	CHECKSUM

   	COMPRESSTYPE

   	COMPRESSLEVEL

   	ORIENTATION

   这些默认值可以为数据库，模式和用户设置。关于存储选项设置的更多信息，请参考服务器配置参数gp_default_storage_options。

   重要：目前的Greenplum数据库中，传统优化器允许有多个列作为分区键的列表分区。Pivotal查询优化器不支持组合键，因此Pivotal不推荐使用组合分区键。

   **示例**

   创建一个名为rank的表，在模式baby中，使用列rank, gender和year分布数据：

   CREATE TABLE baby.rank (id int, rank int, year smallint, gender char(1), count int) DISTRIBUTED BY (rank, gender, year);

   创建表films和表distributors（默认主键会用作Greenplum分布式键）：

   CREATE TABLE films (

   code char(5) CONSTRAINT firstkey PRIMARY KEY,

   title varchar(40) NOT NULL,

   did integer NOT NULL,

   date_prod date,

   kind varchar(10),

   len interval hour to minute

   );

   CREATE TABLE distributors (

   did integer PRIMARY KEY DEFAULT nextval('serial'),

   name varchar(40) NOT NULL CHECK (name <> '')

   );

   创建一个GZIP压缩的，追加优化表：

   CREATE TABLE sales (txn_id int, qty int, date date)

   WITH (appendonly=true, compresslevel=5)

   DISTRIBUTED BY (txn_id);

   创建一个三级的分区表，使用子分区模板，每一级都有默认分区：

   CREATE TABLE sales (id int, year int, month int, day int,

   region text)

   DISTRIBUTED BY (id)

   PARTITION BY RANGE (year)

   SUBPARTITION BY RANGE (month)

   SUBPARTITION TEMPLATE (

   START (1) END (13) EVERY (1),

   DEFAULT SUBPARTITION other_months )

   SUBPARTITION BY LIST (region)

   SUBPARTITION TEMPLATE (

   SUBPARTITION usa VALUES ('usa'),

   SUBPARTITION europe VALUES ('europe'),

   SUBPARTITION asia VALUES ('asia'),

   DEFAULT SUBPARTITION other_regions)

   ( START (2002) END (2010) EVERY (1),

   DEFAULT PARTITION outlying_years);

   **兼容性**

   CREATE TABLE命令符合SQL标准，有以下例外：

   * 临时表——在标准里，临时表只是定义一次并且从空内容开始自动存在于任何需要它们的会话中。Greenplum数据库要求每个会话为它们使用的每个临时表发出它们自己的 CREATE TEMPORARY TABLE 命令。这样就允许不同的会话将相同的临时表名字用于不同的目的，而标准的实现方法则把一个临时表名字约束为具有相同的表结构。

   标准中的全局和局部临时表之间的区别在Greenplum数据库里不存在。Greenplum数据库将接受临时表声明中的 GLOBAL 和 LOCAL 关键字，但是他们没有任何作用。

   如果忽略了ON COMMIT 子句，SQL 标准声明缺省的行为是 ON COMMIT DELETE ROWS。但是Greenplum数据库里的缺省行为是 ON COMMIT PRESERVE ROWS 。在 SQL 标准里不存在 ON COMMIT DROP 选项。

   * 列检查约——SQL 标准说 CHECK 列约束只能引用他们作用的字段；只有 CHECK 表约束才能引用多个字段。Greenplum数据库并不强制这个限制；它把字段和表约束看作相同的东西。

   * NULL约束——NULL 约束是Greenplum数据库对 SQL 标准的扩展，它是为了和其它一些数据库系统兼容以及为了和 NOT NULL 约束对称而存在的。因为它是任何字段的缺省，所以它是不需要出现的。

   * 继承——通过 INHERITS 子句的多重继承是Greenplum数据库语言的扩展。SQL:1999 及以后的标准使用不同的语法和语义定义了单继承。SQL:1999风格的继承还没有在Greenplum数据库中实现。

   * 分区——用PARTITION BY子句对表进行分区是Greenplum数据库的语言扩展。

   * 零字段表——Greenplum数据库允许创建没有字段的表(比如 CREATE TABLE foo();)。这是对 SQL 标准的扩展，标准不允许存在零字段表。零字段表本身没什么用，但是禁止他们会给 ALTER TABLE DROP COLUMN 带来很奇怪的情况，所以，Greenplum忽视标准的限制。

   * WITH 子句——WITH 子句是Greenplum数据库的扩展，同样，存储参数和 OID 也是扩展。

   * 表空间——Greenplum数据库的表空间概念不是标准的东西。因此 TABLESPACE 和 USING INDEX TABLESPACE 都是扩展。

   * 数据分布——Greenplum数据库的并行或分布式数据库的概念是标准的东西。DISTRIBUTED子句是扩展。

   **参见**

   ALTER TABLE, DROP TABLE, CREATE EXTERNAL TABLE, CREATE TABLE AS
   
2.50 CREATE TABLE AS
--------------------------

   从查询的结果来建立表。

   **语法**

   CREATE [ [GLOBAL | LOCAL] {TEMPORARY | TEMP} ] TABLE table_name

   [(column_name [, ...] )]

   [ WITH ( storage_parameter=value [, ... ] ) ]

   [ON COMMIT {PRESERVE ROWS | DELETE ROWS | DROP}]

   [TABLESPACE tablespace]

   AS query

   [DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY]

   storage_parameter是：

   APPENDONLY={TRUE|FALSE}

   BLOCKSIZE={8192-2097152}

   ORIENTATION={COLUMN|ROW}

   COMPRESSTYPE={ZLIB|QUICKLZ}

   COMPRESSLEVEL={1-9 | 1}

   FILLFACTOR={10-100}

   OIDS[=TRUE|FALSE]

   **描述**

   CREATE TABLE AS 创建一个表并且用来自 SELECT 命令的结果填充该表。该表的字段和 SELECT 输出字段的名字及类型相关。不过你可以通过明确地给出一个字段名字列表来覆盖 SELECT 输出字段的名字。

   它创建一个新表并且只对查询计算一次来填充这个新表。新表不能跟踪源表的变化。

   **参数**

   GLOBAL | LOCAL

   这些关键字的存在是为了和SQL标准兼容，在Greenplum数据库中没有作用。

   TEMPORARY | TEMP

   如果指定，新表会是一个临时表。临时表在会话结束时会自动删除，或者也可以在当前事务结束时删除（参见ON COMMIT）。已有的同名永久表在临时表存在期间对当前会话不可见，除非它们用模式名来修饰。在临时表上创建的任何索引也自动是临时的。

   table_name

   要创建的新表的名字。

   column_name

   字段的名称。如果没有提供字段名字，那么就从查询的输出字段名中获取。如果表是从一个 EXECUTE 命令创建的，那么就不能声明字段名列表。

   WITH ( storage_parameter = value)

   WITH子句用来为表或索引设置存储选项。注意你也可以通过在分区声明中指定WITH子句来为特定的分区或子分区设置存储参数。

   可用的存储选项如下：

   APPENDONLY——设为TRUE表示创建的表是一个追加优化表。如果是FALSE或不声明，创建的表就是一个常规的堆存储表。

   BLOCKSIZE——设置表的每个数据库的大小，单位字节。BLOCKSIZE的大写必须介于8192字节和2097152字节之间，或者8192字节的倍数。默认值是32768。

   ORIENTATION——设置为column表示是面向列存储，设置为row（默认值）表示面向行的存储。这个选项只在APPENDONLY=TRUE时才有效。堆存储表只能是面向行的。

   COMPRESSTYPE——压缩类型可以设置为ZLIB（默认值）\或QUICKLZ。QUICKLZ用更少的CPU，相比ZLIB，它压缩速度更快，压缩比更低。反过来讲，ZLIB压缩速度更低，压缩比更高。这个选项只在APPENDONLY=TRUE时有效。

   COMPRESSLEVEL——对于追加优化表的zlib压缩来说，这个值可以设置为1（最快压缩）到9（最高压缩比）之间的一个整数。QuickLZ压缩的压缩水平只能设为1。如果不声明这个值，默认是1。这个选项只在APPENDONLY=TRUE时有效。

   FILLFACTOR——关于这个索引存储选项的更多信息，参见CREATE INDEX。

   OIDS——指定了 OIDS=FALSE，系统不会为行分配 OID 。Greenplum强烈推荐在创建表的时候不要打开OIDS选项。在大表上，就像Greenplum数据库中的表一样，对表行使用OIDS会导致32-bit OID 计数器循环。一旦计数器发生循环之后 OID 将不能被视为唯一的，这不仅会大大降低 OID 的实用性，而且会在Greenplum数据库系统的系统表中引起问题。另外，排除了 OID 的表也为每条记录减小了 4 字节的存储空间，从而可以稍微提升一些性能。OIDS列存储表上是不允许的。

   ON COMMIT

   使用ON COMMIT，可以控制临时表在事务块结束时的行为。有三个选项：

   PRESERVE ROWS——在事务结尾不对临时表采取特殊行动。这是默认行为。

   DELETE ROWS——在每一个事务块的结尾会删除临时表中所有的行。事实上，在每次提交时会自动执行TRUNCATE。

   DROP——在当前事务块结尾会自动删除临时表。

   TABLESPACE tablespace

   在其中创建新表的表空间。如果不指定，使用数据库的默认表空间。

   AS query

   一个SELECT或VALUES命令，或者一个EXECUTE命令，它运行一个准备好的SELECT或VALUES查询。

   DISTRIBUTED BY (column, [ ... ] )

   DISTRIBUTED RANDOMLY

   用来为表声明Greenplum数据库分布式策略。DISTRIBUTED BY使用哈希索引，一个或多个列会声明为分布式键。对大多数的均匀数据分布来说，分布式键应该是表的主键或唯一列（或列组）。如果不能设置DISTRIBUTED BY，那么你可以选择DISTRIBUTED RAMDOMLY，它会把数据循环发给每一个segment。

   如果在创建表时没有指定DISTRIBUTED BY子句，Greenplum数据库服务器的配置参数gp_create_table_random_default_distribution会控制表分布式策略的默认值。如果分布式策略没有指定，Greenplum会遵循这些规则来建表的。

   * 如果传统的查询优化器创建表，而且这个参数的值是off，表的分布式策略会基于这个命令来决定。

   * 如果传统的查询优化器创建表，而且这个参数的值是on，表的分布式策略是随机的。

   * 如果Pivotal查询优化器创建表，表的分布式策略是随机的。这个参数没有作用。

   关于这个参数的更多信息，请参考“服务器配置参数”。关于传统查询优化器和Pivotal查询优化器的更多信息，参见《Greenplum数据库管理员指南》中的“查询数据”。

   **注解**

   这个命令的功能和SELECT INTO类似，但是更建议你用这个命令，因为它不太可能和 SELECT INTO 语法的其它方面混淆。另外，CREATE TABLE AS 提供了 SELECT INTO 功能的超集。

   CREATE TABLE AS可以用来快速从外部表数据源导入数据。参见CREATE EXTERNAL TABLE。

   **示例**

   创建一个只包含表 films 中最近的记录的新表 films_recent ：

   CREATE TABLE films_recent AS SELECT * FROM films WHERE date_prod >= '2007-01-01';

   使用预备语句创建一个只包含表 films 中最近的记录的新临时表 films_recent ，该临时表包含 OID 并且在事务结束时将被删除：

   PREPARE recentfilms(date) AS SELECT * FROM films WHERE date_prod > $1;

   CREATE TEMP TABLE films_recent WITH (OIDS) ON COMMIT DROP AS

   EXECUTE recentfilms('2007-01-01');

   **兼容性**

   CREATE TABLE AS 兼容 SQL 标准，只有下面几个区别：

   * 标准要求在子查询子句周围有圆括弧，在Greenplum数据库里，这些圆括弧是可选的。

   * 标准定义了一个 WITH [ NO ] DATA 子句；这个目前在Greenplum数据库里还没有实现。Greenplum数据库提供的行为相当于标准的 WITH DATA 情况。WITH NO DATA 在查询后面附加 LIMIT 0 进行模拟。

   * Greenplum数据库处理临时表的方法和标准相差较大；参阅 CREATE TABLE 获取细节。

   * WITH 子句是Greenplum数据库的扩展，并且 SQL 标准中也没有存储参数和 OID 。

   * Greenplum数据库表空间的概念也不是标准的一部分。因此 TABLESPACE 子句也是扩展。

   **参见**

   CREATE EXTERNAL TABLE, EXECUTE, SELECT, SELECT INTO, VALUES

2.51 CREATE TABLESPACE
----------------------------

   定义一个表空间。

   **语法**

   CREATE TABLESPACE tablespace_name [OWNER username]

   FILESPACE filespace_name

   **描述**

   CREATE TABLESPACE 为Greenplum数据库系统注册一个新的表空间。表空间的名字必须和系统中已有的表空间的名字不同。

   表空间允许超级用户在文件系统中定义一个位置，这个位置可以存放包含数据库对象(比如表和索引)的数据文件。

   一个用户，如果有合适的权限，就可以把一个表空间的名字传递给CREATE DATABASE, CREATE TABLE, CREATE INDEX, ADD CONSTRAINT，这样就让这些对象的数据文件存储在指定的表空间里。

   在Greenplum数据库中，必须位master，每一个主segment和每一个镜像Segment都定义一个文件系统位置，以便表空间有一个位置可以跨整个Greenplum系统来存储它的对象。这一组文件系统位置定义在一个文件空间对象中。在创建一个表空间之前，必须有一个文件空间。参阅《Greenplum数据库工具指南》获取关于gpfilespace的更多信息。

   **参数**

   tablespace

   表空间的名字。不能以pg_或gp_开头，这些名字是保留给系统表空间的。

   OWNER username

   表空间所有者的名字。如果省略，默认是执行这个命令的用户。只有超级用户才能创建表空间，但是它们也可以把表空间的所有权赋给一个非超级用户。

   FILESPACE

   用kafilespace管理工具定义Greenplum数据库文件空间的名字。

   **注解**

   在一个文件空间用在表空间上之前必须定义这个文件空间。参阅《Greenplum数据库工具指南》获取关于gpfilespace的更多信息。

   只有支持符号链接的系统才支持表空间。

   在一个事务块中不能执行CREATE TABLESPACE。
  
   **示例**

   通过指定一个对应的文件空间来创建一个新的表空间：

   CREATE TABLESPACE mytblspace FILESPACE myfilespace;

   **兼容性**

   CREATE TABLESPACE是一个Greenplum数据库的扩展。

   **参见**

   CREATE DATABASE, CREATE TABLE, CREATE INDEX, DROP TABLESPACE, ALTER TABLESPACE, 《Greenplum数据库工具指南》中的gpfilespace。

2.52 CREATE TYPE
---------------------

   定义一个新的数据类型。

   **语法**

   CREATE TYPE name AS ( attribute_name data_type [, ... ] )

   CREATE TYPE name (

   INPUT = input_function,

   OUTPUT = output_function

   [, RECEIVE = receive_function]

   [, SEND = send_function]

   [, INTERNALLENGTH = {internallength | VARIABLE}]

   [, PASSEDBYVALUE]

   [, ALIGNMENT = alignment]

   [, STORAGE = storage]

   [, DEFAULT = default]

   [, ELEMENT = element]

   [, DELIMITER = delimiter] )

   CREATE TYPE name

   **描述**

   CREATE TYPE 为当前数据库注册一个新的数据类型。定义该类型的用户成为其所有者。

   如果给出模式名，那么该类型是在指定模式中创建。否则它将在当前模式中创建。类型名必需和同一模式中任何现有的类型或者域不同。类型名也不能和同模式中的表名字冲突。

   复合类型

   第一种形式的 CREATE TYPE 创建一个复合类型。复合类型是通过一列属性名和数据类型声明的。这样实际上和一个表的行类型一样，但是如果只是想定义一个类型，那么使用 CREATE TYPE 就可以避免直接创建实际的表。一个独立的复合类型做一个函数的参数或者返回类型是非常有用的。

   基本类型

   第二种形式的CREATE TYPE 创建一种新的基本类型(标量类型)。参数可以按任意顺序出现，而不是上面显示的那样，并且大多数都是可选的。它要求要在定义类型之前先用 CREATE FUNCTION 注册两个或更多个函数。支持函数 input_function 和 output_function 是必须的，而函数 receive_function, send_function, analyze_function 是可选的。通常，这些函数必须用 C 或者其它低层语言编写。在Greenplum数据库中，任何用来实现数据类型的函数都必须定义为IMMUTABLE的。

   input_function 函数将该类型的外部文本形式转换成可以被该类型的操作符和函数识别的内部形式。output_function 用途相反。输入函数可以声明为接受一个类型为 cstring 的参数，或者接受三个类型分别为 cstring, oid, integer 的参数。第一个参数是 C 字符串形式的输入文本，第二个参数是该类型自身的 OID(数组类型除外，这种情况下它们接受自身元素的类型 OID)，第三个是目标字段的 typmod(如果未知则传递 -1)。输入函数必须返回一个自身数据类型的值。通常，输入函数应当被声明为 STRICT ；否则当读取 NULL 输入时会使用NULL为第一个参数进行调用。在这种情况下，该函数必须仍然返回 NULL 或报错。这个特性主要是为了支持域输入函数，这种函数可能需要拒绝 NULL 输入。输出函数必须被声明为接受一个新数据类型的参数，并且必须返回 cstring 类型。输出函数不会被使用 NULL 调用。

   可选的 receive_function 把该类型的外部二进制表现形式转换成内部表现形式。如果没有提供这个函数，那么该类型不能用二进制输入。二进制格式应该选取那种比较容易转换同时还有一定移植性的内部格式。比如，标准的整数数据类型使用网络字节序作为外部的二进制表现形式，而内部表现形式则是机器的本机字节序。接收函数应该进行适当的检查，以确保这个值是有效的。接收函数应该声明为接受一个类型为 internal 的参数，或者是三个类型分别为 internal, oid, integer 的参数。第一个参数是一个指向一个 StringInfo 缓冲区的、保存接收的字节串的指针；可选的参数和文本输入函数一样。接收函数必须返回一个该类型的数据值。通常接收函数应当被声明为 STRICT ，否则否则当读取 NULL 输入时将被使用NULL为第一个参数进行调用，并且必须仍然返回 NULL 或报错。这个特性主要是为了支持域接收函数，这种函数可能需要拒绝 NULL 输入。同样，可选的 send_function 把类型的内部表现形式转换为外部二进制表现形式。如果没有提供这些函数，那么类型就不能用二进制方式输出。发送函数必须声明为接收一个新数据类型并且必须返回 bytea 结果。发送函数不会被以 NULL 值调用。

   这个时候你应该觉得奇怪，输入和输出函数怎么可以声明为返回新类型的结果或者是接受新类型的参数，而且是在新类型创建之前就需要创建它们。答案是类型必须被首先定义为一个壳类型，它只是一个除了名称和属主之外没有其他属性的占位符类型。这可以通过没有其他额外参数的 CREATE TYPE name 命令来完成。然后就可以引用该壳类型定义输入输出函数。最后，CREATE TYPE 把这个壳类型替换成完整的、有效的类型定义，这样就可以使用新类型了。

   尽管新类型的内部表现形式只有输入输出函数和其它你创建来使用该类型的函数了解，但内部表现形式还是有几个属性必须为Greenplum数据库声明。internallength 是最重要的一个。基本数据类型可定义成为定长，这时 internallength 是一个正整数，也可以是变长的，这时用把 internallength 设为 VARIABLE 表示。(在内部，这个状态是通过将 typlen 设置为-1 实现的。)所有变长类型的内部形式都必须以一个四字节整数开头，这个整数给出此类型这个数值的全长。

   可选的标记 PASSEDBYVALUE 表明该类型的数值是按值而不是引用传递。你不能传递那些内部形式大于 Datum 类型尺寸(大多数机器上是 4 字节，有些是 8 字节)的数据类型的值。

   alignment 参数声明该数据类型要求的对齐存储方式。允许的数值等效于按照 1, 2, 4, 8 字节边界对齐。请注意变长类型必须有至少 4 字节的对齐，因为它们必须包含一个 int4 作为第一个部分。

   storage 参数允许为变长数据类型选择存储策略。(定长类型只允许使用 plain。) plain 声明该数据类型总是用内联的方式而不是压缩的方式存储。extended 声明系统将首先试图压缩一个长的数据值，然后如果它仍然太长的话就将它的值移出主表。external 声明允许将它的值移出主表，但是系统不会尝试压缩。main 允许压缩，但是不赞成把数值移动出主表。(如果实在不能放在一行里的话，这种存储策略的数据项仍将移动出主表，它比 extended 和 external 项更愿意保存在主表里。)

   如果用户希望该数据类型的字段的缺省值不是 NULL，可以指定一个缺省值。在DEFAULT 关键字里缺省值。(这个缺省值可以被附着在特定字段上的 DEFAULT 子句覆盖)。

   为了声明一个数组类型，用 ELEMENT 关键字声明数组元素的类型。比如，ELEMENT = int4 定义了一个 4 字节整数(int4)的数组。有关数组类型的更多细节在下面描述。

   可用 delimiter 指定用于这种类型数组的外部形式的数值之间的分隔符。缺省的分隔符是逗号(,)。请注意分隔符是和数组元素类型相关联，而不是数组类型本身。

   数组类型

   在创建用户定义数据类型的时候，Greenplum数据库自动创建一个与之关联的数组类型，其名字由该基本类型的名字前缀一个下划线组成。分析器理解这个命名约定，并且把对类型为 foo[] 的字段的请求转换成对类型为 _foo 的字段的请求。这个隐含创建的数组类型是变长并且使用内建的 array_in 和 array_out 输入和输出函数。

   你很可能会问如果系统自动制作正确的数组类型，那为什么还要有个 ELEMENT 选项?使用 ELEMENT 的唯一场合是：你定义的定长类型碰巧在内部是一个许多相同事物的数组，而你又想允许这 N 个事物可以通过下标直接访问，除了你想为这个类型整体提供的其他操作。比如，构成 name 类型的 char 成员就允许用这种方法访问。一个二维的 point 类型也可以允许其两个浮点型构成成员按照类似 point[0] 和 point[1] 的方法访问。请注意这个功能只适用于定长类型，并且其内部形式是一个相同定长域的序列。一个可以下标化的变长类型必须有被 array_in 和 array_out 使用的一般化的内部表现形式。出于历史原因，定长数组类型的下标从 0 开始，而不是像变长类型那样的从 1 开始。

   **参数**

   name

   将要创建的类型名(可以有模式修饰)

   attribute_name

   复合类型的一个属性(字段)的名字

   data_type

   要成为一个复合类型的字段的现有数据类型的名字

   input_function

   一个函数的名称，将数据从外部文本形式转换成内部格式。

   output_function

   一个函数的名称，将数据从内部格式转换成适于显示的外部文本形式。

   receive_function

   把数据从类型的外部二进制形式转换成内部形式的函数名称

   send_function

   把数据从类型的内部形式转换成外部二进制形式的函数名称

   internallength

   一个数值常量，说明新类型的内部表现形式的长度。缺省假定它是变长的。

   alignment

   该数据类型的存储对齐要求。如果声明了，必须是 char, int2, int4(缺省), double 之一。

   storage

   该数据类型的存储策略。必须是 plain(缺省), external, extended, main 之一。

   default

   该类型的缺省值。若省略则为 NULL

   element

   被创建的类型是数组；这个声明数组元素的类型。

   delimiter

   此类型数组元素之间的分隔符。

   **注解**

   用户定义类型名不能以下划线(_)开头而且只能有 62 个字符长 (或者说是 NAMEDATALEN-2 个而不是其它名字那样的可以有 NAMEDATALEN-1 个字符)。以下划线开头的类型名被解析成内部创建的数组类型名。

   因为一旦类型被创建之后对它的使用就没有限制，所以创建一个基本类型就等价于授予所有用户执行类型定义中指定的各个函数的权限。该类型的创建者也将是这些函数的拥有者。这对于大多数类型定义中指定的函数来说不会造成什么不良问题。但是如果你设计的新类型在内部形式和外部形式之间转换的时候使用"敏感信息"，那么你仍然要再三考虑、多加小心。

   在Greenplum数据库2.4之前的版本中，并不存在 CREATE TYPE name 语法。创建新的基本类型之前必须首先创建其输入函数。这样，Greenplum数据库将会首先把新类型的名字看作输入函数的返回类型。这种情况下会隐含创建壳类型，然后这个壳类型将被随后定义的输入输出函数引用。这种方法目前仍然被支持，但已经过时，将来可能不再支持。同样，为了避免函数定义中的简单输入错误而导致壳类型突然搞乱系统表，当输入函数用 C 语言书写时，将只能用这种方法创建壳类型。

   **示例**

   这个例子创建一个复合类型并且在一个函数定义中使用它：

   CREATE TYPE compfoo AS (f1 int, f2 text);

   CREATE FUNCTION getfoo() RETURNS SETOF compfoo AS $$
    
	SELECT fooid, fooname FROM foo

   $$ LANGUAGE SQL;

   这个命令创建 box 数据类型，并且将这种类型用于一个表定义：

   CREATE TYPE box;

   CREATE FUNCTION my_box_in_function(cstring) RETURNS box AS ... ;

   CREATE FUNCTION my_box_out_function(box) RETURNS cstring AS ... ;

   CREATE TYPE box (
    
	INTERNALLENGTH = 16,

    INPUT = my_box_in_function,
    
	OUTPUT = my_box_out_function

   );
   
   CREATE TABLE myboxes (
    
	id integer,

    description box
   
   );

   如果 box 的内部结构是一个四个 float4 的数组，可以使用

   CREATE TYPE box (
    
	INTERNALLENGTH = 16,

    INPUT = my_box_in_function,

    OUTPUT = my_box_out_function,

    ELEMENT = float4

   );

   来允许一个 box 的数值成分成员可以用下标访问。否则该类型和前面的行为一样。

   这条命令创建一个大对象类型并将其用于一个表定义：

   CREATE TYPE bigobj (
    
	INPUT = lo_filein, OUTPUT = lo_fileout,

    INTERNALLENGTH = VARIABLE

   );

   CREATE TABLE big_objs (
    
	id integer,

    obj bigobj

   );

   **兼容性**

   CREATE TYPE是Greenplum数据库的扩展。在 SQL 里有一个 CREATE TYPE 语句，但是细节和 PostgreSQL 有比较大区别。

   **参见**

   CREATE FUNCTION, ALTER TYPE, DROP TYPE, CREATE DOMAIN

2.53 CREATE USER
--------------------

   定义一个默认有LOGIN权限的数据库角色。

   **语法**

   CREATE USER name [  [ WITH ] option [ … ] ]

   option是：

   SUPERUSER | NOSUPERUSER

   | CREATEDB | NOCREATEDB

   | CREATEROLE | NOCREATEROLE

   | CREATEUSER | NOCREATEUSER

   | INHERIT | NOINHERIT

   | LOGIN | NOLOGIN

   | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'

   | VALID UNTIL 'timestamp'

   | IN ROLE rolename [, ...]

   | IN GROUP rolename [, ...]

   | ROLE rolename [, ...]

   | ADMIN rolename [, ...]

   | USER rolename [, ...]

   | SYSID uid | RESOURCE QUEUE queue_name

   **描述**

   在Greenplum2.2中，CREATE USER已经被CREATE ROLE替换了， 虽然依然接受它，是为了向后兼容。

   CREATE USER和CREATE ROLE唯一的区别就是CREATE USER是默认有LOGIN权限的，而CREATE ROLE默认是NOLOGIN。

   **兼容性**

   SQL标准中没有CREATE USER语句。

   **参见**

   CREATE ROLE

2.54 CREATE VIEW
--------------------

   定义一个新的视图。

   **语法**

   CREATE [OR REPLACE] [TEMP | TEMPORARY] VIEW name

   [ ( column_name [, ...] ) ]

   AS query

   **描述**

   CREATE VIEW 定义一个查询的视图。这个视图不是物理上实际存在的。相反，在该视图每次被引用的时候都会运行一次 query。

   CREATE OR REPLACE VIEW 类似，不过是如果一个同名的视图已经存在，那么将替换它。你只能用一个生成相同字段的新查询替换一个视图(字段名和数据类型都相同)。

   如果给出了一个模式名，那么该视图将在指定的模式中创建，否则将在当前模式中创建。临时视图存在于一个特殊的模式里，所以创建临时视图的时候，不用给出模式名。新视图名字必需和同一模式中任何其它视图、表、序列、索引的名字不同。

   **参数**

   TEMPORARY 或 TEMP

   如果声明了这个子句，那么视图就以临时视图的方式创建。临时视图在当前会话结束的时候将被自动删除。当前会话中，在临时视图存在的期间，将无法看到现有的同名关系，除非用模式修饰的名字引用它们。如果视图引用的任何基础表是临时的，那么视图将被创建为临时的(不管是否声明了 TEMPORARY)。

   name

   所要创建的视图名称(可以有模式修饰)

   column_name

   一个可选的名字列表，用作视图的字段名。如果没有给出，字段名取自查询。

   query

   一个将为视图提供行和列的 SELECT 或 VALUES 语句。

   **注解**

   Greenplumn数据库中的视图是只读的。系统将不允许在视图上插入、更新、删除数据。你可以通过在视图上创建把插入等动作重写为向其它表做合适操作的规则来实现可更新视图的效果。更多信息详见 CREATE RULE 。

   请注意视图字段的名字和类型一定是你们期望的那样。比如，

   CREATE VIEW vista AS SELECT 'Hello World';

   在两个方面很糟糕：字段名缺省是 ?column? 并且字段的数据类型缺省是 unknown 。如果你想让视图的结果是一个字符串文本，那么请像下面这样使用

   CREATE VIEW vista AS SELECT text 'Hello World' AS hello;

   访问视图中引用的表由视图的所有者决定，而不是当前用户（即使当前用户是超级用户）。这也许会迷惑超级用户，因为一般而言超级用户可以访问所有的对象。在视图中，如果用户不是视图的所有者，即使是超级用户，也必须显式赋权，才能访问视图中引用的表。

   不过，在视图里调用的函数当作他们直接从使用视图的查询里调用看待。因此，视图的用户必须有权限调用视图使用的所有函数。

   如果你创建视图的时候用了ORDER BY子句，当你从视图中SELECT时ORDER BY子句会被忽略。

   **示例**

   创建一个由所有喜剧电影组成的视图：

   CREATE VIEW comedies AS SELECT * FROM films WHERE kind = 'Comedy';

   创建一个视图，获取排名前十的小孩名字：

   CREATE VIEW topten AS SELECT name, rank, gender, year FROM names, rank WHERE rank < ‘11’ AND names.id=rank.id;

   **兼容性**

   SQL 标准为 CREATE VIEW 声明了一些附加功能，这些功能在Greenplumn数据库数据中没有。标准中完整的SQL命令包含以下可选子句：

   * CHECK OPTION——这个选项用于可更新视图。所有对视图的 INSERT 和 UPDATE 都要经过视图定义条件的校验。也就是说，新数据应该可以通过视图看到。如果没有通过校验，更新将被拒绝。

   * LOCAL——对这个视图进行完整性检查。

   * CASCADED——对此视图和任何相关视图进行完整性检查。在既没有声明 CASCADED 也没有声明 LOCAL 时，假设为 CASCADED 。

   CREATE OR REPLACE VIEW 是 Greenplumn数据库的语言扩展。临时视图的概念也是扩展。

   **参见**

   SELECT, DROP VIEW   
   
2.55 DEALLOCATE
-------------------

   删除一个预备语句。

   **语法**

   DEALLOCATE [PREPARE] name

   **描述**

   DEALLOCATE 用于删除前面编写的预备语句。如果你没有明确删除一个预备语句，那么它将在会话结束的时候被删除。

   有关预备语句的更多信息参阅 PREPARE。

   **参数**

   PREPARE

   这个关键字总被忽略。

   name

   将要删除的预备语句。

   **示例**

   删除一个名为insert_names的预定义预备语句：

   DEALLOCATE insert_names;

   **兼容性**

   SQL 包括一个 DEALLOCATE 语句，但它只是用于嵌入式 SQL 。

   **参见**

   EXECUTE, PREPARE

2.56 DECLARE
---------------

   定义一个游标。

   **语法**

   DECLARE name [BINARY] [INSENSITIVE] [NO SCROLL] CURSOR

   [{WITH | WITHOUT} HOLD]

   FOR query [FOR READ ONLY]

   **描述**

   DECLARE 允许用户创建游标，用于在一个大的查询里面检索少数几行数据。使用 FETCH ，游标可以返回文本或二进制格式。

   普通游标和 SELECT 一样返回文本格式。因为数据在系统内部是用二进制格式存储的，系统必须对数据做一定转换以生成文本格式。一旦数据是以文本形式返回，那么客户端应用需要把它们转换成二进制进行操作。另外，文本格式一般都比对应的二进制格式占用的存储空间大。二进制游标返回内部二进制形态的数据，可能更易于操作。当然，如果你想以文本方式显示数据，那么以文本方式检索会为你节约很多客户端的工作。

   比如，如果查询从某个整数列返回 1 ，在缺省的游标里将获得一个字符串 1 ，但在二进制游标里将得到一个 4 字节的包含该数值内部形式的数值(大端顺序)。

   应该小心使用二进制游标。一些客户端应用，包括 psql，是不能识别二进制游标的，它们期望返回的数据是文本格式。

   注意：如果客户端应用使用"扩展查询"协议发出 FETCH 命令，那么 Bind 协议声明数据是用文本还是用二进制格式检索。这个选择覆盖游标的定义。因此，在使用扩展查询协议的时候，二进制游标的概念已经过时了，任何游标都可以当作文本或者二进制的格式发出。

   在UPDATE或DELETE命令中可以通过WHERE CURRENT OF子句指定游标，用来更新或删除表数据。

   **参数**

   name

   将要创建的游标名

   BINARY

   令游标以二进制而不是文本格式返回数据

   INSENSITIVE

   表明从游标检索出来的数据不应该被游标之下的表的更新影响。在 Greenplum数据库里，所有游标都是不敏感的，这个关键字没有什么作用，提供它只是为了和 SQL 标准兼容。

   SCROLL

   NO SCROLL

   游标不能用来获取非连续行。这是Greenplum数据库中的默认行为，因为可滚动游标（SCROLL）是不支持的。

   WITH HOLD

   WITHOUT HOLD

   WITH HOLD声明该游标可以在创建它的事务成功提交后继续使用。WITHOUT HOLD 声明该游标不能在创建它的事务提交后使用。WITHOUT HOLD是默认值。

   query

   一个 SELECT 或 VALUES 命令，它提供游标返回的行。

   如果一个游标要用在UPDATE或DELETE命令的WHERE CURRENT OF子句中，它必须满足一下条件：

   * 不能引用视图或外部表。

   * 只能引用一个表。

     这个表必须是可更新的。比如，下面的表是不可更新的：表函数，设置返回函数，仅追加表，柱状表。

   * 不能包含下列情况：

   	分组子句。

   	集合操作比如UNION ALL或UNION DISTINCT。
        
		排序子句。

		窗口子句。

		联合或自联合。

   在SELECT命令中指定FOR UPDATE子句会阻止其他会话在预取记录和更新记录时改变记录。不加FOR UPDATE子句，如果从游标创建之后记录被改变过，那么接下来带有WHERE CURRENT OF子句的UPDATE和DELETE命令是无效的。

   **注意**

   在SELECT命令中指定FOR UPDATE子句会锁住整个表，不光是选定的记录。

   FOR READ ONLY

   FOR READ ONLY 表明游标将用于只读模式。

   **注解**

   如果没有声明 WITH HOLD ，那么这个命令创建的游标只能在当前事务中使用。这样，不带 WITH HOLD 的 DECLARE 在事务块外面没有任何用处：游标将一直存活到事务结束。因此，PostgreSQL 将在这个命令出现在事务块外面的时候报错。使用 BEGIN, COMMIT 和 ROLLBACK 定义一个事务块。

   如果声明了 WITH HOLD 并且创建该游标的事务成功提交，那么游标还可以在同一会话随后的事务里访问。但如果创建它的事务回滚，那么游标被删除。带 WITH HOLD 创建的游标是用一个明确的 CLOSE 命令或者是会话终止来关闭的。在目前的实现里，由一个游标代表的行是被拷贝到一个临时文件或者内存区里的，这样他们就仍然可以在随后的事务中被访问。

   如果你在一个事务中用DECLARE命令创建一个游标，在用CLOSE命令关闭游标之前，你不能在事务中使用SET命令。

   可滚动游标目前Greenplum数据库不支持。你只能用FETCH向前移动游标，而不能向后移动。

   DECLARE……FORUPDATE在追加优化表中不支持。

   可以通过查询 pg_cursors 系统视图看到所有可用游标。

   **示例**

   定义一个游标：

   DECLARE mycursor CURSOR FOR SELECT * FROM mytable;

   **兼容性**

   SQL 标准只允许在嵌入的 SQL 中和模块中使用游标。Greenplum数据库允许交互地使用游标。

   Greenplum数据库没有为游标实现OPEN语句。当游标创建后就是打开的。

   SQL标准允许游标向前向后移动。所有的Greenplum数据库只能向前移动（不是可滚动的）。

   二进制游标是Greenplum数据库扩展。

   **参见**

   CLOSE, DELETE, FETCH, MOVE, SELECT, UPDATE

2.57 DELETE
------------------

   从表中删除记录。

   **语法**

   DELETE FROM [ONLY] table [[AS] alias]

   [USING usinglist]

   [WHERE condition | WHERE CURRENT OF cursor_name ]

   **描述**

   DELETE从指定的表中删除满足WHERE子句的记录。如果没有WHERE子句，效果是删除表中所有的记录。结果是一个有效的空表。

   默认情况下，DELETE删除指定表及其所有子表中的记录。如果只想从指定的表中删除记录，可以使用ONLY子句。

   使用数据库中其它表的信息删除某个表中的数据行有两个办法：使用子查询，或者在USING 子句中指定额外的表。哪种技巧更合适取决于特定的环境。

   如果指定WHERE CURRENT OF子句，删除的记录是最近从指定的游标中频繁获取的记录。

   要对表进行删除，你必须对它有 DELETE 权限。

   输出

   如果执行成功，DELETE命令返回一个命令标签，格式如下：

   DELETE count

   count是删除的记录数。如果count是0，没有记录满足条件（这没有当成错误）。

   **参数**

   ONLY

   如果声明了这个选项，则只从指定的表中删除数据行。否则从指定的表及其所有子表中的删除。

   table

   一个现存表的名字(可以有模式修饰)。

   alias

   目标表的别名。如果提供了别名，那么它将完全掩盖实际的表名。例如给定 DELETE FROM foo AS f 之后，DELETE 语句的剩余部分必须使用 f 而不是 foo 来引用该表。

   usinglist

   表表达式列表，允许来自其他表的列出现在 WHERE 条件中。这与可以在 SELECT 命令的 FROM 子句中指定的表列表相似。例如，可以为该表的名字声明一个别名。不要在 usinglist 里重复目标表，除非你希望产生一个自连接。

   condition

   一个返回 boolean 值的表达式，用于判断哪些行需要被删除。

   cursor_name

   在WHERE CURRENT OF条件中使用的游标的名字。要删除的记录是最近这个游标中频繁获取的记录。游标必须是要删除的目标表上的一个简单查询（没有联结，没有聚集）。关于创建游标，请参考DECLARE。

   WHERE CURRENT OF 不能和逻辑条件一起指定。

   关于创建游标，请参考DECLARE。

   注解

   Greenplum数据库允许你在WHERE条件里引用其它表的字段，方法是在 USING 子句里声明其它表。比如，rank表中的Hannah名字，可以这么做：

   DELETE FROM rank USING names WHERE names.id = rank.id AND name=’Hannah’;

   这里实际发生的事情是在 rank 和 names 之间的一个连接，然后所有成功连接的行都标记为删除。这个语法不是标准的。然而，有时候连接风格比执行一个更加标准的子查询风格更容易写或者执行更快，比如：

   DELETE FROM rank WHERE id IN (SELECT id FROM names WHERE name = ‘Hannah’);

   当要删除一个表中所有记录时（比如：DELETE * FROM table），Greenplum增加了一个隐式的TRUNCATE命令（当用户权限允许的时候）。这个增加的TRUNCATE命令会释放删除的行占据的磁盘空间，不需要在表上执行VACCUM。这会提升后续查询的性能，也会使经常插入或删除临时表的ETL工作负载受益。

   直接在分区表的某一个分区上执行UPDATE和DELETE命令是不支持的。相反，这些命令必须在分区表的主表上执行，就是用CREATE TABLE命令创建的表。

   示例

   删除所有电影(films)但不删除音乐(musicals)：
   
   DELETE FROM films WHERE kind <> 'Musical';

   清空 films 表：

   DELETE FROM films;

   在DELETE中使用join：

   DELETE FROM tasks USING names WHERE names.id = rank.id AND name = 'Hannah';

   兼容性

   这个命令符合SQL标准，除了USING子句是GREENPLUM数据库的扩展。、

   参见

   DECLARE, TRUNCATE

2.58 DROP AGGREGATE
--------------------------

   删除聚集函数。

   **语法**

   DROP AGGREGATE [IF EXISTS] name ( type [, ...] ) [CASCADE | RESTRICT]

   **描述**

   DROP AGGREGATE 删除一个现存的聚集函数。执行这条命令的用户必须是该聚集函数的所有者。

   **参数**

   IF EXISTS

   如果指定的聚集不存在，那么发出一个 notice 而不是抛出一个错误。

   name

   现存的聚集函数名(可以有模式修饰)。

   type

   聚集函数操作的输入数据类型，要引用一个零参数聚集函数，请用 * 代替输入数据类型列表。

   CASCADE

   级联删除依赖于这个聚集函数的对象。

   RESTRICT

   如果有任何依赖对象，则拒绝删除这个聚集函数。这是缺省。

   **示例**

   将 integer 类型的聚集函数 myavg 删除：

   DROP AGGREGATE myavg(integer);

   **兼容性**

   SQL 标准里没有 DROP AGGREGATE 语句。

   **参见**

   ALTER AGGREGATE, CREATE AGGREGATE

2.59 DROP CAST
----------------

   删除一个转换。

   **语法**

   DROP CAST [IF EXISTS] (sourcetype AS targettype) [CASCADE | RESTRICT]

   **描述**

   DROP CAST 删除一个先前定义过的类型转换。要能删除一个类型转换，你必须拥有源或者目的数据类型。这是和创建一个类型转换相同的权限。

   **参数**

   IF EXISTS

   如果指定的转换不存在，那么发出一个 notice 而不是抛出一个错误。

   sourcetype

   类型转换里的源数据类型。

   targettype

   类型转换里的目标数据类型。

   CASCADE

   RESTRICT

   这些键字没有任何效果，因为在类型转换上没有依赖关系。

   **示例**

   删除从 text 到 int 的转换：

   DROP CAST (text AS int);

   **兼容性**

   DROP CAST 遵循 SQL 标准。

   **参见**

   CREATE CAST

2.60 DROP CONVERSION
----------------------

   删除一个转换。

   **语法**

   DROP CONVERSION [ IF EXISTS ] name [ CASCADE | RESTRICT ]

   **描述**

   DROP CONVERSION 删除一个先前定义过的编码转换。要想删除一个转换，你必须拥有该转换。

   **参数**

   IF EXISTS

   如果指定的转换不存在，那么发出一个 notice 而不是抛出一个错误。

   name

   编码转换的名字(可以用模式修饰)。

   CASCADE

   RESTRICT

   这些关键字没有作用，因为编码转换上没有依赖关系。

   **示例**

   删除一个叫做 myname 的编码转换：

   DROP CONVERSION myname;

   **兼容性**

   SQL 标准里没有 DROP CONVERSION 语句。

   **参见**

   ALTER CONVERSION, CREATE CONVERSION
   
2.61 DROP DATABASE
---------------------

   删除一个数据库。

   **语法**

   DROP DATABASE [ IF EXISTS ] name

   **描述**

   DROP DATABASE 删除一个数据库。删除一个现存数据库的目录入口并且删除包含数据的目录。只有数据库所有者能够执行这条命令。还有，如果你或者任何其他人正在与目标数据库连接，那么就不能执行这条命令。（要与template1或者任何其它数据库连接，再发出这条命令。）

   警告：DROP DATABASE 不能撤销，小心使用！

   **参数**

   IF EXISTS

   如果指定的数据库不存在，那么发出一个 notice 而不是抛出一个错误。

   name

   要被删除的现有数据库名。

   **注解**

   DROP DATABASE 不能在事务块中执行。

   这条命令在和目标数据库连接时不能执行。通常更好的做法是用 dropdb 程序代替，该程序是此命令的一个封装。

   **示例**

   删除名为testdb的数据库：

   DROP DATABASE testdb;

   **兼容性**

   SQL 标准里没有 DROP DATABASE 语句。

   **参见**

   ALTER DATABASE, CREATE DATABASE

2.62 DROP DOMAIN
-------------------

   删除域。

   **语法**

   DROP DOMAIN [IF EXISTS ] name [, ...]  [ CASCADE | RESTRICT ]

   **描述**

   DROP DOMAIN 将删除一个域。只有域的所有者才能删除它。

   **参数**

   IF EXISTS

   如果指定的域不存在，那么发出一个 notice 而不是抛出一个错误。

   name

   一个现有的域(可以有模式修饰)。

   CASCADE

   级联删除依赖该域的对象（比如表列）。

   RESTRICT

   如果有任何依赖对象存在，则拒绝删除此域。这个是缺省。

   **示例**

   删除zipcode域：

   DROP DOMAIN zipcode;

   **兼容性**

   这条命令兼容 SQL 标准，但 IF EXISTS 选项是一个Greenplum数据库扩展。

   **参见**

   ALTER DOMAIN, CREATE DOMAIN

2.63 DROP EXTERNAL TABLE
-------------------------------

   删除一个外部表的定义。

   **语法**

   DROP EXTERNAL [WEB] TABLE [IF EXISTS] name [CASCADE | RESTRICT]

   **描述**

   DROP EXTERNAL TABLE从数据库系统中删除一个已有的外部表的定义。外部数据源或文件不会被删除。只有外部表的所有者才能执行这个命令。

   **参数**

   WEB

   可选关键字，用来删除外部网络表。

   IF EXISTS

   如果外部表不存在，那么发出一个 notice 而不是抛出一个错误。

   name

   已有外部表的名字（可用模式名修饰）。

   CASCADE

   自动删除依赖于此外部表的对象（比如视图）。

   RESTRICT

   如果有对象依赖外部表，则拒绝删除。这是默认情况。

   **示例**

   如果存在外部表staging则删除它：

   DROP EXTERNAL TABLE IF EXISTS staging;

   **兼容性**

   SQL标准中没有DROP EXTERNAL TABLE语句。

   **参见**

   CREATE EXTERNAL TABLE
   
2.64 DROP FILESPACE
-------------------------

   删除一个文件空间。

   **语法**

   DROP FILESPACE [IF EXISTS] filespacename

   **描述**

   DROP FILESPACE删除一个文件空间的定义，以及系统为它产生的数据目录。

   文件空间只能由其所有者或超级用户来删除。在删除之前，文件空间中的表空间对象必须清除。很有可能当前数据库中没有表空间使用此文件空间了，但是其他数据库中的表空间还在使用这个文件空间。

   **参数**

   IF EXISTS 

   如果文件空间不存在，那么发出一个 notice 而不是抛出一个错误。

   tablespace name

   要删除的文件空间的名字。

   **示例**

   删除表空间myfs：

   DROP FILESPACE myfs;

   **兼容性**

   在SQL标准或PostgreSQL中没有DROP FILESPACE语句。

   **参见**

   ALTER FILESPACE, DROP TABLESPACE, 《Greenplum数据库工具指南》中的gpfilespace

2.65 DROP FUNCTION
-------------------

   删除一个函数。

   **语法**

   DROP FUNCTION [ IF EXISTS ] name ( [ [ argmode ] [ argname ] argtype [, ...] ] )

   [ CASCADE | RESTRICT ]

   **描述**

   DROP FUNCTION 将删除一个现存的函数。要执行这条命令，用户必须是函数的所有者。必须声明函数的参数类型，因为几个不同的函数可能会有同样的名字和不同的参数列表。

   **参数**

   IF EXISTS

   如果指定的函数不存在，那么发出一个 notice 而不是抛出一个错误。

   name

   现存的函数名称(可以有模式修饰)。

   argmode

   参数的模式：IN(缺省), OUT, INOUT 。请注意 DROP FUNCTION 实际上并不注意 OUT 参数，因为判断函数的身份只需要输入参数。因此列出 IN 和 INOUT 参数就足够了。

   argname

   参数的名字。请注意 DROP FUNCTION 实际上并不注意参数的名字，因为判断函数的身份只需要输入参数的数据类型。

   argtype

   如果有的话，是函数参数的类型(可以用模式修饰)。

   CASCADE

   级联删除依赖于函数的对象(比如操作符)。

   RESTRICT

   如果有任何依赖对象存在，则拒绝删除该函数。这个是缺省。

   **示例**

   这条命令删除平方根函数：

   DROP FUNCTION sqrt(integer);

   **兼容性**

   SQL 标准里定义了一个 DROP FUNCTION 语句。但和这条命令不兼容。

   **参见**

   CREATE FUNCTION, ALTER FUNCTION

2.66 DROP GROUP
-----------------------

   删除一个数据库角色。

   **语法**

   DROP GROUP [ IF EXISTS ] name [, ...]

   **描述**

   DROP GROUP是一个过时的命令，现在依然接受它是为了向后兼容。组（和用户）已经被更为一般的概念角色替代了。参阅DROP ROLE获取更多信息。

   **参数**

   IF EXISTS

   如果角色不存在，则抛出一个警告而不是一个错误。

   name

   已有的角色的名字。

   **兼容性**

   SQL 标准里没有 DROP GROUP 语句。

   **参见**

   DROP ROLE

2.67 DROP INDEX
--------------------

   删除索引。

   **语法**

   DROP INDEX [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]

   **描述**

   DROP INDEX 从数据库中删除一个现存的索引。要执行这个命令，你必须是索引的所有者。

   **参数**

   IF EXISTS

   如果指定的索引不存在，那么发出一个 notice 而不是抛出一个错误。

   name

   要删除的索引名(可以有模式修饰)。

   CASCADE

   级联删除依赖于该索引的对象。

   RESTRICT

   如果有依赖对象存在，则拒绝删除该索引。这个是缺省。

   **示例**

   此命令将删除 title_idx 索引：

   DROP INDEX title_idx;

   **兼容性**

   DROP INDEX 是 PostgreSQL 语言扩展。在 SQL 标准里没有索引的规定。

   **参见**

   ALTER INDEX, CREATE INDEX, REINDEX
   
2.68 DROP LANGUAGE
-------------------------

   删除一个过程语言

   **语法**

   DROP [ PROCEDURAL ] LANGUAGE [ IF EXISTS ] name [ CASCADE | RESTRICT ]

   **描述

   DROP LANGUAGE 将删除曾注册过的过程语言。删除语言必须是超级用户才可以。

   **参数**

   PROCEDURAL

   可选关键字，没有效果。

   IF EXISTS

   如果指定的过程语言不存在，那么发出一个 notice 而不是抛出一个错误。

   name

   现存语言的名称。出于向后兼容的考虑，这个名字可以用单引号包围。

   CASCADE

   级联删除依赖于该语言的对象(比如该语言写的函数)。

   RESTRICT

   如果存在依赖对象，则拒绝删除。这个是缺省。

   **示例**

   下面命令删除 plsample 语言：

   DROP LANGUAGE plsample;

   **兼容性**

   SQL 标准里没有 DROP LANGUAGE 语句。

   **参见**

   ALTER LANGUAGE, CREATE LANGUAGE

