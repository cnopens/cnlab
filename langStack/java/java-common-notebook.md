#java-common-notebook.md

---

## Java entity config auto generate Id method 




## jpa knowleguese
---
	springboot jpa 使用uuid生成id:

		直接上代码记录备忘 ：
		@Entity
		@Table(name="sys_f")
		@GenericGenerator(name = "jpa-uuid", strategy = "uuid")
		@NamedQuery(name="SysF.findAll", query="SELECT s FROM SysF s")
		public class SysF implements Serializable {
		    private static final long serialVersionUID = 1L;
		    @Id
		    @GeneratedValue(generator = "jpa-uuid")
		    @Column(name="F_ID")
		    private String fId;
		}

	@Id
	@GenericGenerator(name = "user_id", strategy = "uuid")
	@GeneratedValue(generator = "user_id")
	@Column(name = "user_id", unique = true, nullable = false, length = 32)

### JPA注解主键生成策略-UUID
	@GeneratedValue:主键的产生策略，通过strategy属性指定。
	主键产生策略通过GenerationType来指定。GenerationType是一个枚举，它定义了主键产生策略的类型。

	1、AUTO　自动选择一个最适合底层数据库的主键生成策略。如MySQL会自动对应auto increment。这个是默认选项，即如果只写@GeneratedValue，等价于@GeneratedValue(strategy=GenerationType.AUTO)。

	2、IDENTITY　表自增长字段，Oracle不支持这种方式。

	3、SEQUENCE　通过序列产生主键，MySQL不支持这种方式。

	4、TABLE　通过表产生主键，框架借由表模拟序列产生主键，使用该策略可以使应用更易于数据库移植。不同的JPA实现商生成的表名是不同的，如 OpenJPA生成openjpa_sequence_table表，Hibernate生成一个hibernate_sequences表，而TopLink则生成sequence表。这些表都具有一个序列名和对应值两个字段，如SEQ_NAME和SEQ_COUNT。

	在我们的应用中，一般选用@GeneratedValue(strategy=GenerationType.AUTO)这种方式，自动选择主键生成策略，以适应不同的数据库移植。
	如果使用Hibernate对JPA的实现，可以使用Hibernate对主键生成策略的扩展，通过Hibernate的@GenericGenerator实现。

　@GenericGenerator(name = “system-uuid”, strategy = “uuid”)　声明一个策略通用生成器，name为”system-uuid”,策略strategy为”uuid”。

　@GeneratedValue(generator = “system-uuid”)　用generator属性指定要使用的策略生成器。

　这是我在项目中使用的一种方式，生成32位的字符串，是唯一的值。最通用的，适用于所有数据库。
