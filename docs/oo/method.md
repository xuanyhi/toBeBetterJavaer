---
title: Java中的方法：实例方法、静态方法、抽象方法
shortTitle: Java 中的方法
description: Java程序员进阶之路，小白的零基础Java教程，Java中的方法：实例方法、静态方法、抽象方法
category:
  - Java 核心
tag:
  - 面向对象编程
head:
  - - meta
    - name: keywords
      content: Java,Java SE,Java基础,Java教程,Java程序员进阶之路,Java入门,教程,方法,实例方法,静态方法,抽象方法,java方法
---

“二哥，这一节我们学什么呢？”三妹满是期待的问我。

“这一节我们来了解一下 Java 中的方法——什么是方法？如何声明方法？方法有哪几种？什么是实例方法？什么是静态方法？什么是抽象方法？”我笑着对三妹说，“我开始了啊，你要注意力集中啊。”

## 01、Java 中的方法是什么？

方法用来实现代码的可重用性，我们编写一次方法，并多次使用它。通过增加或者删除方法中的一部分代码，就可以提高整体代码的可读性。

只有方法被调用时，它才会执行。Java 中最有名的方法当属 `main()` 方法，这是程序的入口。

## 02、如何声明方法？

方法的声明反映了方法的一些信息，比如说可见性、返回类型、方法名和参数。如下图所示。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/object-class/17-01.png)

**访问权限**：它指定了方法的可见性。Java 提供了四种访问权限修饰符：

- public：该方法可以被所有类访问。
- private：该方法只能在定义它的类中访问。
- protected：该方法可以被同一个包中的类，或者不同包中的子类访问。
- default：该方法如果没有使用任何访问权限修饰符，Java 默认它使用 default 修饰符，该方法只能被同一个包中类可见。

**返回类型**：方法返回的数据类型，可以是基本数据类型、对象和集合，如果不需要返回数据，则使用 void 关键字。

**方法名**：方法名最好反应出方法的功能，比如，我们要创建一个将两个数字相减的方法，那么方法名最好是 subtract。

方法名最好是一个动词，并且以小写字母开头。如果方法名包含两个以上单词，那么第一个单词最好是动词，然后是形容词或者名词，并且要以驼峰式的命名方式命名。比如：

- 一个单词的方法名：`sum()`
- 多个单词的方法名：`stringComparision()`

一个方法可能与同一个类中的另外一个方法同名，这被称为方法重载。

**参数**：参数被放在一个圆括号内，如果有多个参数，可以使用逗号隔开。参数包含两个部分，参数类型和参数名。如果方法没有参数，圆括号是空的。

**方法签名**：每一个方法都有一个签名，包括方法名和参数。

**方法体**：方法体放在一对花括号内，把一些代码放在一起，用来执行特定的任务。

## 03、方法有哪几种？

方法可以分为两种，一种叫预先定义方法，一种叫用户自定义方法。

### **1）预先定义方法**

Java 提供了大量预先定义好的方法供我们调用，也称为标准类库方法，或者内置方法。比如说 String 类的 `length()`、`equals()`、`compare()` 方法，以及我们在初学 Java 阶段最常用的 `println()` 方法，用来在控制台打印信息。

```java
/**
 * @author 微信搜「沉默王二」，回复关键字 PDF
 */
public class PredefinedMethodDemo {
    public static void main(String[] args) {
        System.out.println("沉默王二，一枚有趣的程序员");
    }
}
```

在上面的代码中，我们使用了两个预先定义的方法，`main()` 方法是程序运行的入口，`println()` 方法是 `PrintStream` 类的一个方法。这些方法已经提前定义好了，所以我们可以直接使用它们。

我们可以通过集成开发工具查看预先定义方法的方法签名，当我们把鼠标停留在 `println()` 方法上面时，就会显示下图中的内容：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/object-class/17-02.png)

`println()` 方法的访问权限修饰符是 public，返回类型为 void，方法名为 println，参数为 `String x`，以及 Javadoc（方法是干嘛的）。

预先定义方法让编程变得简单了起来，我们只需要在实现某些功能的时候直接调用这些方法即可，不需要重新编写。

### **2）用户自定义方法**

当预先定义方法无法满足我们的要求时，就需要自定义一些方法，比如说，我们来定义这样一个方法，用来检查数字是偶数还是奇数。

```java
public static void findEvenOdd(int num) {
    if (num % 2 == 0) {
        System.out.println(num + " 是偶数");
    } else {
        System.out.println(num + " 是奇数");
    }
}
```

