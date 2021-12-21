## 写一个脚本，实现从一个存有多个名字的文件里随机抽取一个名字，且抽取不重样
输入：
```shell
./choujiang.sh namelist.txt
```
namelist.txt是存放抽奖名单的文件

输出
![[Pasted image 20211219194656.png]]

choujiang.sh脚本文件，先判断当前目录里是否已经有luckylist.txt文件_（用来存放已经中过奖的名单）_ ，加一个死循环，当namelist.txt里的名字全部被抽了一遍后跳出。
```shell
	(当前目录里是否已经有已中奖名单文件) ? 创建luckylist.txt : 不操作
	while true
	do
		获取抽奖名单人数和已中奖人数
		if ( 抽奖名单人数== 已中奖人数)
			break
		else
			从抽奖名单中随机抽取一个名字
			if (抽到的名字是否在luckylist.txt已存在)
				then
					显示名字
					将抽到的名字加到luckylist.txt文件中
			else
				continue
	done

```

```shell
#!/bin/bash

[ -f luckylist.txt ] || touch luckylist.txt #定义一个中奖的文件，保存中奖人的信息

while true
do
	num_total=$(cat $1|wc -l)   #统计文件里的总人数
	num_lucky=$(cat luckylist.txt|wc -l) #统计中奖的人数
	if (( $num_total == $num_lucky ));then
		>luckylist.txt
		echo "全抽完了"
		break
	else
		random_num=$(($RANDOM %num_total + 1))
		if [ -z $(cat luckylist.txt |grep ^$random_num$) ]
		then
			echo "$random_num" >>luckylist.txt
			echo "恭喜$(cat $1|head -$random_num|tail -1)中奖！"
			#break
			read -p "请按任意键继续"
		else
			continue
		fi
	fi
done
```

namelist.txt
```
lzf
liqingchon
luoshaowei
huangzian
qiantao
```


## 总结
### 文件判断语句
- [ -f file ]      file 判断文件是否存在且是否为文件
- [ -w file ]      write 判断文件是否存在且是否为可写文件
- [ -x file ]     execute 判断文件是否存在且是否为可执行文件
- [ -r file ]   read 判断文件是否存在且是否为可读文件
- [ -d file ]      directory 判断文件是否存在且是否为目录文件
- [ -s file ] 	判断文件是否存在且是否为空文件
- [ -e file ]  exist 判断文件是否存在 
注意：括号两边有空格

### && 和 || 的用法
- 连接两个条件
- 连接两个命令时
>- && 如果前面的命令执行错误($?不为零)则后面的命令不执行
>- || 如果前面的命令执行成功($?不为零)则后面的命令不执行


### 位置变量
- 接受参数$1,$2...
- $0返回脚本名
- $#参数的个数
- \$@,\$*列出所有参数
- \$\$表示脚本进程号


### 重定向
>\>符号
	>>如果文件存在清空文件里的内容，如果不存在新建一个新文件
	>>重定向内容到文件，其实就是保存内容到文件
	>>echo jiarudeneirong > temp.txt会覆盖原来文件中的内容
	>>1> 正确的重定向，默认，命令执行成功输出的内容
	>>2> 错误的重定向  命令执行出错输出的内容
	>>&> 正确和错误的重定向
	
>\>\>符号
	>>如果文件不存在，新建文件
	>>追加内容重定向，不会覆盖文件内容，在末尾追加
	>>echo jiarudeneirong >>temp.txt

