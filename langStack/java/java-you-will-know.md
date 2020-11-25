# java-you-will-know.md

## final modifier advantage

```
final关键字提高了性能。JVM和Java应用都会缓存final变量。
final变量可以安全的在多线程环境下进行共享，而不需要额外的同步开销。
使用final关键字，JVM会对方法、变量及类进行优化。
不可变类创建不可变类要使用final关键字。不可变类是指它的对象一旦被创建了就不能被更改了。String是不可变类的代表。不可变类有很多好处，譬如它们的对象是只读的，可以
在多线程环境下安全的共享，不用额外的同步开销等等。

```