方法名叫做 `findEvenOdd`，访问权限修饰符是 public，并且是静态的（static），返回类型是 void，参数有一个整型（int）的 num。方法体中有一个 if else 语句，如果 num 可以被 2 整除，那么就打印这个数字是偶数，否则就打印这个数字是奇数。

方法被定义好后，如何被调用呢？

```java
/**
 * @author 微信搜「沉默王二」，回复关键字 PDF
 */
public class EvenOddDemo {
    public static void main(String[] args) {
        findEvenOdd(10);
        findEvenOdd(11);
    }

    public static void findEvenOdd(int num) {
        if (num % 2 == 0) {
            System.out.println(num + " 是偶数");
        } else {
            System.out.println(num + " 是奇数");
        }
    }
}
```

`main()` 方法是程序的入口，并且是静态的，那么就可以直接调用同样是静态方法的 `findEvenOdd()`。

当一个方法被 static 关键字修饰时，它就是一个静态方法。换句话说，静态方法是属于类的，不属于类实例的（不需要通过 new 关键字创建对象来调用，直接通过类名就可以调用）。

## 04、什么是实例方法？

没有使用 [static 关键字](https://tobebetterjavaer.com/oo/static.html)修饰，但在类中声明的方法被称为实例方法，在调用实例方法之前，必须创建类的对象。

```java
/**
 * @author 微信搜「沉默王二」，回复关键字 PDF
 */
public class InstanceMethodExample {
    public static void main(String[] args) {
        InstanceMethodExample instanceMethodExample = new InstanceMethodExample();
        System.out.println(instanceMethodExample.add(1, 2));
    }

    public int add(int a, int b) {
        return a + b;
    }
}
```

`add()` 方法是一个实例方法，需要创建 InstanceMethodExample 对象来访问。

实例方法有两种特殊类型：

- getter 方法
- setter 方法

getter 方法用来获取私有变量（private 修饰的字段）的值，setter 方法用来设置私有变量的值。

```java
/**
 * @author 沉默王二，一枚有趣的程序员
 */
public class Person {
    private String name;
    private int age;
    private int sex;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getSex() {
        return sex;
    }

    public void setSex(int sex) {
        this.sex = sex;
    }
}
```

getter 方法以 get 开头，setter 方法以 set 开头。

## 05、什么是静态方法？

相应的，有 [static 关键字](https://tobebetterjavaer.com/oo/static.html)修饰的方法就叫做静态方法。

```java
/**
 * 微信搜索「沉默王二」，回复 Java
 *
 * @author 沉默王二
 * @date 8/9/22
 */
public class StaticMethodExample {
    public static void main(String[] args) {
        System.out.println(add(1,2));
    }

    public static int add(int a, int b) {
        return a + b;
    }
}
```

StaticMethodExample 类中，mian 和 add 方法都是静态方法，不同的是，main 方法是程序的入口。当我们调用静态方法的时候，就不需要 new 出来类的对象，就可以直接调用静态方法了，一些工具类的方法都是静态方法，比如说 hutool 工具类库，里面有大量的静态方法可以直接调用。

> Hutool 的目标是使用一个工具方法代替一段复杂代码，从而最大限度的避免“复制粘贴”代码的问题，彻底改变我们写代码的方式。

以计算 MD5 为例：

- 👴【以前】打开搜索引擎 -> 搜“Java MD5 加密” -> 打开某篇博客-> 复制粘贴 -> 改改好用
- 👦【现在】引入 Hutool -> SecureUtil.md5()

Hutool 的存在就是为了减少代码搜索成本，避免网络上参差不齐的代码出现导致的 bug。

## 06、什么是抽象方法？

没有方法体的方法被称为抽象方法，它总是在抽象类中声明。这意味着如果类有抽象方法的话，这个类就必须是抽象的。可以使用 atstract 关键字创建抽象方法和抽象类。

```java
/**
 * @author 微信搜「沉默王二」，回复关键字 PDF
 */
abstract class AbstractDemo {
    abstract void display();
}
```

当一个类继承了抽象类后，就必须重写抽象方法：

```java
/**
 * @author 微信搜「沉默王二」，回复关键字 PDF
 */
public class MyAbstractDemo extends AbstractDemo {
    @Override
    void display() {
        System.out.println("重写了抽象方法");
    }

    public static void main(String[] args) {
        MyAbstractDemo myAbstractDemo = new MyAbstractDemo();
        myAbstractDemo.display();
    }
}
```

输出结果如下所示：

```
重写了抽象方法
```

## 07、什么是本地 native 方法？

类似 Thread 类中的 `private native start0()` 方法；

又或者 Object.class 类中的 getClass() 方法、hashCode()方法、clone() 方法，其中方法签名如下：

```java
public final native Class<?> getClass();
public native int hashCode();
protected native Object clone() throws CloneNotSupportedException;
```

也就是用【native】关键词修饰的方法，多数情况下不需要用 Java 语言实现。

“二哥，为什么要用 native 来修饰方法呢，这样做有什么用？”三妹很乖，但这个问题也问的很掷地有声。

“好的，三妹，我们一步步来扒拉”。

### **1、JNI：Java Native Interface**

在介绍 native 之前，我们先了解什么是 JNI。

一般情况下，我们完全可以使用 Java 语言编写程序，但某些情况下，Java 可能满足不了需求，或者不能更好的满足需求，比如：

- ①、标准的 Java 类库不支持。
- ②、我们已经用另一种语言，比如说 C/C++ 编写了一个类库，如何用 Java 代码调用呢？
- ③、某些运行次数特别多的方法，为了加快性能，需要用更接近硬件的语言（比如汇编）编写。

上面这三种需求，说到底就是如何用 Java 代码调用不同语言编写的代码。那么 JNI 应运而生了。

从 Java 1.1 开始，Java Native Interface (JNI)标准就成为 Java 平台的一部分，它允许 Java 代码和其他语言编写的代码进行交互。

JNI 一开始是为了本地已编译语言，尤其是 C 和 C++而设计的，但是它并不妨碍你使用其他语言，只要调用约定受支持就可以了。使用 Java 与本地已编译的代码交互，通常会丧失平台可移植性，但是，有些情况下这样做是可以接受的，甚至是必须的，比如，使用一些旧的库，与硬件、操作系统进行交互，或者为了提高程序的性能。JNI 标准至少保证本地代码能工作能在任何 Java 虚拟机实现下。

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-67e26e52-bf45-4126-a516-1e768632aaa8.jpg)

通过 JNI，我们就可以通过 Java 程序（代码）调用到操作系统相关的技术实现的库函数，从而与其他技术和系统交互；同时其他技术和系统也可以通过 JNI 提供的相应原生接口调用 Java 应用系统内部实现的功能。

在 Windows 上，一般可执行的应用程序都是基于 native 的 PE 结构，Windows 上的 JVM 也是基于 native 结构实现的。Java 应用体系都是构建于 JVM 之上。

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-61f0597a-b164-4128-a55d-1ee65189eae1.jpg)

