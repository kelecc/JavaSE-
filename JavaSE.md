# JavaSE基础知识

## 1. IO流

### File类

> File类代表与平台无关的文件和目录

**public File(path)**

> * path {String} 路径
> * 返回 File对象
>
> * 以path为路径创建File对象。

**File.getName()**

> * 返回 当前的文件或者目录名

**File.getPath()**

> * 返回 new File时候写的路径

**File.getAbsolutePath()**

> * 返回 File的绝对路径

**File.getAbsoluteFile()**

> * 返回 当前绝对路径的File对象

**File.getParent()**

> * 返回 当前File的父级路径

**File.renameTo(dest)**

> * dest {File} 新名字的File对象
> * 返回 {boolean} 成功true 失败false
> * 给当前File对象改名字

```java
File.renameTo(new File("D/test/newName"));
```

**File.exists()**

> * 返回 {boolean} 存在为true 不存在false
> * 判断当前File对象是否存在

**File.canRead()**

> * 返回 可读为true 否则false
> * 判断当前File对象是够可读

**File.canWrite()**

> * 返回 可写为true 否则false
> * 判断当前File对象是够可写

**File.isFile()**

> * 返回 是文件为true 否则false
> * 判断File对象是不是文件

**File.isDirectory()**

> * return {boolean}
> * 判断当前的File对象是不是目录

**File.lastModified()**

> * 返回 {long} 毫秒数
> * 获取文件的最后修改时间

**File.length()**

> + 返回 {long} 文件长度 字节
> + 获取当前文件的长度



***



### 文件字节流

> 在读取文件时如果文件不存在会抛出异常
>
> 写文件时会覆盖文件

**FileInputStream**

* in.read()

  > 返回读取的长度 到头了返回-1

```java
try {
    FileInputStream in = new FileInputStream("E:/test.txt");
    byte[] b = new byte[10];
    int len = 0;
    while((len = in.read(b)) != -1) {
        System.out.println(new String(b,0,len))
    }
    in.close();
}catch(Exception e) {
	System.out.println(e);
}
```

**FileOutputStream**

* out.write(Bytes)

  >   tes {bytes[]} 要写入的数据

```java
try {
      FileOutputStream out = new FileOutputStream("E:/Study/随堂笔记/JavaSE/JavaSE.txt");
      String str = "kelehhhhhh!";
       // 把数据写入内存
      out.write(str.getBytes());
       //把数据写入硬盘
      out.flush();
      out.close();
}catch(Exception e) {
      System.out.println(e);
 }
```





***


### 文件字符流

> 　只适合操作内容是字符的文件
>
> ​    用法和文件字节流一样，字符流用char[]
>
> ​    在读取文件时如果文件不存在会抛出异常
>
> ​	写文件时会覆盖文件

**FileReader**

**FileWriter**



---



### 缓冲字节流

> 缓冲流就是先把数据缓冲到内存中 ，在内存中去做io操作，比基于硬盘的io操作快75000多倍

**BufferedInputStream**

* new BufferedInputStream(FileInputStream in) 

**BufferedOutputStream**

* new BufferedOutputStream(FileOutputStream out)



***



### 缓冲字符流

**BufferedReader**

* new BufferedReader(FileReader r)

**BufferedWriter**

* new BufferedWriter(FileWriter w)

***



### 转换流 还没写！！！！！！！！！





### Properties

> 读取键值对 key value
```java
//通过io流读取文件
FileReader r = new FileReader("E/test.txt");
//创建属性类对象
Properties pro = new Properties();
//加载
pro.load(r);
//关闭流
r.close();
//通过key获取value
String value = pro.getProperties("key");
```
* 资源绑定器
```java
//文件必须在src下面，拓展名必须是properties
ResourceBundle bundle = ResourceBundle.getBundle("文件名");
String value = bundle.getString("key");
```
---

## 2.多线程



### 实现线程的三种方法

1. 继承**Thread**类 重写**run**方法
2. 实现**runnable**接口 重写**run**方法，也可以用匿名内部类。
3. 实现**Callable**接口（JDK8新特性），能接受线程返回值。但是效率低。
```java 
public class Test {
	public static void mian(String[] args) throws Exception {
		//第一步 创建未来任务类对象
		FutureTask task = new FutureTask(new Callable(){
			@Override
			public Object call() throws Exception {
				System.out.println("hhh");
				return 100;
			}
		});
		
		//创建线程对象
		Thread t = new Thread(task);
		t.start();
		//获取线程返回结果 会让main方法受阻 等待t线程结束返回结果
		Object obj = task.get();
		
	}
}
```
***

### 常用的方法

**String getName()**

> 获取线程名字

**void setName(String name)**

> 设置线程名字

**static Thread currentThread()**

> 返回当前正在执行的线程对象

**static void sleep(long time)**

