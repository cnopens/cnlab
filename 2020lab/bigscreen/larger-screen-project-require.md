#larger-screen-project-require.md
---

1. 支持直连MySQL、SQL Server、PostgreSQL、Greenplum、Oracle、SAP HANA、Hive、Kylin等数据源，也可上传本地Excel/Csv文件或通过API连接数据

2. 我们提供65+种基于百度ECharts和D3的可视化图表，以及10+种过滤组件，支持专业的GIS可视化效果和自定义图表效果，充分满足您多样化的可视化需求

3. 拖拽式编辑
使用可视化编辑器，所见即所得，拖拽生成可视化页面，让您轻松编辑，不写代码

4. 我们将BI报表和大屏功能融合，提供了多套酷炫的大屏模板，让你快速搭建科技感爆棚的大屏

5. 报表与大屏均支持数据表建模、拖拽字段绑定数据，以及过滤、下钻和联动等灵活交互，让您对数据轻松进行深度分析挖掘

6. 企业组织级别管理，项目空间数据隔离，基于角色和用户的授权机制，充分保证您的数据安全

Organ->projectSpace->Role&User

## Suger-app研究：
------------
添加数据源->
创建数据模型->(build model for pie,line.....)

-报表
制作报表并分析数据->可视化组件{自助}数据分析
数据管理

-展现
制作大屏->组件实现低成本cool Screen
数据管理


##数据管理

-数据源

-数据模型

数据绑定联动方式：
API后端获取下钻参数
API后端获取联动参数：
API后端获取URL额外参数

空间数目、报表个数、大屏个数、公开分享的大屏和报表个数不做任何限制。



6Rhqp7sM

