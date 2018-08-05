---
title: Java基础问答（持续更新）
date: 2018-07-31 13:47:56
tags: "Java"
categories: JAVA
---
###### 10.问：接口和抽象类的区别，如何选择？
解： 

接口和抽象类，最明显的区别就是接口只是定义了一些方法而已，在不考虑Java8中default方法情况下，接口中是没有实现的代码的。 

抽象类中的抽象方法可以有public、protected和default这些修饰符，而接口中默认修饰符是public。不可以使用其它修饰符。 

关于如何选择，我们一般会把接口暴露给外部，然后在业务代码中实现接口。如果多个实现类中有相同可复用的代码，则在接口和实现类中间加一层抽象类，将公用部分代码抽出到抽象类中。可以参考下模板方法模式，这是一个很好的理解接口、抽象类和实现类之间关系的设计模式。

---

###### 9.问：什么是方法重写，什么是方法重载，成员变量可以被重写吗？

解： 

重载（Overloading）和重写（Overriding）是Java中两个比较重要的概念。但是对于新手来说也比较容易混淆。 

重载 

简单说，就是函数或者方法有同样的名称，但是参数列表不相同的情形，这样的同名不同参数的函数或者方法之间，互相称之为重载函数或者方法。 

重写 重写指的是在Java的子类与父类中有两个名称、参数列表都相同的方法的情况。由于他们具有相同的方法签名，所以子类中的新方法将覆盖父类中原有的方法。 

关于重载和重写，你应该知道以下几点： 
1. 重载是一个编译期概念、重写是一个运行期间概念。
2. 重载遵循所谓“编译期绑定”，即在编译时根据参数变量的类型判断应该调用哪个方法。
3. 重写遵循所谓“运行期绑定”，即在运行的时候，根据引用变量所指向的实际对象的类型来调用方法。
4. 因为在编译期已经确定调用哪个方法，所以重载并不是多态。而重写是多态。重载只是一种语言特性，是一种语法规则，与多态无关，与面向对象也无关。（注：严格来说，重载是编译时多态，即静态多态。但是，Java中提到的多态，在不特别说明的情况下都指动态多态） 

正式因为Java在继承中有方法的重写，所以，这也体现了Java的动态多态性。

重写，指的是方法。并没有提到成员变量。成员变量不会被重写，这里就有另外一个词：隐藏。

在一个类中，子类中的成员变量如果和父类中的成员变量同名，那么即使他们类型不一样，只要名字一样。父类中的成员变量都会被隐藏。在子类中，父类的成员变量不能被简单的用引用来访问。而是，必须从父类的引用获得父类被隐藏的成员变量，一般来说，我们不推荐隐藏成员变量，因为这样会使代码变得难以阅读。 

其实，简单来说，就是子类不会去重写覆盖父类的成员变量，所以成员变量的访问不能像方法一样使用多态去访问。

---

###### 8.问：什么是多态，多态有什么好处，多态的必要条件是什么、Java中多态的实现方式
解： 

大家说的都有道理，说说我的理解。

多态的概念呢比较简单，就是同一操作作用于不同的对象，可以有不同的解释，产生不同的执行结果。 

如果按照这个概念来定义的话，那么多态应该是一种运行期的状态。 为了实现运行期的多态，或者说是动态绑定，需要满足三个条件。 即有类继承或者接口实现、子类要重写父类的方法、父类的引用指向子类的对象。 简单来一段代码解释下：

```
public class Parent{ 
    public void call(){ 
        sout("im Parent"); 
    } 
} 

// 1.有类继承或者接口实现 
public class Son extends Parent{
    // 2.子类要重写父类的方法 
    public void call(){
        sout("im Son"); 
    } 
} 

// 1.有类继承或者接口实现
public class Daughter extends Parent{
    // 2.子类要重写父类的方法 
    public void call(){
        sout("im Daughter"); 
    } 
}

public class Test{ 
    public static void main(String[] args){ 
        //3.父类的引用指向子类的对象 
        Parent p = new Son(); 
        //3.父类的引用指向子类的对象 
        Parent p1 = new Daughter(); 
    }
}
```

