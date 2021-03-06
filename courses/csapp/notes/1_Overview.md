# 1_Overview
第一节课几乎没有讲任何的知识, 只是单纯地介绍了这节课, 老师是Bryant和O’Hallaron, CSAPP原书的作者. 这门课的前置要求是会C语言, 教材有两本, 一是**Computer Systems: A Programmer’s Perspective, Third Edition (CS:APP3e), Pearson, 2016**, 二是讲C语言的书**The C Programming Language, Second Edition, Prence Hall, 1988**. 目前网上找得到的版本是FALL 2015的课, 各个视频平台都有, CMU也把这门课放在了[panopto](https://scs.hosted.panopto.com/Panopto/Pages/Sessions/List.aspx#folderID=%22b96d90ae-9871-4fae-91e2-b1627b43e25e%22), 可以体验上网课的感觉. 这门课的课件都在*参考1*的连接, lab在*参考2*的链接.

这门课的中心是一句话: **Abstraction Is Good But Don’t Forget Reality**, 当我们用软件去抽象现实中的东西时, 不能不考虑现实中的限制. 课上举了几个例子.
1. Ints are not Integers, Floats are not Reals. 课上用lldb举了个例子, 我们会发现50000的平方计算出来了一个神奇的数字, 乘法结合律也错了. 而这些结果并不是随机的数字.
```
(lldb) print 40000 * 40000
(int) $0 = 1600000000
(lldb) print 50000 * 50000
(int) $1 = -1794967296
(lldb)

(lldb) print (1e20 + -1e20) + 3.14
(double) $2 = 3.1400000000000001
(lldb) print 1e20 + (-1e20 + 3.14)
(double) $3 = 0
```

2. You’ve Got to Know Assembly. 你可以不写, 但是不能不懂.
3. Memory Matters. 当入参是0, 1, 2, 3直到6的时候, 发现输出改变了, 原因是不应该访问的内存被访问了. (我没复现)
```
typedef struct {
  int a[2];
  double d;
} struct_t;

double fun(int i) {
  volatile struct_t s;
  s.d = 3.14;
  s.a[i] = 1073741824; /* Possibly out of bounds */
  return s.d;
}
```

4. There’s more to performance than asymptotic complexity. 换一下两个for的顺序, 运行的速度就会慢很多.
```
void copyij(int src[2048][2048],
            int dst[2048][2048]) {
  int i,j;
  for (i = 0; i < 2048; i++)
    for (j = 0; j < 2048; j++)
      dst[i][j] = src[i][j];
}
```
5. Computers do more than execute programs. 比如网络操作, 各种i/o操作. 

这门课有7个lab都很有趣, 可以等做到了那个地方再看. 

## 参考
1. [Intro to Computer Systems: Schedule for Fall 2015](http://www.cs.cmu.edu/afs/cs/academic/class/15213-f15/www/schedule.html)
2. [lab](http://csapp.cs.cmu.edu/3e/labs.html)