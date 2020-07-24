#yaml-manual.md

## yaml文件的 锚点& 与 引用*

两种方式实现
1. 通过 << 符号
2. 通过 & 符号

### example 

	user:
		host: 127.0.0.1
		db: 2
		
	order:
		host: 127.0.0.1
		db: 3
		
	pay:
		host: 127.0.0.1
		db: 4

use <<

	localhost: &localhost1
		host: 127.0.0.1
	user:
		<<: *localhost1
		db: 2
	order:
		<<: *localhost1
		db: 3
	pay:
		<<: *localhost1
	db: 4

use & 

	localhost: 
		host: &host 127.0.0.1
	user:
		host: *host
		db: 8
		
	book:
		host: *host
		db: 9
		
	comment:
		host: *host
		db: 10