这样，就实现了多态，同样是Parent类的实例，p.call调用的是Son类的实现、p1.call调用的是Daughter的实现。 有人说，你自己定义的时候不就已经知道p是son，p1是Daughter了么。但是，有些时候你用到的对象并不都是自己声明的啊 。 比如Spring 中的IOC出来的对象，你在使用的时候就不知道他是谁，或者说你可以不用关心他是谁。根据具体情况而定。

另外，还有一种说法，包括维基百科也说明，动态还分为动态多态和静态多态。上面提到的那种动态绑定认为是动态多态，因为只有在运行期才能知道真正调用的是哪个类的方法。

还有一种静态多态，一般认为Java中的函数重载是一种静态多态，因为他需要在编译期决定具体调用哪个方法。

关于这个动态静态的说法，我更偏向于重载和多态其实是无关的。

但是也要看情况，普通场合，我会认为只有方法的重写算是多态，毕竟这是我的观点。但是如果在面试的时候，我“可能”会认为重载也算是多态，毕竟面试官也有他的观点。我会和面试官说：我认为，多态应该是一种运行期特性，Java中的重写是多态的体现。不过也有人提出重载是一种静态多态的想法，这个问题在StackOverflow等网站上有很多人讨论，但是并没有什么定论。我更加倾向于重载不是多态。

这样沟通，既能体现出你了解的多，又能表现出你有自己的思维，不是那种别人说什么就是什么的。

---

###### 7.问：Java 8中的lambda表达式是不是语法糖，具体如何实现的。

解： 

关于lambda表达式，网上有人说他并不是语法糖，也有人说他是匿名内部类的语法糖。其实我想纠正下这个说法。Labmda表达式不是匿名内部类的语法糖，但是他也是一个语法糖。实现方式其实是依赖了几个JVM底层提供的lambda相关api。

先来看一个简单的lambda表达式。遍历一个list：

```
public static void main(String... args) {
    List<String> strList = ImmutableList.of("ccr", "chengchenrui", "博客：chengchenrui.com");
    strList.forEach( s -> { System.out.println(s); } ); 
}
```

为啥说他并不是内部类的语法糖呢，#直面Java#的第006期介绍过的，内部类在编译之后会有两个class文件，但是，包含lambda表达式的类编译后只有一个文件。

反编译后代码如下:

```
public static /* varargs */ void main(String ... args) { 
    ImmutableList strList = ImmutableList.of((Object)"ccr", (Object)"chengchenrui", (Object)"\u535a\u5ba2\uff1achengchenrui.com"); strList.forEach((Consumer<String>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)V, lambda$main$0(java.lang.String ), (Ljava/lang/String;)V)()); 
} 
private static /* synthetic */ void lambda$main$0(String s) { 
    System.out.println(s); 
}
```

可以看到，在forEach方法中，其实是调用了java.lang.invoke.LambdaMetafactory#metafactory方法，该方法的第四个参数implMethod指定了方法实现。可以看到这里其实是调用了一个lambda$main$0方法进行了输出。

再来看一个稍微复杂一点的，先对List进行过滤，然后再输出：

```
public static void main(String... args) { 
    List<String> strList = ImmutableList.of("ccr", "chengchenrui", "博客：chengchenrui.com");
    List HollisList = strList.stream().filter(string -> string.contains("ccr")).collect(Collectors.toList()); 
    HollisList.forEach( s -> { System.out.println(s); } ); 
}
```

反编译后代码如下：

```
public static /* varargs */ void main(String ... args) { 
    ImmutableList strList = ImmutableList.of((Object)"ccr", (Object)"chengchenrui", (Object)"\u535a\u5ba2\uff1achengchenrui.com"); 
    List<Object> HollisList = strList.stream().filter((Predicate<String>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)Z, lambda$main$0(java.lang.String ), (Ljava/lang/String;)Z)()).collect(Collectors.toList()); HollisList.forEach((Consumer<Object>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)V, lambda$main$1(java.lang.Object ), (Ljava/lang/Object;)V)()); 
} 
    
private static /* synthetic */ void lambda$main$1(Object s) { 
    System.out.println(s); 
} 

private static /* synthetic */ boolean lambda$main$0(String string) { 
    return string.contains("Hollis"); 
}
```

