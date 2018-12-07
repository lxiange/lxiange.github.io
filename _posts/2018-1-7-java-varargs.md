---
published: true
title: Java变长参数的限制
category: tech
description: Java的可变长参数一些奇怪的现象以及使用的限制
keywords: 'Java, programming languages'
---
Java从Java 5开始，引入了可变长参数(varargs)机制，这个机制不是所有语言都有，并且各个语言实现可变长参数的方式也都不尽相同。
Java5是Java语言历史上改动最大的一代，引入了泛型机制。
我们知道Java的泛型是“伪泛型”，只是“编译器小说(compiler fiction)”，因为只是Java语言层的改进，而JVM层没有做改变。因此Java的泛型便存在了若干限制（譬如不支持泛型数组）。

同样，Java的可变长参数也存在若干限制。并且Java支持继承和函数重载，因此在选择调用哪个函数时，变会存在更多规则，以及一些不够优雅的现象。

### 本质是语法糖

和泛型一样，可变长参数只是编译器提供的语法糖，是通过数组来实现的。
在JVM层并没有做任何改动，只是编译器前端将可变参数翻译成了数组。

因此这两行代码，编译生成的字节码是完全一样的：

```Java
    test.foo("1", "2");
    test.foo(new String[] {"1", "2"});
    /*
      21: aload_1
      22: iconst_2
      23: anewarray     #21                 // class java/lang/String
      26: dup
      27: iconst_0
      28: ldc           #22                 // String 1
      30: aastore
      31: dup
      32: iconst_1
      33: ldc           #23                 // String 2
      35: aastore
      36: invokevirtual #24                 // Method foo:([Ljava/lang/String;)V
    */
```

### 类型不安全

正如前面所说，可变长参数本质是转换为数组对象进行传递。而Java不支持泛型数组[why?](https://www.zhihu.com/question/20928981)，因此对于引用类型而言，可变长参数是类型不安全的，需要加上`SafeVarargs`注解，以绕过编译器检查。

以下代码摘自Java标准库 Collections.java

```Java
    @SafeVarargs
    public static <T> boolean addAll(Collection<? super T> c, T... elements) {
        boolean result = false;
        for (T element : elements)
            result |= c.add(element);
        return result;
    }
```

这样可变参数数组，实质上就是一个泛型数组。

譬如这样的代码，是完全合法的：

```Java
    public void foo(List<Integer>... arg) {
        System.out.println("hello");
    }
    List<Integer> list = new ArrayList<>();
    List list2 = list;
    list2.add("boom");
    test.foo(list2);
```

因此泛型数组的类型安全问题，在可变参数上都得到了体现。

### 一个有意思的现象

之前说了，可变长参数本质是语法糖。可以用数组来替代。
可以将数组传给可变参数，但不能将可变参数传给数组参数。
后者比较容易理解，为了保证原有代码的语义。

但是我觉得“可以将数组传给可变参数”则不是一个很好的想法，尤其是当数组作为参数时，会产生歧义，导致各种稀奇古怪的问题，而这些歧义只能通过编译器的“约定”来解决。
虽然正常人是不会写这种代码的，但是这种不完备性依然会让人觉得很不舒服。

对于如下代码：

```Java
    public void foo(Object... arg) {
        System.out.println(arg[0].getClass());
    }
    test.foo(1);
    test.foo(1, 2);
    /* output:
     class java.lang.Integer
     class java.lang.Integer
     */
```

传入一个参数或者传入两个参数，最后结果是一样的，这点毫无疑问。

但是另一种情况下：

```Java
    test.foo(new Object[] {1, 2, 3});
    test.foo(new Object[] {1, 2, 3}, new Object[] {4, 5, 6});
    /* output:
     class java.lang.Integer
     class [Ljava.lang.Object;
    */
```

如果只传一个数组，那么Java会把数组当成可变参数来看，而如果传入两个数组，则会把数组本身当成一个可变参数数组的一个元素。
这种异常的表现，如果不清楚可变参数底层原理的话，就难以理解了。

当然，不能只怪可变参数，因为`Object`是所有类的公共父类，如果上述例子改成`Integer`就不会有这个问题了。

### 一些其他问题

在函数重载时，如果有多个函数可以调用，具体调用哪个函数的优先级是一个问题。一旦加入可变参数后，这个问题就更加烧脑。

譬如这种情况，到底会调用哪一个呢？

```Java
    public void foo(String s, String... arg) {
        System.out.println("String s, String... arg");
    }

    public void foo(String s, Object arg) {
        System.out.println("String s, Object arg");
    }
    test.foo("1", "2");
    /* output:
     String s, Object arg
    */
```

好在大部分人并不会写这样的代码，并且现在的IDE都可以很清楚地指出实际调用的函数。

### 总结

Java由于历史原因，后加入了很多特性，譬如泛型、可变参数等等，不可避免地带来了一下不够优雅的地方，而Java为了保证兼容性而做出的一些妥协，我觉得还是值得的。并且那些可以由IDE很容易就检测出来的错误，就没必要像孔乙己那样去钻牛角尖了。

Java里还有很多稀奇古怪的问题，推荐阅读[《Java解惑》](https://book.douban.com/subject/5362860/)