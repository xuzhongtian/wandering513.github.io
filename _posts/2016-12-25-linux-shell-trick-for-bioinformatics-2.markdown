---
layout: post
title: Shell tricks for bioinformatics 2
tag: shell trick bioinformatics
date: 2016-12-25 15:32:24.000000000 +09:00
---

上一次写了一个《Linux tricks for bioinformatics系列文章之一》，有读者从后台给我留言指出了一些欠缺的地方，非常感谢这些认真的读者。如果读者想熟悉这些命令，建议将命令手动操作一遍，从管道符的左边到右边依次输入，观察输出，细细体会每一个操作的作用。只有灵活运用这些招式，才能打出一套漂亮的组合拳。


1) 将fastq格式文件转化为fasta序列文件。在上个礼拜我提供了一种解法，但是后来有小伙伴在后台私信我，在使用的过程当中遇到了一些问题。因为fasta文件的质量行，也有可能是以@开头。当碰到这样的情况的时候，下面的方法就会带来问题。在深表歉意的同时，贡献了另外一种解法。

原先的解法：

```bash 
sed '/^@/!d;s//>/;N' your.fastq> your.fasta 
```


新解法：


{% highlight bash %}
awk '{if(NR%4==1||NR%4==2){print $0}}'  test.fq | sed 's/^@/>/g' |less
{% endhighlight %}


2)  有时候，比如说我们需要做一个clustalw，我们需要将多行的fasta文件(multiple line fasta)格式文件，转换成单行的fasta文件(single line fasta)序列文件，怎么办呢。 巧用awk + printf一行搞定。


{% highlight python %}
awk '/^>/ { print n $0;}  !/^>/ {printf "%s", $0, n="\n"}  END {print ""}'  test.fa
{% endhighlight %}




3. 在做GO注释的时候，我们一定会用到go2geneid.txt这个文件。以拟南芥为例，它的go2geneid.txt文件如下。

go2geneID.txt文件中，GO号和gene之间是tab分开，gene之间是以逗号分开的。


现在我们很好奇那个GO号拥有的基因数目最多，写脚本吗？NO，shell一行搞定。


```
```



4. 如何统计文件的行数。大家心想，还用说吗？ `wc -l`不就好了。但是当这个问题交给awk时，awk的解法是。 


```
awk 'END {print NR}' file.txt 
```


这种情况下，虽然awk可以胜任，但是不建议使用awk了 ，因为已经有更简单的解法。简洁胜于复杂。



5. 当我们想要提取文件的某列时。


```
cut -f 7 file.txt > out.txt
```



当然交给万能的awk，则会这么写。


```
```


这种时候就要读者自己去权衡了，因为自己习惯并欣赏awk，所以个人反而更喜欢后一种写法。



6.  将GFF里面第三列不是chromosome的行给取出来。


```
```



7.  删除文件里所有的空行


```
sed 's/^$/d' file
```


8. 有时候我们不想看到header，直接用文件本身的内容去进行下一步操作，我们需要将header去掉。


```
sed '1d'  file.txt
```


这个题目当然也可以用awk去实现，用awk的解法是...

都看了两篇系列文章了，自己去试试呗~~



9.  有时候我们可能会需要使用awk为每一行行头添加字符串。


```
awk '{print "gene_ID " $0}' file.txt
```


10. 统计fastq文件里面的reads数目。


```
grep  -c "^@"  test.fastq
```


本来准备把这种解法也算一个，但是这种方法依然有一个问题，那就是质量行是有可能是以@开头的，这样做就会导致有点点误差，虽然也不会差太大。



因此应该是用下面的方法。


```
cat test.fastq | echo $((`wc -l`/4)) 
```

同理，也可以很轻松能够地写出统计fasta文件函数的shell命令，就是改成除以2呗。



为了方便使用电脑的读者，本文也同步发表在知乎专栏。有兴趣的可以移步知乎

知乎链接：  https://zhuanlan.zhihu.com/pybio