两个lambda表达式分别调用了`lambda$main$1`和`lambda$main$0`两个方法。 

所以，lambda表达式的实现其实是依赖了一些底层的api，在编译阶段，编译器会把lambda表达式进行解糖，转换成调用内部api的方式。

---

###### 6.问：什么是Java的语法糖，列举你个知道的语法糖。如何解语法糖？ 
解： 

语法糖 

语法糖（Syntactic Sugar），也称糖衣语法，是由英国计算机学家 Peter.J.Landin 发明的一个术语，指在计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用。简而言之，语法糖让程序更加简洁，有更高的可读性。 

解语法糖

前面提到过，语法糖的存在主要是方便开发人员使用。但其实，Java虚拟机并不支持这些语法糖，这些语法糖在编译阶段就会被还原成简单的基础语法结构，这个过程就是解语法糖。 

说到编译，大家肯定都知道，Java语言中，javac命令可以将后缀名为.java的源文件编译为后缀名为.class的可以运行于Java虚拟机的字节码。如果你去看com.sun.tools.javac.main.JavaCompiler的源码，你会发现在compile()中有一个步骤就是调用desugar()，这个方法就是负责解语法糖的实现的。 

Java 中最常用的语法糖主要有泛型、变长参数、条件编译、自动拆装箱、内部类等。本文主要来分析下这些语法糖背后的原理，一步一步剥去糖衣，看看其本质。 

糖块一、 switch 支持 String 与枚举 前面提到过，从Java 7 开始，Java语言中的语法糖在逐渐丰富，其中一个比较重要的就是Java 7中switch开始支持String。

在开始coding之前先科普下，Java中的swith自身原本就支持基本类型。比如int、char等。对于int类型，直接进行数值的比较。对于char类型则是比较其ascii码。所以，对于编译器来说，switch中其实只能使用整型，任何类型的比较都要转换成整型。比如byte、short、char（ackii码是整型）以及int。 

那么接下来看下switch对String得支持，有以下代码： 
```
public class switchDemoString { 
    public static void main(String[] args) { 
        String str = "world"; 
        switch (str) { 
        case "hello": 
            System.out.println("hello"); 
            break;
        default: 
            break; 
        } 
    } 
}
```


反编译后内容如下： 

```
public class switchDemoString { 
    public switchDemoString() { } 
    public static void main(String args[]) { 
        String str = "world"; 
        String s; 
        switch((s = str).hashCode()) { 
        default: 
            break; 
        case 99162322: 
            if(s.equals("hello")) 
                System.out.println("hello"); 
            break; 
        } 
    } 
}
```


看到这个代码，你知道原来字符串的switch是通过equals()和hashCode()方法来实现的。还好hashCode()方法返回的是int，而不是long。 

仔细看下可以发现，进行switch的实际是哈希值，然后通过使用equals方法比较进行安全检查，这个检查是必要的，因为哈希可能会发生碰撞。因此它的性能是不如使用枚举进行switch或者使用纯整数常量，但这也不是很差。

糖块二、 泛型 

我们都知道，很多语言都是支持泛型的，但是很多人不知道的是，不同的编译器对于泛型的处理方式是不同的，通常情况下，一个编译器处理泛型有两种方式：Code specialization和Code sharing。C++和C#是使用Code specialization的处理机制，而Java使用的是Code sharing的机制。 

Code sharing方式为每个泛型类型创建唯一的字节码表示，并且将该泛型类型的实例都映射到这个唯一的字节码表示上。将多种泛型类形实例映射到唯一的字节码表示是通过类型擦除（type erasue）实现的。

也就是说，对于Java虚拟机来说，他根本不认识Map<String, String> map这样的语法。需要在编译阶段通过类型擦除的方式进行解语法糖。 

