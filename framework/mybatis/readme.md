# source analysis

# basic knowlogue

### 关于Mybatis的@Param注解 
---

### params type 




## mybatis /Example 参数传递方式
---
    public List<TableA> query(String name) {
        Example example = new Example(TableA.class);
        example.createCriteria()
                .andLike("name", "%"+ name +"%");
        example.setOrderByClause("CREATE_TIME DESC");

        return this.tableAMapper.selectByExample(example);
    }
    ---