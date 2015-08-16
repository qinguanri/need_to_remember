#Shell命令

##awk
awk比较倾向于一行当中分成数个『栏位』（或者称为一个域，也就是一列）来处理。awk命令格式：

[root@www ~]# awk '条件类型1{动作1} 条件类型2{动作2} ...' filename
```
[root@www ~]# last -n 5| awk '{print $1 "\t lines: " NR "\t columns: " NF}'
root     lines: 1        columns: 10
root     lines: 2        columns: 10
root     lines: 3        columns: 10
dmtsai   lines: 4        columns: 10
root     lines: 5        columns: 9
```

注意喔，在 awk 内的 NR, NF 等变量要用大写。

##sed
##grep
##find