类型擦除的主要过程如下： 

将所有的泛型参数用其最左边界（最顶级的父类型）类型替换。 
移除所有的类型参数。 
以下代码： 

```
Map<String, String> map = new HashMap<String, String>(); 
map.put("name", "hollis");
```
 解语法糖之后会变成： 
 
```
Map map = new HashMap(); 
map.put("name", "hollis");
```
 虚拟机中没有泛型，只有普通类和普通方法，所有泛型类的类型参数在编译时都会被擦除，泛型类并没有自己独有的Class类对象。比如并不存在List<String>.class或是List<Integer>.class，而只有List.class。 
 
 糖块三、 自动装箱与拆箱 
 
 自动装箱就是Java自动将原始类型值转换成对应的对象，比如将int的变量转换成Integer对象，这个过程叫做装箱，反之将Integer对象转换成int类型值，这个过程叫做拆箱。 
 
 先来看个自动装箱的代码：

```
public static void main(String[] args) { 
    int i = 10; Integer n = i; 
}
```
 反编译后代码如下: 

```
public static void main(String args[]) { 
    int i = 10; Integer n = Integer.valueOf(i); 
}
```

 从反编译得到内容可以看出，在装箱的时候自动调用的是Integer的valueOf(int)方法。而在拆箱的时候自动调用的是Integer的intValue方法。
 
 所以，装箱过程是通过调用包装器的valueOf方法实现的，而拆箱过程是通过调用包装器的 xxxValue方法实现的。 
 
 糖块四 、 方法变长参数 
 
 可变参数(variable arguments)是在Java 1.5中引入的一个特性，它允许一个方法把任意数量的值作为参数。 
 
 看下以下可变参数代码，其中print方法接收可变参数： 
 
```
public static void main(String[] args) {
    print("Holis", "公众号:Hollis", "博客：www.hollischuang.com"); 
    
} 
public static void print(String... strs) { 
    for (int i = 0; i < strs.length; i++) { 
        System.out.println(strs[i]); 
    }
}
```

 反编译后代码： 
 
```
public static void main(String args[]) { 
    print(new String[] { "Holis", "\u516C\u4F17\u53F7:Hollis", "\u535A\u5BA2\uFF1Awww.hollischuang.com" }); 
} 
public static transient void print(String strs[]) { 
    for(int i = 0; i < strs.length; i++) 
        System.out.println(strs[i]); 
    
}
```

 从反编译后代码可以看出，可变参数在被使用的时候，他首先会创建一个数组，数组的长度就是调用该方法是传递的实参的个数，然后再把参数值全部放到这个数组当中，然后再把这个数组作为参数传递到被调用的方法中。
 
 糖块五 、 枚举 
 
 Java SE5提供了一种新的类型-Java的枚举类型，关键字enum可以将一组具名的值的有限集合创建为一种新的类型，而这些具名的值可以作为常规的程序组件使用，这是一种非常有用的功能。 
 
 要想看源码，首先得有一个类吧，那么枚举类型到底是什么类呢？是enum吗？答案很明显不是，enum就和class一样，只是一个关键字，他并不是一个类，那么枚举是由什么类维护的呢，我们简单的写一个枚举： 
 
```
public enum t { 
    SPRING,SUMMER; 
}
```
 
 然后我们使用反编译，看看这段代码到底是怎么实现。
 
 通过反编译后代码我们可以看到（内容太多不贴了，读者自行反编译了解下），public final class T extends Enum，说明，该类是继承了Enum类的，同时final关键字告诉我们，这个类也是不能被继承的。当我们使用enmu来定义一个枚举类型的时候，编译器会自动帮我们创建一个final类型的类继承Enum类，所以枚举类型不能被继承。 
 
 糖块六 、 内部类 
 
 内部类又称为嵌套类，可以把内部类理解为外部类的一个普通成员。
 
内部类之所以也是语法糖，是因为它仅仅是一个编译时的概念，outer.java里面定义了一个内部类inner，一旦编译成功，就会生成两个完全不同的.class文件了，分别是outer.class和outer$inner.class。所以内部类的名字完全可以和它的外部类名字相同。 
 
 糖块七 、条件编译 
 
