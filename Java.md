

# Java注解
注解是一系列元数据，它提供数据用来解释程序代码，但是注解并非是所解释的代码本身的一部分。注解对于代码的运行效果没有直接影响。想像代码具有生命，注解就是对于代码中某些鲜活个体的贴上去的一张标签。简化来讲，注解如同一张标签。
注解的定义
通过 @interface 关键字进行定义。
public @interface TestAnnotation {
}
它的形式跟接口很类似，不过前面多了一个 @ 符号。上面的代码就创建了一个名字为 TestAnnotaion 的注解。
你可以简单理解为创建了一张名字为 TestAnnotation 的标签。

元注解
元注解是可以注解到注解上的注解，或者说元注解是一种基本注解，但是它能够应用到其它的注解上面。
如果难于理解的话，你可以这样理解。元注解也是一张标签，但是它是一张特殊的标签，它的作用和目的就是给其他普通的标签进行解释说明的。
元标签有 @Retention、@Documented、@Target、@Inherited、@Repeatable 5 种。

注解的属性
注解的属性也叫做成员变量。注解只有成员变量，没有方法。注解的成员变量在注解的定义中以“无形参的方法”形式来声明，其方法名定义了该成员变量的名字，其返回值定义了该成员变量的类型。
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation {
    int id();
    String msg();
}
上面代码定义了 TestAnnotation 这个注解中拥有 id 和 msg 两个属性。在使用的时候，我们应该给它们进行赋值。
赋值的方式是在注解的括号内以 value=”” 形式，多个属性之前用 ，隔开。

Java预置的注解
@Deprecated
这个元素是用来标记过时的元素，其实就是编译器识别后的提醒效果。
@Override
提示子类要复写父类中被 @Override 修饰的方法
@SuppressWarnings
阻止警告的意思。之前说过调用被 @Deprecated 注解的方法后，编译器会警告提醒，而有时候开发者会忽略这种警告，他们可以在调用的地方通过 @SuppressWarnings 达到目的。
@SafeVarargs
参数安全类型注解。它的目的是提醒开发者不要用参数做一些不安全的操作,它的存在会阻止编译器产生 unchecked 这样的警告。它是在 Java 1.7 的版本中加入的。
@FunctionalInterface
函数式接口注解，这个是 Java 1.8 版本引入的新特性。函数式编程很火，所以 Java 8 也及时添加了这个特性。
函数式接口 (Functional Interface) 就是一个具有一个方法的普通接口。

注解的提取
形象的比喻就是你把这些注解标签在合适的时候撕下来，然后检阅上面的内容信息。
注解与反射
  注解通过反射获取。首先可以通过 Class 对象的 isAnnotationPresent() 方法判断它是否应用了某个注解
      public boolean isAnnotationPresent(Class<? extends Annotation> annotationClass) {}
  然后通过 getAnnotation() 方法来获取 Annotation 对象。
      public <A extends Annotation> A getAnnotation(Class<A> annotationClass) {}
  或者是 getAnnotations() 方法。
      public Annotation[] getAnnotations() {}
  前一种方法返回指定类型的注解，后一种方法返回注解到这个元素上的所有注解。
  
注解的使用场景
注解有许多用处，主要如下： 
- 提供信息给编译器： 编译器可以利用注解来探测错误和警告信息 
- 编译阶段时的处理： 软件工具可以用来利用注解信息来生成代码、Html文档或者做其它相应处理。 
- 运行时的处理： 某些注解可以在程序运行的时候接受代码的提取。
注解主要针对的是编译器和其它工具软件(SoftWare tool):当开发者使用了Annotation 修饰了类、方法、Field 等成员之后，这些 Annotation 不会自己生效，必须由开发者提供相应的代码来提取并处理 Annotation 信息。这些处理提取和处理 Annotation 的代码统称为 APT（Annotation Processing Tool)。
用途之一：[通过注解我完成对别人的代码进行测试]https://blog.csdn.net/briblue/article/details/73824058
