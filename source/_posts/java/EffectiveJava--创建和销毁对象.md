---
title: EffectiveJava--对象的通用方法
date: 2018-01-10 22:00:46
tags: [java]
---
## 第1条，考虑用静态工厂方法代替构造器
### 优点:
- 有名称，作用更清晰
- 可以实现单例
- 可以返回原类型的任何子类型
- 创建参数化实例对象时，代码更简洁

### 缺点:
- 类如果不含公有或者受保护的构造器，就不能被子类化  
- 与其他静态方法没区别

## 第2条，遇到多个构造器参数时考虑用建造者(builder)模式  
构造器遇到多个参数组合的时候，需要定义不通组合的构造器，复杂而且顺序容易弄错。使用建造者模式就可以解决此类问题。  
优点:灵活链式构建  
缺点:需创建构建器，有开销  
## 第3条，用私有构造器或者枚举类型强化Singleton属性  
序列化，反射安全？  
单元素的枚举类型已经成为实现Sibgleton的最佳方法。  
## 第4条，通过私有构造器强化不可实例化的能力  
不需要被实例化的类，添加私有构造函数，并且在构造函数中抛出异常，来避免被实例化。  
```java
    public class UtilityClass{
		private UtilityClass(){
			throw new AssertionError();
		}
	}
```
## 第5条，避免创建不必要的对象  
如下情况可以不创建对象，示例如下：
1、""字符串本身就是一个String对象，再new会重复创建String对象。 
```java
    String s = new String("this is a wrong");//bad  
    String s = "good";//good  
```
2、提供静态方法和构造函数的不可变类，静态方法优于创建对象  
```java
    Boolean constructObj = new Boolean("true");//bad  
    Boolean staticMethodObj = Boolean.valueOf("true");//good  
```
3、作为常量使用的可变类。比如作为固定开始日期的Date，只需要实例化一次即可重复使用  
4、优先使用基本类型而不是装箱基本类型，当心无意识的自动装箱  
```java
    Long sum = 0L;//bad  
    long sum = 0L;//good  
```
不是不要创建对象，小对象的开销很小。  
重量级的对象才需要维护资源池，例如数据库连接。  
## 第6条，消除过期的对象引用  
例如，Stack中pop的对象引用，需要主动释放内存,同时也能尽早暴漏错误调用  
```java
    public Object pop(){
        if(size == 0)
            throw new EmptyStackException();
        Object result = elements[--size];
        elements[size] = bull; //释放过期的引用
        return result;
    }
```
清空对象引用应该是一种例外，而不是一种规范行为。通过再尽量小的作用域内，变量及时结束其生命周期来释放。  
关于内存泄露的来源：  
- 类自己管理的内存，程序猿需要关注其释放
- 缓存，可以使用WeakHashMap代替缓存，需要定期清理，或者类似LinkedHashMap的removeEldestEntry方法清理。
- 监听器和其他回调。确保回调立即回收的方法保存他们的弱引用。例如保存成WeakHashMap的键。 

## 第7条，避免使用终结方法(finalizer)#
finalizer的缺点：
- 不能保证会被及时的执行，间隔是任意的。
- 在不同JVM平台表现不同。
- finalizer线程的优先级比程序中其他线程低很多，会导致队列积压，内存溢出。
- finalizer可能不会被执行。
需要注意的地方：
- 所以不应该依赖finalizer来更新重要的持久状态。  
- System.gc和System.runFinalization可以增加finalizer被执行的机会，单不能保证一定被执行。  
- finalizer中抛出的异常如果未捕获，该异常可能被忽略（警告也不会被打印），并且finalizer也会终止。继续使用此对象时会产生不确定的结果。
- 使用finalizer会导致对象的创建和销毁时间大幅增加，甚至几百倍。
- 建议定义一个显式的终止方法释放资源。例如InputStream/FileOutputStream/Connection的close等。本地对等体需要finalizer或者显式的终止方法才能释放，GC不会自动释放。对于需要及时释放资源的情况，应该用显式的终止方法来释放。
- 显式的终止方法通常与try-finally结构结合起来使用，以确保及时终止
- 子类的finalizer，需要在try-finally的finally中调用超类的finalizer。
```java
    @Override protected void finalize() throws Throwable {
		 try{
			 //Finalize subClass state
		 } finally {
			 super.finalize();
		 }
    } 
```
- 可以考虑使用匿名类来充当总结方法守卫者，确保能够调用finalizer.因为成员变量会被主动释放，从而触发finalizer。
```java
    private final Object finalizerGuardian = new Object(){
		 @Override protected void finalize() throws Throwable {
			 super.finalize();
	    } 
	 };
```