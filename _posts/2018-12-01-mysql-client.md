---
layout: default-blog-md
title: mysql源码探究之mysql client
category: markdown
---

mysql命令行客户端源码位于client目录下，main函数位于mysql.cc文件里。  
main函数里首先进行一些初始化工作，包括读取配置参数、申请内存、初始化变量等，然后会调用read_and_execute函数，这个函数内部会做read-eval循环。  
read_and_execute函数在每次循环内，先调用readline读取用户输入，然后调用find_command来处理命令。
find_command函数里会拿用户输入在commands这个全局变量里查找。commands变量是COMMANDS结构体数组，里面定义了mysql支持的命令。
命令结构定义如下：
```c
typedef struct
{
  const char *name;                 /* User printable name of the function. */
  char cmd_char;                    /* msql command character */
  int (*func)(String *str, char *); /* Function to call to do the job. */
  bool takes_params;                /* Max parameters for command */
  const char *doc;                  /* Documentation for this function.  */
} COMMANDS;
```
commands数组里只有参数类命令才定义了func成员变量，而查找修改数据类命令的func成员都是0，由此可以推测出这些命令都是在服务端进行实际操作的。
所以对于`select * from table`这条语句，find_command函数无法找到合适的命令来处理，会继续调用add_line函数，内部会调用com_go函数。
com_go会调用mysql_real_query_for_lazy函数，经过多层调用，最后会调用simple_command函数，并传递COM_QUERY参数。
mysql支持的参数如下：
```c
enum enum_server_command
{
  COM_SLEEP, COM_QUIT, COM_INIT_DB, COM_QUERY, COM_FIELD_LIST,
  COM_CREATE_DB, COM_DROP_DB, COM_REFRESH, COM_SHUTDOWN, COM_STATISTICS,
  COM_PROCESS_INFO, COM_CONNECT, COM_PROCESS_KILL, COM_DEBUG, COM_PING,
  COM_TIME, COM_DELAYED_INSERT, COM_CHANGE_USER, COM_BINLOG_DUMP,
  COM_TABLE_DUMP, COM_CONNECT_OUT, COM_REGISTER_SLAVE,
  COM_STMT_PREPARE, COM_STMT_EXECUTE, COM_STMT_SEND_LONG_DATA, COM_STMT_CLOSE,
  COM_STMT_RESET, COM_SET_OPTION, COM_STMT_FETCH, COM_DAEMON,
  /* don't forget to update const char *command_name[] in sql_parse.cc */

  /* Must be last */
  COM_END
};
```
而simple_command函数只是一层包装，实际调用MYSQL结构体的methods成员变量的advanced_command成员函数。而这个成员函数是初始化时在mysql_real_connect函数里赋值的，实际值定义如下：
```c
MYSQL_METHODS embedded_methods= 
{
  emb_read_query_result,
  emb_advanced_command,
  emb_read_rows,
  emb_store_result,
  emb_fetch_lengths, 
  emb_flush_use_result,
  emb_read_change_user_result,
  emb_list_fields,
  emb_read_prepare_result,
  emb_stmt_execute,
  emb_read_binary_rows,
  emb_unbuffered_fetch,
  emb_free_embedded_thd,
  emb_read_statistics,
  emb_read_query_result,
  emb_read_rows_from_cursor
};
```
所以会调用emb_advanced_command函数，内部会调用dispatch_command函数。
之后调用mysql_store_result_for_lazy函数来读取结果，类似的，会调用mysql->methods->read_rows，即emb_read_rows函数。