—般情况下，程序中的每一行代码都要参加编译。但有时候出于对程序代码优化的考虑，希望只对其中一部分内容进行编译，此时就需要在程序中加上条件，让编译器只对满足条件的代码进行编译，将不满足条件的代码舍弃，这就是条件编译。 
 
如在C或CPP中，可以通过预处理语句来实现条件编译。其实在Java中也可实现条件编译。我们先来看一段代码：

```
public class ConditionalCompilation { 
    public static void main(String[] args) { 
        final boolean DEBUG = true; 
        if(DEBUG) { 
            System.out.println("Hello, DEBUG!"); 
        } 
        final boolean ONLINE = false; 
        if(ONLINE){ 
            System.out.println("Hello, ONLINE!"); 
        } 
    } 
}
```
 反编译后代码如下： 

```
public class ConditionalCompilation { 
    public static void main(String args[]) { 
        boolean DEBUG = true; 
        System.out.println("Hello, DEBUG!"); 
        boolean ONLINE = false; 
    } 
}
```
首先，我们发现，在反编译后的代码中没有System.out.println("Hello, ONLINE!");，这其实就是条件编译。当if(ONLINE)为false的时候，编译器就没有对其内的代码进行编译。

所以，Java语法的条件编译，是通过判断条件为常量的if语句实现的。其原理也是Java语言的语法糖。根据if判断条件的真假，编译器直接把分支为false的代码块消除。通过该方式实现的条件编译，必须在方法体内实现，而无法在正整个Java类的结构或者类的属性上进行条件编译，这与C/C++的条件编译相比，确实更有局限性。在Java语言设计之初并没有引入条件编译的功能，虽有局限，但是总比没有更强。 
 
 糖块八 、 断言 
 
在Java中，assert关键字是从 Java SE 1.4 引入的，为了避免和老版本的Java代码中使用了assert关键字导致错误，Java在执行的时候默认是不启动断言检查的（这个时候，所有的断言语句都将忽略！），如果要开启断言检查，则需要用开关-enableassertions或-ea来开启。 

看一段包含断言的代码： 

```
public class AssertTest { 
    public static void main(String args[]) { 
        int a = 1; 
        int b = 1; 
        assert a == b; 
        System.out.println("ccr"); 
        assert a != b : "chengchenrui"; 
        System.out.println("chengchenrui.com"); 
    } 
}
```
 反编译后代码太多，不贴了，但是很明显，反编译之后的代码要比我们自己的代码复杂的多。所以，使用了assert这个语法糖我们节省了很多代码。其实断言的底层实现就是if语言，如果断言结果为true，则什么都不做，程序继续执行，如果断言结果为false，则程序抛出AssertError来打断程序的执行。-enableassertions会设置$assertionsDisabled字段的值。 
 
 糖块九 、 数值字面量 
 
 在Java 7中，数值字面量，不管是整数还是浮点数，都允许在数字之间插入任意多个下划线。这些下划线不会对字面量的数值产生影响，目的就是方便阅读。 
 
 比如： 
 
```
public class Test { 
    public static void main(String... args) { 
    int i = 10_000; 
    System.out.println(i); 
    } 
}
```

 反编译后： 
 
```
public class Test { 
    public static void main(String[] args) { 
    int i = 10000; 
    System.out.println(i); 
    } 
}
```
 
 反编译后就是把_ 删除了。也就是说 编译器并不认识在数字字面量中的_，需要在编译阶段把他去掉。 
 
 糖块十 、 for-each 增强for循环（for-each）
 
 相信大家都不陌生，日常开发经常会用到的，他会比for循环要少写很多代码，那么这个语法糖背后是如何实现的呢？ 
 
```
public static void main(String... args) { 
    String[] strs = {"ccr", "chengchenrui", "chengchenrui.com"}; 
    for (String s : strs) { 
    System.out.println(s); 
    } 
}
```
 反编译后代码如下： 
