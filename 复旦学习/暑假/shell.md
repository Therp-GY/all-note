# shell

- 变量
  变量定义中间不能有空格
  使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

- 传递参数
  我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……
  $# 	传递到脚本的参数个数
  $* 	以一个单字符串显示所有向脚本传递的参数。
  如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
  $$ 	脚本运行的当前进程ID号
  $! 	后台运行的最后一个进程的ID号
  $@ 	与$*相同，但是使用时加引号，并在引号中返回每个参数。
  如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
  $- 	显示Shell使用的当前选项，与set命令功能相同。
  $? 	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

- 数组
  定义：array_name=(value1 value2 ... valuen)
  读取：${array_name[index]}

- 运算符
  注意：表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程      语言不一样。
  完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。
  例如：
  val=`expr 2 + 2`
  echo "两数之和为 : $val"

  - 关系运算：
    -eq 	检测两个数是否相等，相等返回 true。 	[ $a -eq $b ] 返回 false。
    -ne 	检测两个数是否不相等，不相等返回 true。 	[ $a -ne $b ] 返回 true。
    -gt 	检测左边的数是否大于右边的，如果是，则返回 true。 	[ $a -gt $b ] 返回 false。
    -lt 	检测左边的数是否小于右边的，如果是，则返回 true。 	[ $a -lt $b ] 返回 true。
    -ge 	检测左边的数是否大于等于右边的，如果是，则返回 true。 	[ $a -ge $b ] 返回 false。
    -le 	检测左边的数是否小于等于右边的，如果是，则返回 true。 	[ $a -le $b ] 返回 true。

  - 布尔运算：
    ! 	非运算，表达式为 true 则返回 false，否则返回 true。 	[ ! false ] 返回 true。
    -o 	或运算，有一个表达式为 true 则返回 true。 	[ $a -lt 20 -o $b -gt 100 ] 返回 true。
    -a 	与运算，两个表达式都为 true 才返回 true。 	[ $a -lt 20 -a $b -gt 100 ] 返回 false。

  - |：
    管道符号，是unix一个很强大的功能,符号为一条竖线:"|"。
    用法:
    command 1 | command 2
    他的功能是把第一个命令command 1执行的结果作为command2的输入传给command 2

- 流程控制
  if：
  if condition
  then
      command1 
      command2
      ...
      commandN 
  fi   
  fi为if反过来   必须有

  

  for：
  for var in item1 item2 ... itemN
  do
      command1
      command2
      ...
      commandN
  done

- 函数
  在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数... 
  函数在定义时不设参数

- 重定向
  大多数 UNIX 系统命令从你的终端接受输入并将所产生的输出发送回到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端。

  重定向命令列表如下：
  命令 	说明
  command > file 	将输出重定向到 file。
  command < file 	将输入重定向到 file。
  command >> file 	将输出以追加的方式重定向到 file。
  n > file 	将文件描述符为 n 的文件重定向到 file。
  n >> file 	将文件描述符为 n 的文件以追加的方式重定向到 file。
  n >& m 	将输出文件 m 和 n 合并。
  n <& m 	将输入文件 m 和 n 合并。
  << tag 	将开始标记 tag 和结束标记 tag 之间的内容作为输入。

  如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：

  $ command > /dev/null

  /dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。

# linux命令

- Linux ps （英文全拼：process status）命令用于显示当前进程的状态，类似于 windows 的任务管理器。
  ps [options] [--help]      -aux 显示所有包含其他使用者的行程

- nohup 不挂起运行