#Libvirt-architectrue.md

## Libvirt What'it?

The libvirt project:

is a toolkit to manage virtualization platforms
is accessible from C, Python, Perl, Java and more
is licensed under open source licenses
supports KVM, QEMU, Xen, Virtuozzo, VMWare ESX, LXC, BHyve and more
targets Linux, FreeBSD, Windows and macOS
is used by many applications
Recent / forthcoming release changes


Libvirt 库是一种实现 Linux 虚拟化功能的 Linux® API，它支持各种虚拟机监控程序，包括 Xen 和 KVM，以及 QEMU 和用于其他操作系统的一些虚拟产品。

why&&what
实现一朵可运行、可运维的云，需要完整的实现三层：VIM层、VNFM层、NFVO层，其中实现对VNF的生命周期管理是VNFM层要实现核心功能。但要做到对VNF的控制管理谈何容易，VIM层中提供的hypervisor技术多种多样，包括kvm,vmware,xen等等，每种技术提供的驱动和API又都不尽相同。即使能够做到单种技术的控制管理，还要实现不同hypervisor下VNF的迁移，困难可想而知。
而Libvirt正是为解决上述问题而生，通过在VIM层和VNFM层提供一个虚拟抽象层，提供统一API供上层调用，下层统一封装不同虚拟机，从而方便地实现对虚拟机的管理。
Libvirt是管理虚拟机、存储、网络的一系列软件集合。它包括了一个API库、一个daemon程序（libvirtd）和一个命令行工具(virsh).主要目标是为各种虚拟化工具提供一套统一可靠的API，让上层可以用一种单一的方式来管理多种不同的虚拟化技术。

1、什么是libvirt
Libvirt是管理虚拟机和其他虚拟化功能，比如存储管理，网络管理的软件集合。位于虚拟机和云管理中间的一个抽象管理层。它包括一个API库，一个守护程序（libvirtd）和一个命令行工具（virsh）；libvirt本身构建于一种抽象的概念之上。它为受支持的虚拟机监控程序实现的常用功能提供通用的API。
libvirt的主要目标是为各种虚拟化工具提供一套方便、可靠的编程接口，用一种单一的方式管理多种不同的虚拟化提供方式。

libvirt 的基本操作和大概结构

libvirt 组件有一个 shell，被称为 virsh，提供类似 shell 的界面，可以输入 start、shutdown
等命令操作虚拟机

libvirt 有一个守护进程，libvirtd，其对 virsh 的命令做出响应

以 non-root 执行 virsh start 时，将以 qemu://session 的方式运行。libvirtd 将启动一个 non-root 的子进程来与 virsh 进行 socket 通信
以 root 执行 virsh start 时，将以 qemu://system 方式运行，libvirtd 直接与 virsh 进行 socket 通信
无论是上述哪种方式，都会创建多个（一般16个）线程，该线程的的作用是将 socket 传递过来的各个命令和配置进行解析，最终形成一个 cmd。
子线程会将 cmd 通过 pipe 传递给 libvirtd，libvirtd 会 fork 出一个子进程，并 exec cmd
来自https://blog.csdn.net/sdulibh/article/details/90257557

## Technology- architecture
libvirt是目前使用最为广泛的对KVM虚拟机进行管理的工具和API。Libvirtd是一个daemon进程，可以被本地的virsh调用，也可以被远程的virsh调用，Libvirtd调用qemu-kvm操作虚拟机。


![libvirt架构](/home/cnopens/workspace/cnlab/2020lab/libvirt/technology-architectrue.png)


- Refferences 
https://blog.csdn.net/weixin_42752248/article/details/107299491
