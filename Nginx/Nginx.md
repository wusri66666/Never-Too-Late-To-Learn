# Nginx

## 1、常用命令

| 命令                  | 解释                                                         |
| --------------------- | ------------------------------------------------------------ |
| nginx -h              | 查看帮助命令                                                 |
| nginx -v              | 显示nginx版本（-v显示版本，-V显示版本，编译器版本和配置参数） |
| ps -aux \| grep nginx | 查看nginx进程                                                |
| nginx -s stop         | 暴力停止nginx（快速关闭，不管有没有正在处理的请求）          |
| nginx -s quit         | 优雅停止nginx（在退出前完成已经接受的连接请求）              |
| nginx -s reload       | 重启nginx，配置重载                                          |
| nginx -s reopen       | 重启nginx                                                    |
| nginx -c 文件路径     | nginx指定配置文件启动                                        |
| nginx -t              | 检查nginx配置文件正确性（不加-c，默认检查nginx.conf）        |

## 2、配置文件

### server_name

#### 匹配顺序

1. 精确匹配server_name
2. 通配符在开始时匹配server_name成功
3. 通配符在结束时匹配server_name成功
4. 正则表达式匹配server_name成功
5. 默认的default_server处理，如果没有指定默认找第一个server

### location

#### 语法

~~~
location [=|~|~*|^~|@] /uri/ {
  ...
} 

=    进行普通字符精确匹配
~    波浪线表示执行一个正则匹配，区分大小写
~*   表示执行一个正则匹配，不区分大小写
^~   ^~表示普通字符匹配，如果该选项匹配，只匹配该选项，不匹配别的选项，一般用来匹配目录
@    "@" 定义一个命名的 location，使用在内部定向时，例如 error_page, try_files
~~~

#### 匹配顺序

1. = 前缀的指令严格匹配这个查询。如果找到，停止搜索
2. 普通匹配，按最长匹配。如果这个匹配使用 ^~ 前缀，搜索停止，否则继续正则匹配
3. 正则表达式匹配， ~* 和 ~按照在配置文件中定义的顺序
4. 如果第 3 条规则产生匹配的话，结果被使用。否则，使用第 2 条规则的结果

总结：正则 location 匹配让步普通 location 的严格精确匹配结果；但覆盖普通 location 的最大前缀匹配结果。

### root和alias

#### 区别

+ root处理结果：root路径+location路径
+ alias处理结果：alias路径替换location路径

#### 注意

+ alias是一个目录别名的定义，root是最上层目录的定义。如果location路径是以/结尾，alias也必须以/结尾，root没有要求。
+ 使用alias时，目录名后面一定要加"/"。alias只能位于location中，root可以不放在location中。

### try_files

#### 语法

~~~
格式1：try_files file ... uri;  
格式2：try_files file ... =code;
~~~

#### 注意

+ 按指定的file顺序查找存在的文件，并使用第一个找到的文件进行请求处理
+ 查找路径是按照给定的root或alias为根路径来查找的
+ 如果给出的file都没有匹配到，则重新请求最后一个参数给定的uri，就是新的location匹配
+ 如果是格式2，如果最后一个参数是 = 404 ，若给出的file都没有匹配到，则最后返回404的响应码

### proxy_pass

请求地址：/foo/api/

| location | proxy_pass           | 结果                         |
| -------- | -------------------- | ---------------------------- |
| /foo/    | http://192.168.1.48/ | http://192.168.1.48/api/     |
| /foo     | http://192.168.1.48/ | http://192.168.1.48//api/    |
| /foo/    | http://192.168.1.48  | http://192.168.1.48/foo/api/ |
| /foo     | http://192.168.1.48  | http://192.168.1.48/foo/api/ |

总结：配置proxy_pass时，当在后面的url加上了/，相当于是绝对根路径，则nginx不会把location中匹配的路径部分代理走;如果没有/，则会把匹配的路径部分也给代理走

### rewrite

#### flag参数

+ last 停止rewrite检测【如果没有匹配到，会继续向下匹配】、
+ break 停止rewrite检测【如果没有匹配到，则不再向下匹配，直接返回结果404】
+ redirect 返回302临时重定向，地址栏会显示跳转后的地址
+ permanent 返回301永久重定向，地址栏会显示跳转后的地址
