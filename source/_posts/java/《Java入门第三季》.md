---
title: 《Java入门第三季》
date: 2019-06-14 00:03:32
categories: Java
tags: Java基础
description: 异常、常用类、集合框架
---

### 异常
<!-- more -->
- 概念
    - Throwable是父类，下有Error和Exception两个子类
    - Error：VirtualMachineError（虚拟机错误），ThreadDeath（线程死锁）
    - Exception：RuntimeException（非检查异常），检查异常
        - 非检查异常：NullPointerException, ArrayIndexOutOfBoundsException, ClassCatException, ArithmeticException
        - 检查异常：IOException, SQLException
- 异常处理
    - `try...catch`
    ```java
    try{
        System.out.println("输入年龄：");
        Scanner in = new Scanner(System.in);
        int age = in.nextInt();
        System.out.println(age);
    }catch(InputMismatchException e){
        System.out.println("请输入整数！");
    }
    ```
    - 多重catch块（先子类后父类）及`try...catch...finally`
    ```java
    try{
        Scanner in = new Scanner(System.in);
        int one = in.nextInt();
        int two = in.nextInt();
        System.out.println(one/two);
    }catch(InputMismatchException e){
        System.out.println("请输入整数！");
    }catch(ArithmeticException e){
        System.out.println("除数不能为零！");
    }catch(Exception e){
        e.printStackTrace();
        System.out.println("程序发生异常！");
    }finally{
        System.out.println("finally执行！");
    }
    ```
- 异常抛出
    - throw关键字：throws声明将要抛出何种类型的异常，throw抛出异常
    ```java
    public void compute() throws Exception{
        System.out.println(5/0);
    }
    
    public void compute2(){
        try{
            System.out.println(5/0);
        }catch(Exception e){
            System.out.println(e.getMessage());
        }
    }
    ```
- 自定义异常
    - class 自定义异常类 extends 异常类型
    ```java
    public class DrunkException extends Exception{
        public DrunkException(){
        }
        
        public DrunkException(String message){
            super(message);
        }
    }
    ```
- 异常链
    - 示例
    ```java
    public void test1() throws DrunkException{
        throw new DrunkException("喝酒不开车！");
    }

    public void test2(){
        try {
            test1();
        } catch (DrunkException e) {
            RuntimeException newExc = new RuntimeException("禁止开车");
            newExc.initCause(e);
            // RuntimeException newExc = new RuntimeException(e);
            throw newExc;

        }
    }

    public static void main(String[] args){
        exception hello = new exception();
        // 异常链
        try{
            test2();
        }catch(Exception e){
            e.printStackTrace();
        }
    }
    ```

### 常用类
- 字符串
    - 字符串的不变性
        - `String s1 = "imooc"`不等于`String s2 = new String("imooc")`
        - 每次new一个字符串会产生一个新的对象，即使内容相同对于==运算是fasle（比较内容用equals）
    - String类的常用方法
        - `length()，indexOf(), substring()`
        - 字符串索引不能用s[i]，要用`s.charAt(i)`
    - 文件名和邮箱格式验证
        - 练习
        ```java
        String fileName = "immoc.com.java";
        String email = "louis@imooc.com";
        int index = fileName.lastIndexOf('.'); // 最后一次出现"."的位置
        String prefix = fileName.substring(index+1); // 获取文件后缀名
        int index2 = email.indexOf('@');
        int index3 = email.indexOf('.');
        if(index != -1 && index !=0 && prefix.equals("java")){
            // "."存在，且不能在首位，文件后缀名是java
            System.out.println("文件名正确");
        }
        if(index2 != -1 && index2 < index3){
            // "@"存在，且在"."之前
            System.out.println("邮件格式正确");
        }
        ```
    - StringBuilder与StringBuffer类
        - 为什么：String类具有不可变性，频繁操作字符串时会产生很多额外的临时变量，可以用StringBuilder与StringBuffer类
        - 区别：StringBuffer是线程安全的，StringBuilder无，故性能较好
        ```java
        StringBuilder str1 = new StringBuilder("imooc");
        str1.append(" hello");
        str1.insert(5, 1);
        System.out.println(str1);
        ```

- 包装类
    - 作用：为了让基本数据类型也具备对象的特性
    - 举例：Integer，Float，Double
    - 自动装箱和拆箱机制（JDK1.5）
        - 装箱（把基本类型转换成包装类）
        ```java
        Integer x = new Integer(5); // 手动
        Integer y = 5; // 自动
        ```
        - 拆箱（包装类 -> 基本类型）
        ```java
        int m = x.intValue(); // 手动
        int n = x; // 自动 
        ```
- 基本类型与字符串的转换
    - 基本类型 -> 字符串：
        1. 包装类的toString()
        2. String类的valueOf()
        3. 空字符串""+基本类型
        ```java
        int a = 1;
        String str1 = Integer.toString(a);
        String str2 = String.valueOf(a);
        String str3 = ""+a;
        ```
    - 字符串 -> 基本类型：
        1. 包装类的parseXXX方法
        2. 包装类的valueOf()
        ```java
        int b = Integer.parseInt(str);
        int c = Integer.valueOf(str);
        ```
