#java枚举专题.md
---
1. 常量定义

    public enum WeekDay {
        SUN, MON, TUE, WED, THT, FRI, SAT
    }

2. swich

    public enum WeekDay {
        SUN, MON, TUE, WED, THT, FRI, SAT
    }
     
    public class SelectDay{
        WeekDay weekday = WeekDay.SUN;
        public void select(){
            switch(weekday){
                case SUN:
                    weekday = WeekDay.SUN;
                    bread;
                ...
            }
        }
    }

3. 向枚举添加新方法

    public enum Color {  
        RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
        // 成员变量  
        private String name;  
        private int index;  
        // 构造方法  
        private Color(String name, int index) {  
            this.name = name;  
            this.index = index;  
        }  
        // 普通方法  
        public static String getName(int index) {  
            for (Color c : Color.values()) {  
                if (c.getIndex() == index) {  
                    return c.name;  
                }  
            }  
            return null;  
        }  
        // get set 方法  
        public String getName() {  
            return name;  
        }  
        public void setName(String name) {  
            this.name = name;  
        }  
        public int getIndex() {  
            return index;  
        }  
        public void setIndex(int index) {  
            this.index = index;  
        }  
    }  

4. 覆盖枚举方法

    public enum Color { 
        RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4); 
        // 成员变量
        private String name; private int index; 
        // 构造方法 
        private Color(String name, int index) { 
            this.name = name; this.index = index; 
        } 
        //覆盖方法 
        @Override 
        public String toString() { 
        return this.index+"_"+this.name; 
        } 
    }

5. 实现接口

    public interface Behaviour { 
        void print(); 
        String getInfo(); 
    } 
    public enum Color implements Behaviour{ 
        RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4); 
        // 成员变量 
        private String name; 
        private int index; 
        // 构造方法 
        private Color(String name, int index) { 
            this.name = name; this.index = index; 
        } 
        //接口方法 
        @Override 
        public String getInfo() { 
            return this.name; 
        } 
        //接口方法 
        @Override 
        public void print() { 
            System.out.println(this.index+":"+this.name); 
        } 
    }

6. 接口组织枚举

    public interface Food { 
        enum Coffee implements Food{ 
            BLACK_COFFEE,DECAF_COFFEE,LATTE,CAPPUCCINO 
        } 
        enum Dessert implements Food{ 
            FRUIT, CAKE, GELATO 
        } 
    }

7. 枚举集合

    public class Test {
        public static void main(String[] args) {
            EnumSet<WeekDay> week = EnumSet.noneOf(WeekDay.class);
            week.add(WeekDay.MON);
            System.out.println("EnumSet中的元素：" + week);
            week.remove(WeekDay.MON);
            System.out.println("EnumSet中的元素：" + week);
            week.addAll(EnumSet.complementOf(week));
            System.out.println("EnumSet中的元素：" + week);
            week.removeAll(EnumSet.range(WeekDay.FRI, WeekDay.SAT));
            System.out.println("EnumSet中的元素：" + week);
        }
8. 项目实战例子

spring/boot 构建返回对象实现
