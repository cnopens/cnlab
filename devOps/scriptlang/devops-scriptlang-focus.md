# devops-scriptlang-focus.md
---
## make learning

1. make introduce
make是一个指令工具，它解释makefile中的指令或者说规则。makefile文件描述了整个工程中所有文件的**编译顺序，编译规则**。Makefile也有自己的编写规则，通常，我们所使用的IDE都会生成相应的makefile，然后再根据makefile来进行编译，只是这些操作是由IDE来完成，我们只需要点击一个编译按钮。

2. why use it
现在可以在GitHub上看到，很多的开源项目，在编译的时候，都是使用make来完成的，也就是说，都有其对应的makefile。他们都有个特点，那就是文件很多。

考虑这样一种情况，我们的项目现在有三、四十个文件，你使用的不是IDE工具，而是命令行，那么不同的人，在编译你的项目的时候，都需要一个一个文件的

gcc -o asample.c bsample.c ...... xxx.out

，这样慢慢的一个文件，一个文件的去找到以后再编译吗？

答案肯定是否定的，当你工程的文件多了以后，时间一长，可能你自己都不能记住所有的文件。所以，这个时候我们就可以使用make来根据makefile对整个项目进行管理。除此之外，make还有一个优点，那就是当你修改你的文件以后，make只会编译更新的文件以及它相关依赖的文件。这里后边进行详细的解释，意思就是，当你只修改了几十个文件中的某一个文件时，make只会重新编译跟你修改的文件有关联的文件，而不是所有的文件。这就大大的减短了编译的时间。 

3. makefile说明
在我们执行make之前，需要有一个名为makefile或Makefile的文件。这个文件用来告诉make需要完成什么样的操作。我们可以简单的把makefile认为是一份定义了源文件间依赖关系、如何编译各个源文件并生成可执行文件的说明书。

4. makefile基本结构
makefile的基本结构

TARGET... : PREREQUISITES...

COMMAND

...

...

TARGET：规则的目标，最终生成文件的名字或者是中间过程文件名，也可以是make执行的动作的名称。

PREREQUISITES：规则的依赖，生成目标所必须的文件名列表。

COMMAND：规则的命令。规则需要执行的动作

注意：这里需要注意的是，命令前面使用的是TAB键，而不是空格，使用空格会出现错误。

五、第一个makefile

makefile文件内容如下

hello :

echo "hello makefile"

test :

echo "test"

pwd

ls