- Date类
    - 导入：`java.util.Date`
    - 格式化输出
    ```java
    Date d = new Date();
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    String today = sdf.format(d);
    ```
    - 文本转时间
    ```java
    String day = "2014年6月1日 21:05:36";
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
    Date d = sdf.parse(day);
    ```
- Calendar类
    - 示例
    ```java
    Calendar c = Calendar.getInstance();
    // MONTH, DAY_OF_MONTH, HOUR_OF_DAY, MINUTE, SECOND
    int year = c.get(Calendar.YEAR); 
    Date d = c.getTime();
    ```
- Math类
    - 取整：`round()，floor()，ceil()`
    - `random()`：返回[0,1)之间的随机浮点数
    ```java
    int num = (int) Math.random()*10;
    ```

### 集合框架
- 概念：Collection和Map
    - Collection：List (ArrayList)、Queue (LinkedList)、Set (HashSet)
    - Map：HashMap
    - 重要的接口和类：Collection接口、Map接口、Collections工具类、Comparable接口、Comparator接口
- List接口方法
    - 初始化：`List coursesList = new ArrayList()`
    - `add()`：`coursesList.add(new Course("1","Java")`
    - `get()`：`Course tmp = (Course) coursesList.get(0)`
    - `addAll()`：`coursesList.addAll(Arrays.asList({new Course("2","Python"), new Course("3","C++"}))`
    - `set()`：`coursesList.set(0, new Course("4","Algorithm"))`
    - `remove()`：`coursesList.remove(coursesList.get(0)) //.remove(0)`
- Set接口方法
    - 同List的方法
    - 初始化：`Set<Course> courses = new HashSet<Course>()`
    - `add()`：`student.courses.add(new Course("1","Java"))`
- Map接口方法
    - 初始化：`Map<String, Student> students = new HashMap<String, Student>()`
    - `put()`：`students.put(ID, newStu)`（同时可以修改）
    - `remove()`：`students.remove(id)`
    - keySet
        - `keySet()`
        ```java
        Set<String> keySet = students.keySet();
        System.out.println("size: "+students.size());

        for(String id: keySet){
            Student stu = students.get(id);
            if(stu != null){
                System.out.println("Student "+stu.name);
            }
        }
        ```
    - entrySet
        - `getKey()` 和`getValue()` 方法
        ```java
        Set<Entry<String, Student>> entrySet = students.entrySet();
        for(Entry<String, Student> entry: entrySet){
            System.out.println("key: "+entry.getKey() + ", value: "+entry.getValue());
        }
        ```
- 集合框架的查询
    - `contains()`：测试List是否存在某课程（但是不同对象就有不同值）
    ```java
    Course c = (Course) coursesList.get(0);
    System.out.println(coursesList.contains(c)); // true
    Course c2 = new Course(c.id, c.name);
    System.out.println(coursesList.contains(c2)); // false
    ```
    - 解决方法：重写`equals()`方法
    ```java
    public boolean equals(Object obj){
        if(this==obj)
            return true;
        if (obj==null)
            return false;
        if(!(obj instanceof Course))
            return false;
        Course c = (Course) obj;
        if(this.name == null){
            if(c.name == null)
                return true;
            else
                return false;
        }
        else{
            if(this.name.equals(c.name))
                return true;
            else
                return false;
        }
    }
    ```    
    - `contains()`：判断Set中课程是否存在
        - 存在跟List相同的问题，需重写`equals()`和`hashCode()`
        - 但是，有bug！写完还是输出false
    - `indexOf()`：查询位置
        - `coursesList.indexOf(c3)`
    - `contains()`：判断Map中课程是否存在
        - `students.containsKey(id)`
        - `students.containsValue(name)`
- Comparable 和 Comparator 接口
    - `Collections.sort()`：可对基本类型进行排序
    ```java
    List<Integer> integerList = new ArrayList<Integer>();
    List<String> stringList = new ArrayList<String>();
    Collections.sort(integerList);
    Collections.sort(stringList);
    // 问题：不能对Student这样的自定义类排序
    Collections.sort(studentList); // 报错
    ```
    - Comparable接口：默认比较规则（`compareTo()`）
    ```java
    // 重写Student类的compareTo方法
    @Override
    public int compareTo(Student o) {
        return this.id.compareTo(o.id);
    }
    // 调用
    Collections.sort(studentList);
    ```
    - Comparator接口：临时比较规则（`compare()`）
    ```java
    // 新建StudentComparator类
    public class StudentComparator implements Comparator<Student> {

        @Override
        public int compare(Student o1, Student o2) {
            return o1.name.compareTo(o2.name);
        }
    }
    // 调用
    Collections.sort(studentList, new StudentComparator());
    ```
    