use strict ";
module.exports={up:function(queryInterface,Sequelize){return queryInterface.renameColumn("
sugar_groups ","
key ","
group_key ")},down:function(queryInterface,Sequelize){return queryInterface.renameColumn("
sugar_groups ","
group_key ","
key ")}}


创建大屏产生的数据集：
{
	"dbHashes": ["d_ada15-5zmw9p9g-g2vp2r"],
	"tables": [{
		"tableId": "SGKDTWPSBMZUY3",
		"tableName": "t_category",
		"customTableHash": "",
		"level": 1,
		"dbHash": "d_ada15-5zmw9p9g-g2vp2r",
		"join": {
			"type": "inner",
			"leftTableId": "",
			"on": [{
				"leftField": "",
				"rightField": ""
			}]
		}
	}, {
		"tableId": "SGKDTWQ02I46RFP",
		"tableName": "t_document",
		"customTableHash": "",
		"level": 2,
		"dbHash": "d_ada15-5zmw9p9g-g2vp2r",
		"join": {
			"type": "inner",
			"leftTableId": "SGKDTWPSBMZUY3",
			"on": [{
				"leftField": "C_ID",
				"rightField": "C_CATEGORYID"
			}]
		}
	}],
	"dimensions": {
		"SG671F1D8C4D9B6B7D": {
			"type": "dimension",
			"tableId": "SGKDTWPSBMZUY3",
			"field": "C_ID",
			"calculated": false,
			"expression": "",
			"alias": "C_ID",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGC7BDC67728377918": {
			"type": "dimension",
			"tableId": "SGKDTWPSBMZUY3",
			"field": "C_NAME",
			"calculated": false,
			"expression": "",
			"alias": "C_NAME",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGC72A2E2763C9E50C": {
			"type": "dimension",
			"tableId": "SGKDTWPSBMZUY3",
			"field": "C_PARENTID",
			"calculated": false,
			"expression": "",
			"alias": "C_PARENTID",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGF79B7A612068A62B": {
			"type": "dimension",
			"tableId": "SGKDTWPSBMZUY3",
			"field": "C_TYPE",
			"calculated": false,
			"expression": "",
			"alias": "C_TYPE",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG676B2E6321FE9016": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_ID",
			"calculated": false,
			"expression": "",
			"alias": "C_ID(t_document)",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG1E14E394CEF7B386": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_NAME",
			"calculated": false,
			"expression": "",
			"alias": "C_NAME(t_document)",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG67F76FE727D0E70D": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_LOCATION",
			"calculated": false,
			"expression": "",
			"alias": "C_LOCATION",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGF6C5FFF17E9B3933": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_SIZE",
			"calculated": false,
			"expression": "",
			"alias": "C_SIZE",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGADD53C99C343B502": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_CATEGORYID",
			"calculated": false,
			"expression": "",
			"alias": "C_CATEGORYID",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGDAAB4B9C46E3A220": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_CREATETIME",
			"calculated": false,
			"expression": "",
			"alias": "C_CREATETIME",
			"dataType": "datetime",
			"dataTypeInDB": "datetime",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG6F924B8C67EB779A": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_CREATEUSER",
			"calculated": false,
			"expression": "",
			"alias": "C_CREATEUSER",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG0E8947C212594C4F": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_UPDATETIME",
			"calculated": false,
			"expression": "",
			"alias": "C_UPDATETIME",
			"dataType": "datetime",
			"dataTypeInDB": "datetime",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG6A08234B8DD0B9C6": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_UPDATEUSER",
			"calculated": false,
			"expression": "",
			"alias": "C_UPDATEUSER",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG744E05E3E902EEEC": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_TIME",
			"calculated": false,
			"expression": "",
			"alias": "C_TIME",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGD1E57224266A0187": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_COMPANYID",
			"calculated": false,
			"expression": "",
			"alias": "C_COMPANYID",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGC6789C0A612E98A3": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_CITYID",
			"calculated": false,
			"expression": "",
			"alias": "C_CITYID",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG003947C26356CD54": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_PROPERTYTYPE",
			"calculated": false,
			"expression": "",
			"alias": "C_PROPERTYTYPE",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGBBE1172D6472C361": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_REPORTTYPE",
			"calculated": false,
			"expression": "",
			"alias": "C_REPORTTYPE",
			"dataType": "string",
			"dataTypeInDB": "varchar",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SG14C5BD9A69E437D7": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_ABSTRACT",
			"calculated": false,
			"expression": "",
			"alias": "C_ABSTRACT",
			"dataType": "string",
			"dataTypeInDB": "text",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		},
		"SGB65A09E6E7287BAB": {
			"type": "dimension",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_RECOMMENDTIME",
			"calculated": false,
			"expression": "",
			"alias": "C_RECOMMENDTIME",
			"dataType": "datetime",
			"dataTypeInDB": "datetime",
			"isHidden": false,
			"renameHash": "",
			"hierarchyId": "",
			"pathIds": [],
			"convert": {
				"type": "",
				"label": "",
				"original": "",
				"dataType": ""
			}
		}
	},
	"measures": {
		"SGB8A0782C77795B10": {
			"type": "measure",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_SERIALNUM",
			"calculated": false,
			"expression": "",
			"alias": "C_SERIALNUM",
			"dataType": "int",
			"dataTypeInDB": "int",
			"isHidden": false,
			"defaultAggregator": "SUM",
			"convert": {
				"type": ""
			},
			"format": {
				"accuracy": -1,
				"dataFormat": "",
				"unit": ""
			}
		},
		"SG0D2E9C5200274297": {
			"type": "measure",
			"tableId": "SGKDTWQ02I46RFP",
			"field": "C_RECOMMEND",
			"calculated": false,
			"expression": "",
			"alias": "C_RECOMMEND",
			"dataType": "int",
			"dataTypeInDB": "tinyint",
			"isHidden": false,
			"defaultAggregator": "SUM",
			"convert": {
				"type": ""
			},
			"format": {
				"accuracy": -1,
				"dataFormat": "",
				"unit": ""
			}
		}
	},
	"dimensionMenu": [{
		"type": "menu",
		"menuId": "SGKDTWPSBP42WN2AD",
		"name": "默认",
		"nodes": ["SG671F1D8C4D9B6B7D", "SGC7BDC67728377918", "SGC72A2E2763C9E50C", "SGF79B7A612068A62B"]
	}, {
		"type": "menu",
		"menuId": "SGKDTWQ02KQXHODSW",
		"name": "t_document",
		"nodes": ["SG676B2E6321FE9016", "SG1E14E394CEF7B386", "SG67F76FE727D0E70D", "SGF6C5FFF17E9B3933", "SGADD53C99C343B502", "SGDAAB4B9C46E3A220", "SG6F924B8C67EB779A", "SG0E8947C212594C4F", "SG6A08234B8DD0B9C6", "SG744E05E3E902EEEC", "SGD1E57224266A0187", "SGC6789C0A612E98A3", "SG003947C26356CD54", "SGBBE1172D6472C361", "SG14C5BD9A69E437D7", "SGB65A09E6E7287BAB"]
	}],
	"measureMenu": [{
		"type": "menu",
		"menuId": "SGKDTWPSBPAQYFC9Q",
		"name": "默认",
		"nodes": []
	}, {
		"type": "menu",
		"menuId": "SGKDTWQ02KGI5WR5O",
		"name": "t_document",
		"nodes": ["SGB8A0782C77795B10", "SG0D2E9C5200274297"]
	}],
	"filters": [],
	"limits": {},
	"synTable": false
}


登录记录：

jQV9Awuc


##Sugar-app主要功能



##Dashuo Smart Screen 项目

20200818->数据设计->创建项目

	- 初始化图元素数据（json-在项目中加载)


	- 权限数据

	- 组织数据


项目名称：dss

```技术架构
目标： 实时性，api设计，

研究方向：
1. 
2. 



````

Nodejs:
- api框架设计
- 代码规范
- 设计模式：mvc(express/sails)
- 全栈(Meteor)
- Rest Api(Loopback)企业级



## Exasol->





##技术架构选型
