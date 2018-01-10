---
title: EffectiveJava--对象的通用方法
date: 2018-01-10 22:00:46
tags: [java]
---
## 第8条，覆盖equals时请遵守通用约定#
除非必要，不要重写equals方法。注意：
- 类的每个实例本质上都是唯一的。这时候继承的Object的euqals方法是完全正确的，不需要重写
- 不关心类是否提供“逻辑相等”功能。比如Random类。
- 超类（父类）已经重写了quals，对于子类是适用的，无需重写。例如:AbstractSet——>Set，AbstractList——>List，AbstractMap——>Map
- 类是私有的或者是包级私有的，并且确定它的equals方法永远不会被调用时。可以进行如下重写，防止意外调用：
```java
    @Override public boolean equals(Object o){
        throw new AssertionError();
    }
```
- 如果类具有自己特有的“逻辑相等概念”，而且超类还没有覆盖equals实现期望的行为，此时就需要重写equals方法。重写后被用做map的key，或者set的元素时才能表现出预期的行为。
- 对于“每个值至多只存在一个对象”的“值类”，Object的euqals方法等同于逻辑相等。例如：枚举类型
Object的euqals方法的规范：
- 自反性（reflexive）。对于任何非null的引用值x，x.equals(x)必须返回true。
- 对称性（symmetric）。对于任何非null的引用值x和y，当且仅当y.equals(x)返回true时，x.equals(y)必须返回true。
- 传递性（transitive）。对于任何非null的引用值x、y和z，如果x.equals(y)返回true，并且y.equals(z)也返回true，那么x.equals(z)必须返回true。
- 一致性（consistent）。对于任何非null的引用值x和y，只要x，y没有被修改过，多次调用x.equals(y)，返回值一定是一致的。
- 对于任何非null的引用值x，x.equals(null)必须返回false。
如果不符合这些规范，程序会表现异常，甚至是崩溃，而且很难找到根源。
在子类和超类，子类之间，以及同一个超类的子类之间，equals就很有可能出现问题。我们无法在扩展可实例化的类的同时，既增加新的值组件，同时又保留equals约定。推荐用复合的方式代替继承，每个属性分别equals。
```java
    public class ColorPoint {
		 private Point point;
		 private Color color;
		 @Override public boolean equals(Object o){
			 if(!(o instanceof ColorPoint)){
				 return false;
			 }
			 ColorPoint cp = (ColorPoint)o;
			 return cp.point.equals(point) && cp.color.equals(color);
		 }
	 }
```
equals实现时一般要使用instanceof进行类型检查，对null检查时必定返回false，所以没必要再检查是否为null。
实现高质量equals方法的诀窍：
- 使用==操作符检查“参数是否为这个对象的引用”。（出于性能优化方面考虑）
- 使用instanceof操作符检查“参数是否为正确的类型”。所谓“正确的类型”一般是指类，如果接口改进了equals约定，允许实现该接口的类之间进行比较，就可以使用接口。例如Set、List、Map和Map.Entry。
- 把参数转换成正确的类型。即转换成当前对象的类型。
- 对该类中的每个关键域（属性），分别进行比较。除float和double的基本值类型，直接使用==比较。float使用Float.compare，double使用Double.compare。如果要检查数组中的每个元素，可以使用Arrays.equals。对于引用域，可能存在null为合法值，需要进行null检查。多个域的情况下，要优先比较开销低的域。
- 要确保equals符合对称性，传递性和一致性。 
- 不要企图让equals做过多的比较
- 不要将equals方法的参数类型必须是Object，不能改成当前类。因为没有重写Object.equals，只是重载了。

## 第9条：重写equals时必须重写hashCode#
如果没有重写hashCode方法，就无法作为散列集合（例如：HashMap，HashSet和HashTable）的key。因为在取值时会优先通过hashCode比较。

## 第10条，始终要重写toString#
toString在打印日志时，可以明确当前实例的重要信息，便于理解。而且要注意格式一旦确定就不要修改，格式要便于字符串解析，去除对应域的值。

## 第11条，谨慎的重写clone#
如果要支持clone需要实现Cloneable接口。
clone方法就是一个构造器，你必须确保它不会伤害到原始的对象，并确保正确地创建被克隆对象中的约束条件。
* 对于子类型的域直接使用Object.clone即可。
* 对于可变的引用类型，可变引用类型的集合，就需要递归的调用clone。以避免伤害到原始对象。
```java
    @Override public Stack clone() {
		 try{
			 Stack result = (Stack) super.clone();
			 result.date = date.clone();
			 result.elements = elements.clone();
             return result;
		 } catch (CloneNotSupportedException e) {
			 throw new AssertionError();
		 }
	 }
```
* clone架构与引用可变对象的final域的正常用法是不相兼容的，除非在clone对象和原始对象间可以安全的共享此可变对象。
* 对于散列桶数组，需要遍历每个数据元素，进行深度拷贝进行clone。
* clone不应该在构造的过程中，调用新对象中任何非final的方法，防止新旧对象不一致。
* 对于有线程安全要求的类，clone方法要处理好同步。Object.clone没有同步。
* 对于ID或者时间戳一类的值域，注意clone后进行修正。
* 可以考虑使用拷贝构造器或者拷贝工厂方法，更加灵活的实现拷贝操作，比如带参数或者类型转换（比如用数组生成List对象）。
* 如果不能提供良好保护的clone方法，他的子类就不可能实现Cloneable接口。

编程通则：永远不要让客户去做任何类库能够替客户完成的事情。
## 第12条，考虑实现Compareable接口#
compareTo的通用约定与equals类似。
注意：HashSet实例中添加new BigDecimal("1.0")和new BigDecimal("1.00")，这个集合将含有两个元素。而如果把类型HashSet改成TreeSet，则只有一个元素。因为HashSet使用equals比较，TreeSet使用comapreTo比较。