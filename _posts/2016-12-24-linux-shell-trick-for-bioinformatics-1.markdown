---
layout: post
title: Shell tricks for bioinformatics 1
tag: shell trick bioinformatics
date: 2016-12-24 15:32:24.000000000 +09:00
---

1)  获得脚本的绝对路径

```bash
script_path=$(cd $(dirname $0); pwd)
cd $script_path
mkdir Folder
```

2)  统计目录大小, 不计算隐藏文件夹

```bash
du -sh * | grep -v '\.\/\.'
```

3)  找出仅在file2中出现的行(不在file1中出现)

```bash
grep -Fxv -f file1 file2
```

4)  监控文件的变化

```bash
watch "ls -al myfile"
```

5)  递归地查找当前目录下所有以"py"为后缀的文件，统计其行数

```bash 
find . -name ".*py " | xargs wc -l 
```

6)  每行一个显示PATH的内容

```bash 
sed 's/:/\n/g' <<< $PATH
```

7)  递归地删除空目录

```bash
find . -depth -type d -empty -exec rmdir -v {} \;
```

8)  复制所有文件. 所有的普通的，隐藏的和以减号开头的文件

```bash
cp ./* .[!.]* ..?* /path/to/dir 
```

9)  因为我们是想对第四列的某一项进行修改，所以先要确保除了第四列之外没有这样的值，避免错误修改 

```bash
cut -f1-3,5-57 -d "," test.csv |grep "mid_school"|le
```  
   
10)  用sed进行替换操作  

```bash
sed -e 's/mid_school/Mid_school/' test.csv
```

11) 如何取出文件里面不含有NA的行

```bash
perl -ne 'print unlesss /NA/' file
awk '{if(!/NA/){print $0}}' file
awk '$0 !~ /NA/ {print}' file 
grep -v "NA" file
sed -e '/*NA*/d' file
```

12) search some text from all files inside a directory

```bash
grep -Hrn "text" .
```

13) Automatically update all the installed python packages

```bash
for i in `pip list -o --format legacy|awk '{print $1}'`; do pip install --upgrade $i; done
```

14)  如何从CDS文件中提取gene_list:

```bash
grep "^>" sequence.fasta | tr -d ">" >geneID.txt 
```

15) 如何取出fastq的5'端的前50个碱基。

```bash
awk '{if(NR%4==2||NR%4==0) {print substr($0,1,50)} else {print $0} }' test.fq
```

16) 如何取出fastq的3'端的后50个碱基。

```bash
awk '{if(NR%4==2||NR%4==0) {print substr($0,length($0)-5 + 1)}  }' test.fq
```



为了方便使用电脑的读者，本文也同步发表在知乎专栏。有兴趣的可以移步知乎。

[知乎链接](https://zhuanlan.zhihu.com/pybio)：  https://zhuanlan.zhihu.com/pybio

