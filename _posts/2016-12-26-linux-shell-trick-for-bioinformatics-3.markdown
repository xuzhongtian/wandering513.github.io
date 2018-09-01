---
layout: post
title: Shell tricks for bioinformatics 3
tag: shell trick bioinformatics
date: 2016-12-26 15:32:24.000000000 +09:00
---

1) 每行一个显示PATH的内容

```bash
sed 's/:/\n/g' <<< $PATH
```

2) 监控文件的变化，特别是当大文件比对，然后你想看看output到底有没有变化时。

```bash 
watch ls -al test.sam
```

3) 如何取出一个字符穿的前50个字符

```bash 
cat test.fa | grep -v  ">" | cut -c 1-10
cat test.fa | grep -v ">" | sed -r 's/(.{50}).*/\1/g'
当然还可以用awk，但此处就不写啊。

```

4) 昨天在生信菜鸟群里，有一个人问，她想把一个Paired end序列的5'端前50个base,　和3'端的后50个base取出来做mapping, 想在bowtie2里面找参数，据我所知是没有这样的参数的。不过用shell也就一行的事。
下面是取出5'端前50个base的命令：

```bash
awk '{if(NR%4==2||NR%4==0) {print substr($0,1,50)} else {print $0} }' test.fq
```

需要注意的是由于第二行的序列和第四行的质量值是一一对应的，所以如果序列只取前50个，表示质量值的那一行也只需要取前50个。


5) 同理取出3'端的后50个base， 然后将结果依然保存为fastq的方法是：

```bash 
awk '{if(NR%4==2||NR%4==0) {print substr($0,length($0)-5 + 1)}  }' test.fq
```

6) 假如说，我们现在有一个sRNAs的数据，拿到之后呢第一步就是去接头。取几行序列出来，用clustwl之类的工具，就能很轻易地找到sRNAs的adapter序列。

```bash
clustal sRNAs.fa
```
7) 去掉接头序列后，就应该去掉含有测序的ambiguous碱基，通常用N表示。也就是说，如果序列中含有N碱基，这条reads就应该被抛弃。

```bash
sed 'N;s/\n/\t/' sRNAs.fa | sed -e '/\t.*N/d' | tr "\t" "\n"
```

+ 稍微地解释一下这段代码，fasta是以两行为单位，一行header，一行sequence, 所以我们先用sed将两行变一行，换行符变为\t分隔符; 然后将序列中含有N的reads给删掉,注意此时header中有N字符是可以容许的，然后末了再用tr将\t分隔符又又换换行符输出。

7) 去掉“N"这种ambiguous碱基之后，我们有必要做长度筛选，因为植物里面的sRNAs一般是18nt-30nt，而去掉adator之后，可能有些reads特别短了，还有些很长，可能是来自于structural RNAs的降解产物，所以应该去除这些过长或者过短的，只保留18-30的reads进行分析。

```bash 
sed 'N;s/\n/\t/' test.fa | awk 'BEGIN{OFS="\t";} {if (length($2)>=18&&length($2)<=30) {print $1,"\n",$2}}' | tr -d "\t"
```

8)  对于去掉接头，去掉“N”碱基，去掉过短或者过长的sRNAs序列之后，我们下一步就该看看sRNAs有那些特征了，比如一个比较重要的特征就是5'端的碱基频率，因为不同的5’端碱基，决定了sRNAs可能与那种AGO蛋白相结合，然后发挥生理作用。

```bash 
cat test.fa | grep -v ">" |cut -c 1-1 | sort| uniq -c| awk '{print $2,$1}' | tr " " "\t"
```

9) 对于去完接头的sRNAs序列文件，虽然还没有进行去掉结构RNA等步骤，但这时候的你，已经迫不及待地看看sRNAs的长度分布了。怎么办呢。

```bash 
cat sRNAs.fa | awk 'NR%2==0' | awk '{print length($0)}'|sort |uniq -c|sed 's/^[][ ]*//g'|awk '{print $2,$1}'
```


10)  把一个文本倒过来输出，即最后一行变成第一行，倒数第二行变成第二行。

```bash
seq 9 | awk '{ lifo[NR]=$0 }END{ for(lno=NR;lno > 0;lno--){print lifo[lno];}}'
seq 9 | tac 
```


为了方便使用电脑的读者，本文也同步发表在知乎专栏。有兴趣的可以移步知乎。

[知乎链接](https://zhuanlan.zhihu.com/pybio)：  https://zhuanlan.zhihu.com/pybio


