---
title: 用大O表示算法时间复杂度的歧义
category: tech
description: 本文介绍了用大O表示法讨论算法时间复杂度时，输入编码所导致的算法时间复杂度的争议。
# comments: false
keywords: algorithm, time complexity, encoding
---

大半年前就想写的这篇博客，当时还在V2ex上发了个[帖子](https://www.v2ex.com/t/330114)和人“撕逼”了一发。一直拖到现在才写，惭愧惭愧。

在本文撰写过程中，认知又有进一步拓展，果然教是最好的学。

## 例题

OK, 长话短说，看一下这个函数的时间复杂度是多少？

```C
void foo1(int x) {
    int bar = 0;
    for (int i = 0; i < x; i++) {
        bar++;
    }
}
```

显然，这个函数输入x，然后函数就循环x次，那就是`O(n)`咯。

此题的来源是考研408里的一道选择题，并做了简化：

```C
void foo2(int n) {
    int sum = 0;
    int i = 0;
    while (sum < n) {
        sum += i++;
    }
}
```

此题问时间复杂度是多少，显然，命题人希望的答案是`O(根号n)`。

**但事实上，真这么简单么？**

## 剧情反转

再来看这个函数：

```Java
void foo3(int[] arr) {
    int bar = 0;
    for (int i = 0; i < arr.length; i++) {
        bar++;
    }
}
```

对于`foo3`，应该没有争议，时间复杂度是`O(n)`。

但是显然，这两个函数输入的数据量完全不一样，并且实际在机器上运行所耗费的时间也完全不同：
`foo1`只要输入一个大的整数，运行的时间就会比输入上亿规模数组的`foo3`还慢。
它们真的都是`O(n)`的时间复杂度？显然`foo1`的时间复杂度应该远高于`foo3`啊。

看到这里，肯定开始觉得矛盾和疑惑了，再举个例子，让你更纠结。

素数判定问题是一个已知的NP-Hard问题，求解这个问题的一个直观算法是：

```C
bool isPrime(int n) {
    // n > 2
    for(int i = 2; i < n; i++){
        if(n % i == 0) {
            return false;
        }
    }
    return true;
}
```

如果`foo1`的时间复杂度是`O(n)`的话，`isPrime`的复杂度也毫无疑问应该是`O(n)`。

OK，一个NP-Hard问题就这么轻易地被多项式时间算法解决了，`P = NP`，图灵奖诞生了！

## 争议点

其实，引起上述悖论的根源，在于我们使用大O表示法来表示算法时间复杂度时经常忽略的那个参数“n”的定义。
在提到`O(n), O(log(n))`时，参数n的定义到底是什么？

认为`foo3`函数的时间复杂度是`O(n)`，这里的n指的是输入的长度。
而如果认为`foo1`的时间复杂度也是`O(n)`，那么这里的n其实指的是输入的值了。而此时输入的长度和值之间，存在`2^n`的关系。
如果强行让n表示为输入的数的二进制长度，那么说`foo1`函数的时间复杂度是`O(2^n)`也就不难理解了。

那么，大O表示法下，这个n到底表示什么意思，有一个确切的定义么？

其实是有的，参见维基百科 [Time complexity](https://en.wikipedia.org/wiki/Time_complexity)词条：

> In computer science, the time complexity of an algorithm quantifies the amount of time taken by an algorithm to run as a function of the length of the string representing the input.

所以，严格来说，n应该取输入的“长度”，而不是输入的“值”。

## 再次反转

不要怀疑了，大O表示法的n指的就是输入的长度。
在此前提下，你是否开始认同`foo1`的时间复杂度是`O(2^n)`了？

但是等等，为什么是一定是2的指数级？又没规定所有计算机都必须是二进制表示的。如果是十进制（事实上第一台计算机就是十进制）表示的呢？譬如17，在十进制下，“长度”是2，二进制下，“长度”就成了5。而我们讨论算法的时间复杂度时，谈论的是算法自身的性质，而不会依赖于具体实现，这个函数的时间复杂度为什么不能是`O(10^n)`？

### 根本原因

造成这个矛盾的根源在于，时间复杂度的定义是输入字符串的长度，但用什么“编码方式”来表示输入却没有指定。

编码不一定指的是字符串编码，7、七、柒、seven、Ⅶ等等都是对抽象数字“7”的一种编码。而算法的输入用不同方式编码表示的长度肯定不同。

极端一点，如果用一进制来编码，4就是`1111`，那么这种情况下`foo1`的时间复杂度还真就是`O(n)`了！

### 伪多项式复杂度

因为这个问题真是太容易引起歧义了，并且对于`foo1`函数，明显也是`O(n)`更符合人的直觉。
因此有了这么一个名词：[Pseudo-polynomial time（伪多项式时间复杂度）](https://en.wikipedia.org/wiki/Pseudo-polynomial_time)

> ... Since computational complexity measures difficulty with respect to the length of the (encoded) input, this naive algorithm is actually exponential. It is, however, pseudo-polynomial time.
...
The distinction between the value of a number and its length is one of encoding: if numeric inputs are always encoded in unary, then pseudo-polynomial would coincide with polynomial.

看！真相大白！

学过计算理论的同学应该在最开始就一眼看出了这是一个“伪多项式时间”的问题。如果真正理解这个词，这个问题也就不会让人产生困扰了。

## 结语

所以，最后，`foo1`这个“伪多项式时间”算法的复杂度用大O表示法到底是多少？

`O(n), O(2^n)`，都对又都不对，取决于对输入n的编码，如果采用不符合直觉的unary numeral system（一进制），那么得到的就是符合直觉的`O(n)`，而如果采用符合直觉的的二进制或十进制，那么时间复杂度就是指数级。

我们平时张口即来的算法时间复杂度其实是有歧义、不完备的，稍加思考就会发现前文介绍的那种矛盾。
所以，在这种情况下，最好还是尊重大部分的人的常识比较好。

<br>

本文看似出了一道无意义的题，最终也没有得到一个确切的答案。但在思考的过程中拓展认知的局限，才是真正有意义的事情。
