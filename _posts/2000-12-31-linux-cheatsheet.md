---
layout: post
title: Linux cheatsheet (持续更新)
<!-- date: 2020-05-30 12:25:06 -0700 -->
<!-- tags: linux cheatsheet -->
<!-- comments: true -->
categories: linux
---


* TOC
{:toc}

Ubuntu basics
==============

1\. 查看磁盘空间： `df -h`

2\. 查看当前文件夹中文件磁盘空间占用： `du -sh *`

3\. 查看ip： `ip addr`

4\. 显卡

```sh
# 查看显卡信息： 
lspci | grep -i vga
# 查看nvidia显卡型号： 
lspci | grep -i nvidia
# 查看驱动版本： 
sudo dpkg --list | grep nvidia-*
```

5\. Nvidia自带一个命令行工具可以查看显存的使用情况： `nvidia-smi`

6\. 查看目前所用的cuda版本： `cat /usr/local/cuda/version.txt`

7\. 下载文件到指定目录下： `wget -P 指定目录 下载链接`

8\. 查看cpu信息： `lscpu`

9\. 查看内核信息： `uname -a`

10\. 查看网络配置： `ifconfig `

11\. 解压缩bz2文件： `bunzip2 FileName.bz2`

12\. 查看文件编码格式： `file file_name`

13\. nohup后台运行程序： `nohup python myscript.py &> out.log &`， 查看python进程： `ps aux|grep python` ，停止进程： `kill num`， 强制停止： `kill -9 num`

14\. 查看ubuntu系统版本： `cat /proc/version`

15\. rsync 传输文件： `rsync -e "ssh -p 端口号" user_name@域名或ip:/目录/文件名 本地目录`

16\. 每（1）秒钟查看一次gpu使用情况： `watch -n 1 nvidia-smi`

17\. 在当前目录及子目录下查找文件： e.g. `find . -name \*.xlsx` （注：查找以xlsx结尾的文件，"\*"前需要加"\"转义）

Bash
==============

1\. 去除文件中的重复行： `cat data1.txt | sort | uniq > out.txt`

2\. 将测试目录test1下所有内容完全复制到test2： `cp -r test1/ test2`

3\. sed 打印文件中某行：

* 打印file中的第4行： `sed -n 4p filename`
* 打印file中的4-8行： `sed -n 4,8p filename`

4\. 指定分隔符（"\|"）选取第几列： 

```sh
awk -F "|" '{print $1"|"$2"|"$4"|"}' yourfile　> newfile
```

5\. 按指定列排序：

```sh
sort -t "," -k 1n,1 -k 3rn,3 file.txt
  1.-t 指定文本分隔符
  2.-k 指定排序列
  3.-n 按数字进行排序
  4.-r 翻转排序结果
#上面的例子为按第一列正排序，按第三列反排序；
e.g. sort -t "," -k 2n,2 test.csv > testout.csv
```
6\. sed查找替换： `sed -i s/\"//g testout.csv`

7\. awk统计文件第（二）列频次（逗号分割）： 

```sh
awk -F',' 'NR==FNR{a[$2]++}NR!=FNR&&++b[$2]==1{print $2,a[$2]}' data.csv data.csv
```

8\. join以第（一）个文件的第（二）列和第（二）个文件的第（三）列做匹配字段： 

```sh
join -t ',' -1 2 -2 3 file1.txt file2.txt
#（-t ','逗号为分隔符；默认为空白字符。）
```

9\. left join

```sh
# file1.txt left join file2.txt:
awk -F',' 'NR==FNR{a[$1]=$2;}NR!=FNR{print $0,a[$1]}'  OFS=',' file2.txt file1.txt> test.out
# 注：OFS为指定输出分隔符 
```

10\. 服务器和本地文件传输

```sh
# 从服务器下载到本地
rsync -e 'ssh -p 端口号' 用户名@serverXX.com:/目录/文件.txt .
```

11\. 用shell脚本一键自定义初始环境

```sh
# 例如：
# 写入env.sh文件如下内容：
# cd /home/dir
# source ~/.bashrc
# conda activate anevn
# 将env.sh变成执行文件
chmod +x env.sh
# 一键运行
source evn.sh
```

pip
==============

1\. 为指定版本python安装包： `python37 -m pip install xxx`

2\. 查看已安装的模块： `pip list`

3\. 升级某个Python包： `pip install --upgrade xxx`

Vi
==============

1\. 清空文件： `:%d`

2\. 删除空行： `:g/^$/d`

3\. 修改文件编码： e.g. 转换文件编码为utf-8 `:set fileencoding=utf-8`，查看文件编码 `:set fileencoding`

4\. 跳转到倒数第n行： `shift+g`到最后一行，`:-n`跳转到倒数第n行

5\. 查找某个关键词出现次数： `:%s/key_word//gn`

Conda virtual env
==============

1\. 查看已安装的虚拟环境： `conda env list`

2\. 查看已安装的模块： `conda list`，可通过linux管道"|"查询关键词, e.g. `conda list | grep tensorflow`

3\. 进入虚拟环境： `source activate env_name`

4\. conda create -n xxxx python=3.7   //创建python3.7的xxxx虚拟环境

5\. conda env remove -n xxx
//删除xxx环境
Docker
==============

1\. 查看docker信息： `docker inspect container_name`

2\. 删除docker： `docker rm -f 1e560fca3906`

3\. 查看已创建的docker： `docker ps -a`

4\. 进入docker： `docker exec -it docker_name bash`

5\. 查看docker镜像列表： `docker image ls`

Mysql
==============

1\. 清空表格： `TRUNCATE tbl;`

2\. 为表格添加列

```sql
-- 在jcol后添加一列icol
ALTER TABLE tbl_name ADD icol INT AFTER jcol;
-- 把icol添加到第一列
ALTER TABLE tbl_name ADD icol INT FIRST;
```

3\. 建表

```sql
-- e.g.
create table tbl_name(col1 char(6) not null, col2 char(20), col3 date);
```

4\. 远程登陆mysql

```shell
<!-- mysql -h 服务器ip -u 用户名 -p -P 端口号 -->
<!-- 注：-P 端口号可省略 -->
mysql -h 服务器ip -u root -p -P 端口号
<!-- 回车输入密码 -->
```

MongoDB
==============

1\. 模糊查询
 
```mongodb
"col_name" => array('$regex'=> 'keyword_1|keyword_2')
```

2\. 某个col值不为空
 
```mongodb
"col_name" => array('$ne' => NULL)
```

3\. 逻辑与（e.g.col_name不为'NULL'且不为0）, 逻辑或(and换成or)
 
```mongodb
'$and' => array(
    array("col_name" => array('$ne' => NULL)),
    array("col_name" => array('$ne' => 0)),
    )
```

4\. 某列是否存在
 
```mongodb
'col_name'=>array('$exists'=>true)
```

5\. 范围限制：
greater than (i.e. >, gt); 
greater than or equal to (i.e. >=, gte); 
less than (i.e. <, lt);
less than or equal to (i.e. <, lte);

```mongodb
'col_date' => array('$gt'=>'2019-01-01')
```


文本编辑器
==============

1\.sublime text3 折叠快捷键：
按ctrl + k，然后按ctrl + 1，可收起所有函数
按ctrl + k， 再按 ctrl + j 显示所有函数