“二哥，等一下，Java 不是跨平台的吗？如果用 JNI，那么程序不就失去了跨平台的优点？”不得不说，三妹这个问题起到好处。

“确实是这样的。”我掐灭了中指和无名指之间的烟头，继续娓娓道来。

JNI 的缺点：

- ①、程序不再跨平台。要想跨平台，必须在不同的系统环境下重新编译本地语言部分。
- ②、程序不再是绝对安全的，本地代码的不当使用可能导致整个程序崩溃。一个通用规则是，你应该让本地方法集中在少数几个类当中。这样就降低了 Java 和 C/C++ 之间的耦合性。

目前来讲使用 JNI 的缺点相对于优点还是可以接受的，可能后面随着 Java 的技术发展，我们不在需要 JNI，但是目前 JDK 还是一直提供了对 JNI 标准的支持。

### **3、用 C 语言编写程序本地方法**

“上面讲解了什么是 JNI，接下来我们来写个例子：如何用 Java 代码调用本地的 C 程序。”我扭头对三妹说，“你注意📢看。”

>官方文档如下：[https://docs.oracle.com/javase/8/docs/technotes/guides/jni/spec/jniTOC.html](https://link.zhihu.com/?target=https%3A//docs.oracle.com/javase/8/docs/technotes/guides/jni/spec/jniTOC.html)

步骤如下： 　　 

①、编写带有 native 方法的 Java 类，生成.java 文件；

②、使用 javac 命令编译所编写的 Java 类，生成.class 文件；

③、使用 javah -jni java 类名 生成扩展名为 h 的头文件，也即生成 .h 文件；

④、使用 C/C++（或者其他编程想语言）实现本地方法，创建 .h 文件的实现，也就是创建 .cpp 文件实现.h 文件中的方法；

⑤、将 C/C++ 编写的文件生成动态连接库，生成 dll 文件；

下面我们通过一个 HelloWorld 程序的调用来完成这几个步骤。

>注意：下面所有操作都是在所有操作都是在目录：D:\\JNI 下进行的。

#### 一、编写带有 native 声明的方法的 java 类

```text
public class HelloJNI {
    //native 关键字告诉 JVM 调用的是该方法在外部定义
    private native void helloJNI();

    static{
        System.loadLibrary("helloJNI");//载入本地库
    }
    public static void main(String[] args) {
        HelloJNI jni = new HelloJNI();
        jni.helloJNI();
    }

}
```

用 native 声明的方法告知 JVM 调用该方法在外部定义，也就是我们会用 C 语言去实现。

`System.loadLibrary("helloJNI");`加载动态库，参数 helloJNI 是动态库的名字。我们可以这样理解：程序中的方法 helloJNI() 在程序中没有实现，但是我们下面要调用这个方法，怎么办呢？我们就需要对这个方法进行初始化，所以用了 [static 代码块进行初始化](https://tobebetterjavaer.com/oo/static.html)。

这时候如果我们直接运行该程序，会报“A Java Exception has occurred”错误：

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-b5a7e2d8-5bae-45ae-8225-6c21090b506a.jpg)

#### 二、使用 javac 命令编译 java 类，生成.class 文件

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-1131ccf8-e4d0-4e4c-b7f6-659f0906c5d4.jpg)

