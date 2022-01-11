# PDB断点调试

## 用法

1. import pdb; pdb.set_trace()
2. python3.7之后支持：breakpoint()
3. 不用手动加入断点代码，只需在命令行输入运行指令时通过参数传递来设置：python -m pdb app.py arg1 arg2

## 常用命令

| 命令                                                     | 作用                                                         | 示例                     |
| -------------------------------------------------------- | ------------------------------------------------------------ | ------------------------ |
| `h`(elp) [command]                                       | 帮助文档                                                     | `h n`：查看n命令帮助文档 |
| `b`(reak) [([filename:]lineno \|function) [, condition]] | 设置非临时断点                                               |                          |
| `tbreak` [([filename:]lineno \|function) [, condition]]  | 设置临时断点                                                 |                          |
| `cl`(ear) [filename:lineno \|bpnumber ...]               | 清除断点，默认清除所有断点                                   |                          |
| `s`(tep)                                                 | 执行下一条命令，如果本句是函数调用，则会执行到函数的第一句   |                          |
| `n`(ext)                                                 | 执行下一条语句，如果本句是函数调用，则执行函数，接着执行当前执行语句的下一条。 |                          |
| `r`(eturn)                                               | 继续执行，知道当前函数返回                                   |                          |
| `c`(ontinue)                                             | 继续执行，遇到断点停止                                       |                          |
| `l`(ist) [first[, last]]                                 | 列出指定行数代码，默认11行                                   |                          |
| `ll` \| longlist                                         | 列出当前函数或框架所有代码                                   |                          |
| `a`(rgs)                                                 | 打印当前函数参数列表                                         |                          |
| `q`(uit)                                                 | 退出调试                                                     |                          |

## 参考

https://docs.python.org/3/library/pdb.html
