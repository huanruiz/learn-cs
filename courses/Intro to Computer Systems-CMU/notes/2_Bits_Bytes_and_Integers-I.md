# 2_Bits_Bytes_and_Integers-II
首先要了解位运算的一些常识如与, 或, 非, 异或的运算, 他们的符号分别为`&, |, ~, ^`, 以及2进制, 16进制, 10进制的转换. 在C语言中, 所有的整型都适用位运算. 对于这些整型的表示, unsigned integer为$\sum^{w-1}_{i=0}x_{i}\cdot 2^{i}$. 而signed integer为Two’s Complement: $-x_{w-1}\cdot 2^{w-1} + \sum^{w-2}_{i=0}x_{i}\cdot 2^{i}$, $x_{i}$表示某一位. 在这里不要被公式迷惑了, 实际上, unsigned整型就是直接用10进制转换的2进制存储, 而signed整型用**2的补码存储**, 且首位为是否负号的标志位. 这种区分unsigned和signed的编码形式在其他语言不一定能见到. 比如Java中的int, 就默认是32位的signed编码. 

举个例子: 我们知道当unsigned与signed做比较时, signed会自动转为unsigned, 这会造成神奇的现象, 比如`-1`与`0U`比较的结果是大于, 因为`-1`的编码为11111, 而0U是00000(假设用5位的编码).

还有个更有趣的例子, 看下面的代码, 你能看出来有什么隐藏的问题吗? 代码中`sizeof(char)`返回的是unsigned的整型, 而`int i;`在C语言中默认是`signed int i;`, 在做运算时, `i`会被转换unsigned, 而`i`在转换后, 就很有可能让这个循环的终止条件产生出乎意料的现象. 
```
int i;
for (i = n - 1; i - sizeof(char) >= 0; i--) {
    ...
}
```

课上还介绍了**sign extension**. 用有符号的整型扩充一位作为例子. 我们会很轻松地发现, 第三位的`+8`和`-8`抵消了, 只要在最高位扩充`1`, 得到的数字实际是一样的.
```
 1110 =    -8+4+2 = -2
11110 = -16+8+4+2 = -2
```

反过来**truncate**一个数字会发生什么呢? 先看一个无符号的例子, 会发现其实是做了一个mod最高位的操作.
```
27 mod 16 = 11
11011 = 16+8+0+2+1 = 27
 1011 =    8+0+2+1 = 16
```

上面是unsigned的情况, 如果是signed会怎样呢. 如下是负数去掉最高位的例子, 我们会发现, 转化成10进制后这两个数字并没有什么关系. 所以只能把`10001`当成无符号的十进制数字19, 然后再mod 16, 最后得到结果3.
```
10001 = -16+2+1 = 13
 0011 =     2+1 = 3
```

## 参考
1. [Intro to Computer Systems: Schedule for Fall 2015](http://www.cs.cmu.edu/afs/cs/academic/class/15213-f15/www/schedule.html)
2. [lab](http://csapp.cs.cmu.edu/3e/labs.html)