# cmd tips
记录linux系统的command的一些小操作

## General tips
1. `&` (backgroud operation): `python renne.py &`即将这个script放入后台运行，在linux中就是background job 
2. `!` (not): `ls !(*txt)`,列出所有不是txt的file
3. `|` (pipe)： `cmd1 | cmd2`, 即cmd1的输出不进入stdout，而是进入作为输入进入cmd2
4. `>` (output direct)： `cmd > file.txt`，cmd的输出不进stdout，而是被写到这个file.txt中。这里`>`即`1 >`，表示stdout to file，如果是error的话用`2 >`，both的话用`& >`。
5. `;` (simi-colon): 使得在同一行可以运行多个cmd
6. `&&` (and): `cmd1 && cmd2`，cmd1执行成功之后，会执行cmd2
7. `||` (or): `cmd || cmd2`，cmd1执行失败之后，会执行cmd2

#### grep (global regular expression point)
用于从output中抓出指定re的的内容，常用的有：`cat file.txt | grep Renne`

#### ldd
用于查看dependency，例如dll文件这种。

#### ps (process)
查看当前正在运行process
```
ps ax 
```
抓出指定的process, (例如iperf)
```
ps ax | grep iperf
```