执行上述命令后，生成 HelloJNI.class 文件：

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-b015e4ae-d636-42ef-93e7-381e33587ea9.jpg)

#### 三、使用 javah -jni java 类名 生成扩展名为 h 的头文件

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-0f2755b6-45ad-44d6-88ff-2eb3fa5cba3b.jpg)

执行上述命令后，在 D:/JNI 目录下会多出一个 HelloJNI.h 文件：

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-7798a73c-1fdf-4f08-9d29-9ee5dfaa8a1b.jpg)

#### 四、使用 C 语言实现本地方法 　　如果不想安装 Visual Studio，需要在 Windows 平台安装 gcc。

安装教程如下：[http://blog.csdn.net/altland/article/details/63252757](https://blog.csdn.net/altland/article/details/63252757)

注意安装版本的选择，根据系统是 32 位还是 64 位来选择。

安装完成之后注意配置环境变量，在 cmd 中输入 `g++ -v`，如果出现如下信息，则安装配置完成：

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-43b26326-e461-4b55-b029-d8b8e745ecbe.jpg)

接着输入如下命令：

```
gcc -m64  -Wl,--add-stdcall-alias -I"C:\Program Files\Java\jdk1.8.0_152\include" -I"C:\Program Files\Java\jdk1.8.0_152\include\include\win32" -shared -o helloJNI.dll helloJNI.c
```

\-m64 表示生成 dll 库是 64 位的。后面的路径表示本机安装的 JDK 路径。生成之后多了一个 helloJNI.dll 文件

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-07eabf6b-166a-4537-9521-8781887e4ad7.jpg)

最后运行 HelloJNI：输出 Hello JNI! 大功告成。

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-541e9154-1643-42f7-9baf-357405466711.jpg)

### **4、JNI 调用 C 的流程图**

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/oo/method-a2103240-def4-460f-a12c-8f74e08e2b1b.jpg)


### **5、native 关键字**

“三妹，现在应该知道什么是 native 了吧？”我问三妹。

“嗯嗯，我来简述一下，二哥你看看我说的是否正确。”

native 用来修饰方法，用 native 声明的方法表示该方法的实现在外部定义，可以用任何语言去实现它，比如说 C/C++。 简单地讲，一个 native Method 就是一个 Java 调用非 Java 代码的接口。

native 语法：

- ①、修饰方法的位置必须在返回类型之前，和其余的方法控制符前后关系不受限制。
- ②、不能用 abstract 修饰，也没有方法体，也没有左右大括号。
- ③、返回值可以是任意类型

“三妹，你学的不错嘛。”我对三妹的学习能力感到非常的欣慰，“**我们在日常编程中看到 native 修饰的方法，只需要知道这个方法的作用是什么，至于别的就不用管了，操作系统会给我们实现**。”


>参考链接：[https://www.zhihu.com/question/28001771/answer/2049534464](https://www.zhihu.com/question/28001771/answer/2049534464)

---

最近整理了一份牛逼的学习资料，包括但不限于 Java 基础部分（JVM、Java 集合框架、多线程），还囊括了 **数据库、计算机网络、算法与数据结构、设计模式、框架类 Spring、Netty、微服务（Dubbo，消息队列） 网关** 等等等等……详情戳：[可以说是 2022 年全网最全的学习和找工作的 PDF 资源了](https://tobebetterjavaer.com/pdf/programmer-111.html)

微信搜 **沉默王二** 或扫描下方二维码关注二哥的原创公众号沉默王二，回复 **111** 即可免费领取。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/gongzhonghao.png)