> 休眠多少毫秒
>
> **是静态方法 在哪个线程里调用就是哪个线程休眠**

**void interrupt()**

> 中断线程的睡眠
>
> 依靠Java的异常处理机制，会让sleep的try catch捕获到异常

**终止线程**

* void stop()

> 已过时 容易导致数据丢失

* 合理终止线程

> ```java
> class Test {
> 	boolean run = true;
>     
>     public void run() {
> 		if(run) {
>             ....
> 		}else {
>             return;
> 		}
>     }
> }
> ```
>

----

###  线程调度

**void setPriority(int newPriority)**

> 设置线程的优先级 最高10 默认5 最低1

**int getPriority()**

> 获取线程优先级

**static void yield()**

> 让位方法 不是阻塞 让当前线程从运行状态到就绪状态

**void join()**

> 线程合并

---------

### synchronized

> 局部变量不存在线程安全问题
>
> 静态变量在方法区中，实例变量在堆中，堆和方法区数据是共享的，他们可能有线程安全问题。
>
> 里头放共享对象 不能是**null**
>
> ``` java
> synchronized(obj) {
> }
> ```

* synchronized 出现在实例方法上锁对象是this，整个方法体同步，效率低，不常用。

* 类锁 synchronized出现在静态方法上。保护静态变量安全。类锁永远只有一把。

### 死锁

> synchronized在开发中最好不要嵌套 容易发生死锁。

### 解决线程安全问题

> 不能一上来就synchronized，这样效率低。体验差。

1. 尽量使用局部变量代替实例变量和静态变量
2. 如果必须是实例变量，可以考虑创建多个对象，这样数据就不会共享了。
3. 如果不能使用局部变量，对象也不能创建多个，这个时候只能使用synchronized。
***
### 守护线程

> **t.setDaemon(true)**
***
### 定时器

```java
public class Test {
	public static void main(String args[]) {
		//创建定时器对象
		Timer timer = new Timer();
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		Date firstTime = sdf.parse("2021-06-01 04:34:33");
		timer.schedule(new myTimerTask(),firstTime,1000*10);
	}
}
class myTimerTask extends TimerTask {
	@Override
	public void run(){
		System.out.println("完成任务");
	}
}
```
***
### **wait**和**notify**
1. **wait**和**notify**不是线程对象的方法，是Object自带的。
**wait**和**notify**方法不是通过线程对象调用（不是t.wait()）
2. **wait**方法作用
>  Object o = new Object();
>  o.wait();
>  表示：让正在o对象上活动的进程进入等待状态并且释放之前占有的o对象的锁再无限期等待，直到被唤醒。	
3. **notify**的作用
>  o.notify();
>  表示唤醒正在o对象上等待的进程



----

## 3.异常

### finally 面试题
***
## 4.反射
> 反射机制常用类
> java.lang.Class 代表整个类 字节码
> java.lang.reflect.Method 代表字节码中的方法字节码
> java.lang.reflect.Constructor 代表构造字节码
> java.lang.reflect.Field 代表属性字节码
### 获取字节码的三种方式
* Class.forName("java.util.Date");
> 通过Class的静态方法获取
* Object.getClass();
> 通过类的getClass()方法
* Object.class
> java中任何一种类型都有class属性
### 通过反射机制实例化对象

> 灵活 结合Properties可以灵活创建对象

```java
Class c = Class.forName("java.lang.String");
Object obj = c.newInstance();
```
***
### 文件的绝对路径
```java
//获取文件的绝对路径
String path = Thread.currentThread().getContextClassLoader().getResource("src里的路径").getPath();
//以流的形式返回
InputStream in = Thread.currentThread().getContextClassLoader().getResourceAsStream("路径");
```
> 文件要放在src下面
***
### 反射Filed 属性
```java
//获取Student字节码
Class stuClass = Class.forName("Student");
//new Student对象
Object stu = stuClass.newInstance();
//获取name属性对象
Filed stuName = stu.getDeclaredFiled("name");
//设置stu的name属性为kele
//私有属性需要**stuName.setAccessible(true)**来打破封装（**缺点**）
stuName.set(stu,"kele");
//获取stu对象的name属性的值
stuName.get(stu);
```
***
### 反射Method 方法
```java
//获取字节码文件
Class stuClass = Class.forName("Student");
//new Student对象
Object stu = stuClass.newInstance();
//反射所有的方法 得到一个Method数组
Method[] methods = stuClass.getDeclaredMethods();
for(Method method : methods) {
	//获取方法名字
	String name = method.getName();
	//获取方法返回值类型(getReturnType()返回一个Class对象)
	String returnType = method.getReturnType().getSimpleName();
}



```
> 
***
### 可变长参数
```java
//必须放在最后面 只能有一个
//可变长参数可以看做一个数组
public static void test(String s,int... n){};
```