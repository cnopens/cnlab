#ubuntu1804-install-workstation14.md

---
## ubuntu1804 install worksation14 因内核版本过高安装不成功问题？


sudo vmware-modconfig --console --install-all
---
错误：
[AppLoader] GLib does not have GSettings support.
Stopping VMware services:
   VMware Authentication Daemon                                        done
   VM communication interface socket family                            done
   Virtual machine communication interface                             done
   Virtual machine monitor                                             done
   Blocking file system                                                done
make: Entering directory '/tmp/modconfig-N85Ceo/vmmon-only'
Using kernel build system.
/usr/bin/make -C /lib/modules/5.4.0-47-generic/build/include/.. SUBDIRS=$PWD SRCROOT=$PWD/. \
  MODULEBUILDDIR= modules
make[1]: Entering directory '/usr/src/linux-headers-5.4.0-47-generic'
  LEX     scripts/kconfig/lexer.lex.c
  YACC    scripts/kconfig/parser.tab.[ch]
  HOSTCC  scripts/kconfig/preprocess.o
  HOSTCC  scripts/kconfig/symbol.o
  HOSTCC  scripts/kconfig/lexer.lex.o
  HOSTCC  scripts/kconfig/parser.tab.o
  HOSTLD  scripts/kconfig/conf
scripts/kconfig/conf  --syncconfig Kconfig
make[2]: *** No rule to make target 'arch/x86/tools/relocs_32.c', needed by 'arch/x86/tools/relocs_32.o'.  Stop.
arch/x86/Makefile:232: recipe for target 'archscripts' failed
make[1]: *** [archscripts] Error 2
make[1]: *** Waiting for unfinished jobs....
  UPD     include/config/kernel.release
make[1]: *** wait: No child processes.  Stop.
Makefile:110: recipe for target 'vmmon.ko' failed
make: *** [vmmon.ko] Error 2
make: Leaving directory '/tmp/modconfig-N85Ceo/vmmon-only'
make: Entering directory '/tmp/modconfig-N85Ceo/vmnet-only'
Using kernel build system.
/usr/bin/make -C /lib/modules/5.4.0-47-generic/build/include/.. SUBDIRS=$PWD SRCROOT=$PWD/. \
  MODULEBUILDDIR= modules
make[1]: Entering directory '/usr/src/linux-headers-5.4.0-47-generic'
make[2]: *** No rule to make target 'arch/x86/tools/relocs_32.c', needed by 'arch/x86/tools/relocs_32.o'.  Stop.
arch/x86/Makefile:232: recipe for target 'archscripts' failed
make[1]: *** [archscripts] Error 2
make[1]: *** Waiting for unfinished jobs....
make[1]: *** wait: No child processes.  Stop.
Makefile:110: recipe for target 'vmnet.ko' failed
make: *** [vmnet.ko] Error 2
make: Leaving directory '/tmp/modconfig-N85Ceo/vmnet-only'
Unable to install all modules.  See log for details.

---