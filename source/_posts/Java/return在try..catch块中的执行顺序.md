---
title: return在try...catch块中的执行顺序
tags:
    - Java  
---
Java中的try...catch块用以在异常发生点终止执行代码，然后进入到匹配异常的catch块。如果将finally放在try...catch块后，则无论是否有异常finally中的代码总会被执行。
那么return在其中是怎么执行的呢？以一个demo引入。
## 示例
```java
public class Demo2 {
    public static void main(String[] args){
       Demo2 m = new Demo2();
       System.out.println(m.demo());
    }
public int demo(){
        int i=1;
        try{

            FileInputStream fis = new FileInputStream("d:\demo.txt");
        }catch (FileNotFoundException fnfe){
            System.out.println("No such file found");
            return i;
        }catch (IOException ioe){
        }finally {
            ++i;
            System.out.println("Doing finally");
        }
        return 0;
    }
```

### 输出
No such file found
Doing finally
1

在上面代码中，如果demo.txt不存在，则会报错输出No such file found。而此时，定义一个有返回值类型的方法时系统会自动声明一个堆栈上的内存地址即返回地址用来存放此方法返回的值，在执行catch块的return时会把return后面的值存放在事先声明的返回地址中，但并不结束此方法，而是跳去执行finally中的代码输出Doing finally，变量i值变为2但返回地址中存储的仍为1。最终再回来执行return i。

**如果在finally块中加入return i：**
```java
public int demo(){
        int i=-1;
        try{

            FileInputStream fis = new FileInputStream("d:\\demo.txt");
        }catch (FileNotFoundException fnfe){
            System.out.println("No such file found");
            return i;
        }catch (IOException ioe){
        }finally {
            ++i;
            System.out.println("Doing finally");
            return i;
        }
    }
```

### 输出
No such file found
Doing finally
2

此时finally块中也有return，会将返回地址中保存的值覆盖为2。

**如果将基本数据类型换成引用类型变量：**
```java
public class Demo2 {
    public static void main(String[] args){
       Demo2 m = new Demo2();
       System.out.println(m.demo().getId());
    }

    public Test demo(){
        Test t=new Test();
        try{

            FileInputStream fis = new FileInputStream("d:\\demo.txt");
        }catch (FileNotFoundException fnfe){
            System.out.println("No such file found");
            return t;
        }catch (IOException ioe){
        }finally {
            System.out.println("Doing finally");
            t.setId(2);
        }
        return null;
    }

}
class Test{
    private int id=1;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }
}
```
### 输出
No such file found
Doing finally
2

由于在执行return t时将对象t的地址拷贝至返回地址，finally块中对对象t的Id属性修改时即改变返回地址中的Id值。