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

13\. nohup后台运行程序： `nohup python myscript.py &> out.log &`， 查看python进程： `ps aux|grep python` ，停止进程： `kill -num`

14\. 查看ubuntu系统版本： `cat /proc/version`

15\. rsync 传输文件： `rsync -e "ssh -p 端口号" user_name@域名或ip:/目录/文件名 .`

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

Conda virtual env
==============

1\. 查看已安装的虚拟环境： `conda env list`

2\. 查看已安装的模块： `conda list`

3\. 进入虚拟环境： `source activate env_name`

Docker
==============

1\. 查看docker信息： `docker inspect container_name`

2\. 删除docker： `docker rm -f 1e560fca3906`

3\. 查看已创建的docker： `docker ps -a`

4\. 进入docker： `docker exec -it docker_name bash`

5\. 查看docker镜像列表： `docker image ls`