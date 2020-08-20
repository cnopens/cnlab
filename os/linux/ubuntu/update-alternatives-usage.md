#Ubuntu18.04-update-alternatives-usage.md

基本语法
update-alternatives [<选项> ...] <命令>

其中选项可以是以下内容：
选项 	描述
--altdir <目录> 	改变候选项目录。
--admindir <目录> 	设置 statoverride 文件的目录。
--log <文件> 	改变日志文件。
--force 	就算没有通过自检，也强制执行操作。
--skip-auto 	在自动模式中跳过设置正确候选项的提示(只与 --config 有关)
--verbose 	启用详细输出。
--quiet 	安静模式，输出尽可能少的信息。不显示输出信息。
具体命令

下面的命令假设没有增加选项。

1、在系统中加入一组候选项。
update-alternatives --install <链接> <名称> <路径> <优先级>
                    [--slave <链接> <名称> <路径>] ...

2、替换组中去除 <路径> 项。
update-alternatives --remove <名称> <路径>   从 <名称>

3、从替换系统中删除 <名称> 替换组。
update-alternatives --remove-all <名称>

4、将 <名称> 的主链接切换到自动模式。
update-alternatives --auto <名称>

5、显示关于 <名称> 替换组的信息。
update-alternatives --display <名称>

6、机器可读版的 --display <名称>。
update-alternatives --query <名称>

7、列出 <名称> 替换组中所有的可用候选项。
update-alternatives --list <名称>

8、列出主要候选项名称以及它们的状态。
update-alternatives --get-selections

9、从标准输入中读入候选项的状态。
update-alternatives --set-selections

10、列出 <名称> 替换组中的可选项，并就使用其中哪一个，征询用户的意见。
update-alternatives --config <名称>

11、 将 <路径> 设置为 <名称> 的候选项。
update-alternatives --set <名称> <路径>

12、对所有可选项一一调用 --config 命令。
update-alternatives --all

其中链接、名称、路径、优先级的含义如下：

<链接> 是指向 /etc/alternatives/<名称> 的符号链接。(如 /usr/bin/pager)
<名称> 是该链接替换组的主控名。(如 pager)
<路径> 是候选项目标文件的位置。(如 /usr/bin/less)
<优先级> 是一个整数，在自动模式下，这个数字越高的选项，其优先级也就越高。