```
public static transient void main(String args[]) { 
    String strs[] = { "ccr", "chengchenrui", "chengchenrui.com" }; 
    String args1[] = strs; 
    int i = args1.length; 
    for(int j = 0; j < i; j++) { 
        String s = args1[j]; 
        System.out.println(s); 
    } 
}
```
 代码很简单，for-each的实现原理其实就是使用了普通的for循环和迭代器。 
 
 糖块十一 、 try-with-resource 
 
 Java里，对于文件操作IO流、数据库连接等开销非常昂贵的资源，用完之后必须及时通过close方法将其关闭，否则资源会一直处于打开状态，可能会导致内存泄露等问题。
 
 关闭资源的常用方式就是在finally块里是释放，即调用close方法。比如，我们经常会写这样的代码：
 
```
public static void main(String[] args) { 
    BufferedReader br = null; 
    try { 
        String line; 
        br = new BufferedReader(new FileReader("d:\\hollischuang.xml")); 
        while ((line = br.readLine()) != null) { 
            System.out.println(line); 
        }
    } catch (IOException e) { 
        // handle exception 
    } finally { 
        try { 
            if (br != null) { 
                br.close(); 
            } 
        } catch (IOException ex) { 
            // handle exception 
        } 
    }
}
```
 
 从Java 7开始，jdk提供了一种更好的方式关闭资源，使用try-with-resources语句，改写一下上面的代码，效果如下： 

```
public static void main(String... args) { 
    try (BufferedReader br = new BufferedReader(new FileReader("d:\\ hollischuang.xml"))) { 
        String line; 
        while ((line = br.readLine()) != null) { 
            System.out.println(line); 
        } 
    } catch (IOException e) { 
        // handle exception 
    } 
}
```
 看，这简直是一大福音啊，虽然我之前一般使用IOUtils去关闭流，并不会使用在finally中写很多代码的方式，但是这种新的语法糖看上去好像优雅很多呢。看下他的背后： 
 
```
public static transient void main(String args[]) { 
    BufferedReader br; 
    Throwable throwable;
    br = new BufferedReader(new FileReader("d:\\ hollischuang.xml")); 
    throwable = null; 
    String line; 
    try { 
        while((line = br.readLine()) != null) 
            System.out.println(line); 
    } catch(Throwable throwable2) { 
        throwable = throwable2; throw throwable2; 
    } 
    if(br != null) 
        if(throwable != null) 
            try { 
                br.close(); 
                
            } catch(Throwable throwable1) { 
                throwable.addSuppressed(throwable1); 
          
            } 
        else 
            br.close();
        break MISSING_BLOCK_LABEL_113; 
        Exception exception; 
        exception; 
        if(br != null) 
            if(throwable != null) 
                try { 
                    br.close(); 
                } catch(Throwable throwable3) { 
                    throwable.addSuppressed(throwable3); 
                } 
            else 
                br.close(); 
        throw exception; 
        IOException ioexception; 
        ioexception; 
    } 
}
```

其实背后的原理也很简单，那些我们没有做的关闭资源的操作，编译器都帮我们做了。所以，再次印证了，语法糖的作用就是方便程序员的使用，但最终还是要转成编译器认识的语言。 

总结 

前面介绍了12种Java中常用的语法糖。所谓语法糖就是提供给开发人员便于开发的一种语法而已。但是这种语法只有开发人员认识。要想被执行，需要进行解糖，即转成JVM认识的语法。当我们把语法糖解糖之后，你就会发现其实我们日常使用的这些方便的语法，其实都是一些其他更简单的语法构成的。

有了这些语法糖，我们在日常开发的时候可以大大提升效率，但是同时也要避免过渡使用。使用之前最好了解下原理，避免掉坑。

---

###### 5.问： 什么是编译，什么是反编译。Java的编译和反编译方法。

解： 

在介绍编译和反编译之前，我们先来简单介绍下编程语言（Programming Language）。编程语言（Programming Language）分为低级语言（Low-level Language）和高级语言（High-level Language）。 

