# sf4j-basic-grammer.md

## intro
slf4j是Java的一种Log Api，类似Apache Commons Logging 。

## usage method
```最直接的log方式
1. logger.debug("Entry number: " + i + " is " + String.valueOf(entry[i]));
这种用字符串拼接的构造方式在debug disabled的情况下，字符串消息还是会被求值，存在类型转换和字符串连接的性能消耗。
```

2. log前做条件判断
```
if(logger.isDebugEnabled()) {
logger.debug("Entry number: " + i + " is " + String.valueOf(entry[i]));
}

Note: isDebugEnabled()的方法在debug disabled的情况下不存在构造字符串参数的性能消耗，但是如果debug enabled，debug是否被enabled将会被求值两次：一次是isDebugEnabled(),一次是debug()本身(该影响较小，因为求值logger状态花费的时间比真正log一条语句花费的时间的1%都还要小)。

```

3. Recommend Practice 使用SLF4J的格式化功能

```
	Object entry = new SomeObject();
	logger.debug(“The entry is {}.”, entry);

	使用SLF4J的格式化功能，这种用法不存在上面提到的缺点。SLF4J使用自己的格式化语法{}，同时提供了适合不同参数个数的方法重载：

	logger.debug(String format, Object param); //支持一个参数

	logger.debug(String format, Object param1, Object param2); //支持两个参数

	logger.debug(String format, Object… param); //任意数量参数，构造参数数组具有一定的性能损耗

	连续的{}才被认为是格式化占位符，所以：

	logger.debug(“Set {1,2} differs from {}”, “3”); //output:Set {1,2} differs from 3

	logger.debug(“Set {1,2} differs from {{}}”, “3”); //output:Set {1,2} differs from {3}

	用”\”转义{}占位符

	logger.debug(“Set \{} differs from {}”, “3”); //output:Set {} differs from 3

	用“\”本身转义“{}”中的”\”

	logger.debug(“File name is C:\\{}.”, “file.zip”); //output:File name is C:\file.zip
```
