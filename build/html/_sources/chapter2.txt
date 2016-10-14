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

   
   