机器语言（Machine Language）和汇编语言（Assembly Language）属于低级语言，直接用计算机指令编写程序。 

而C、C++、Java、Python等属于高级语言，用语句（Statement）编写程序，语句是计算机指令的抽象表示。 

上面提到语言有两种，一种低级语言，一种高级语言。可以这样简单的理解：低级语言是计算机认识的语言、高级语言是程序员认识的语言。

那么如何从高级语言转换成低级语言呢？这个过程其实就是编译。将便于人编写、阅读、维护的高级计算机语言所写作的源代码程序，翻译为计算机能解读、运行的低阶机器语言的程序的过程就是编译。负责这一过程的处理的工具叫做编译器。

反编译的过程与编译刚好相反，就是将已编译好的编程语言还原到未编译的状态，也就是找出程序语言的源代码。就是将机器看得懂的语言转换成程序员可以看得懂的语言。Java语言中的反编译一般指将class文件转换成java文件。

Java语言中负责编译的编译器是一个命令：javac

javac是收录于JDK中的Java语言编译器。该工具可以将后缀名为.java的源文件编译为后缀名为.class的可以运行于Java虚拟机的字节码。

当我们写完一个HelloWorld.java文件后，我们可以使用javac HelloWorld.java命令来生成HelloWorld.class文件，这个class类型的文件是JVM可以识别的文件。通常我们认为这个过程叫做Java语言的编译。其实，class文件仍然不是机器能够识别的语言，因为机器只能识别机器语言，还需要JVM再将这种class文件类型字节码转换成机器可以识别的机器语言。 

Java中反编译工具有多个，如javap ，jad，CRF等。

---

###### 4.什么是值传递，什么是引用传递。为什么说Java中只有值传递。

[为什么说Java中只有值传递。(引用Hollis文章)](https://mp.weixin.qq.com/s/F7Niaa7nD1tLApCEGKAj4A)

---

###### 3.问：什么是平台无关性，Java是如何做到平台无关的？

解：

跨平台指的是一种语言在计算机上的运行不受平台的约束，一次编译，到处执行。 平台无关有两种：源代码级和目标代码级。 

我们常说的跨平台，或者平台无关，指的就是目标代码，或者说是软件交付件跨平台。

C和C++ 具有一定程度的源代码级平台无关，表明用C或C++ 写的应用程序不用修改只需重新编译就可以在不同平台上运行。但是，关键是要重新编译。可是，一般软件交付都是给你个成品，对于C或者C++开发出的软件，只能运行在某个平台的。没有源码，怎么编译。

Java编译出来的是字节码，去到哪个平台都能用，只要有那个平台的JDK就可以运行，所以，Java程序的最大优势就是平台无关。对于Java，交付的就是一堆jar包或者war包，只要系统上有个Java虚拟机，就可以直接运行，这不就是跨平台了么。 

我们使用的C、C++还有Java等高级语言，都需要最终被编译成机器语言，才能被计算机执行。 

C语言和C++语言的编译过程是把源代码编译生成机器语言。这样机器可以直接执行。但是不同系统对同一段“机器语言”的处理结果可能是不一样的，原因可能有很多，比如CPU的指令集不同的。C语言不能实现跨平台运行，就是因为它编译出来的文件的格式，只适用于某种cpu，其他cpu无法识别。 那么Java是如何实现跨平台的呢？ 我们编写的Java源码，编译后会生成一种 .class 文件，称为字节码文件。这种字节码文件需要经过JVM虚拟机，然后翻译成机器语言，才能被机器执行。 那么，由于我们可以在不同的操作系统上安装不同的JVM，而JVM封装了所有对于.class文件的处理，即JVM帮我们把字节码翻译成机器语言的过程中就已经充分考虑到对应平台的特性了。比如，一个.class文件，在不同机器上最终生成的机器语言可能是不同的，但是这种不同不需要我们关心，JVM会保证他可以正常运行，完整的表达正确的程序语义。

---

###### 2.Java和C++同为面向对象语言，Java和C++主要区别有哪些？双方个有哪些优缺点？

解： C++ 被设计成主要用在系统性应用程序设计上的语言，对C语言进行了扩展。对于C语言这个为运行效率设计的过程式程序设计语言, C++ 特别加上了以下这些特性的支持：静态类型的面向对象程序设计的支持、异常处理、RAII以及泛型。另外它还加上了一个包含泛型容器和算法的C++库函数。 

Java 最开始是被设计用来支持网络计算。它依赖一个虚拟机来保证安全和可移植性。Java包含一个可扩展的库用以提供一个完整的的下层平台的抽象。Java是一种静态面向对象语言，它使用的语法类似C++，但与之不兼容。为了使更多的人到使用更易用的语言，它进行了全新的设计。 

其实，最主要的区别是他们分别代表了两种类型的语言：

C++ 是编译型语言（首先将源代码编译生成机器语言，再由机器运行机器码），执行速度快、效率高；依赖编译器、跨平台性差些。 

Java是解释型语言（源代码不是直接翻译成机器语言，而是先翻译成中间代码，再由解释器对中间代码进行解释运行。），执行速度慢、效率低；依赖解释器、跨平台性好。

PS：也有人说Java是半编译、半解释型语言。Java 编译器(javac)先将java源程序编译成Java字节码(.class)，JVM负责解释执行字节码文件。

二者更多的主要区别如下： 

C++ 是平台相关的，Java是平台无关的。 C++ 对所有的数字类型有标准的范围限制，但字节长度是跟具体实现相关的，不同操作系统可能。Java在所有平台上对所有的基本类型都有标准的范围限制和字节长度。 C++ 除了一些比较少见的情况之外和C语言兼容 。 Java没有对任何之前的语言向前兼容。但在语法上受 C/C++ 的影响很大 C++ 允许直接调用本地的系统库 。 Java要通过JNI调用, 或者 JNA C++ 允许过程式程序设计和面向对象程序设计 。Java必须使用面向对象的程序设计方式 C++ 支持指针，引用，传值调用 。Java只有值传递。 C++ 需要显式的内存管理，但有第三方的框架可以提供垃圾搜集的支持。支持析构函数。 Java 是自动垃圾收集的。没有析构函数的概念。 C++ 支持多重继承，包括虚拟继承 。Java只允许单继承，需要多继承的情况要使用接口。

---

###### 1.知识点：Java是一种面向对象的语言。

思考题：什么是面向对象，什么是面向过程。面向对象的三大基本特征和五大基本原则是什么？

解：

什么是面向过程？ 

把问题分解成一个一个步骤，每个步骤用函数实现，依次调用即可。就是说，在进行面向过程编程的时候，不需要考虑那么多，上来先定义一个函数，然后使用各种诸如if-else、for-each等方式进行代码执行。最典型的用法就是实现一个简单的算法，比如实现冒泡排序。 

什么是面向对象？ 

将问题分解成一个一个步骤，对每个步骤进行相应的抽象，形成对象，通过不同对象之间的调用，组合解决问题。就是说，在进行面向对象进行编程的时候，要把属性、行为等封装成对象，然后基于这些对象及对象的能力进行业务逻辑的实现。比如想要造一辆车，上来要先把车的各种属性定义出来，然后抽象成一个Car类。 

面向对象的三大基本特征和五大基本原则？ 

三大基本特征：封装、继承、多态。 

五大基本原则：单一职责原则（Single-Responsibility Principle）、开放封闭原则（Open-Closed principle）、Liskov替换原则（Liskov-Substituion Principle）、依赖倒置原则（Dependency-Inversion Principle）和 接口隔离原则（Interface-Segregation Principle）。

后话： 

这些东西虽然都是概念性的，但是很多程序员根本不知道，更别提如何更好的面向对象编程了，我就看过有些同学写的代码，从头到尾一个函数，各种if-else，完全不考虑复用、扩展等。这种代码非常不利于阅读和维护。希望球友们不仅记住上面的概念，还要通汇贯通，切实的运用到平时的工作中。
