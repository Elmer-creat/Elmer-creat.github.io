# 这是guide页面

#### JVM vs JDK vs JRE

**JVM**

Java 虚拟机（Java Virtual Machine, JVM）是**运行 Java 字节码的虚拟机**。JVM 有针对不同系统的特定实现（Windows，Linux，macOS），**目的是使用相同的字节码，它们都会给出相同的结果**。字节码和不同系统的 JVM 实现是 Java 语言“一次编译，随处可以运行”的关键所在

![image.png](https://obsidian-sync-notes.oss-cn-beijing.aliyuncs.com/PicGo/202411222012799.png)
**JVM 并不是只有一种！只要满足 JVM 规范，每个公司、组织或者个人都可以开发自己的专属 JVM。** 也就是说我们平时接触到的 HotSpot VM 仅仅是是 JVM 规范的一种实现而已。


**JDK 和 JRE**

JDK（Java Development Kit），它是功能齐全的 Java SDK，是提供给开发者使用，能够**创建和编译 Java 程序**的开发套件。它包含了 JRE，同时还包含了编译 java 源码的编译器 javac 以及一些其他工具比如 javadoc（文档注释工具）、jdb（调试器）、jconsole（基于 JMX 的可视化监控⼯具）、javap（反编译工具）等等。

JRE（Java Runtime Environment） 是 **Java 运行时环境**。它是运行已编译 Java 程序所需的所有内容的集合，主要包括 Java 虚拟机（JVM）、Java 基础类库（Class Library）。

也就是说，**JRE 是 Java 运行时环境，仅包含 Java 应用程序的运行时环境和必要的类库**。而 JDK 则包含了 JRE，同时还包括了 javac、javadoc、jdb、jconsole、javap 等工具，可以用于 Java 应用程序的开发和调试。如果需要进行 Java 编程工作，比如编写和编译 Java 程序、使用 Java API 文档等，就需要安装 JDK。而对于某些需要使用 Java 特性的应用程序，如 JSP 转换为 Java Servlet、使用反射等，也需要 JDK 来编译和运行 Java 代码。因此，即使不打算进行 Java 应用程序的开发工作，也有可能需要安装 JDK

![|575](assets/Pasted%20image%2020240720191648.png)

不过，从 JDK 9 开始，就**不需要区分 JDK 和 JRE** 的关系了，取而代之的是**模块系统**。并且，从 JDK 11 开始，Oracle 不再提供单独的 JRE 下载

> 在引入了模块系统之后，JDK 被重新组织成 94 个模块。Java 应用可以通过新增的 jlink 工具，创建出只包含所依赖的 JDK 模块的自定义运行时镜像。这样可以极大的减少 Java 运行时环境的大小

也就是说，可以用 jlink 根据自己的需求，创建一个更小的 runtime（运行时），而不是不管什么应用，都是同样的 JRE。

定制的、模块化的 Java 运行时映像有助于简化 Java 应用的部署和节省内存并增强安全性和可维护性。这对于满足现代应用程序架构的需求，如虚拟化、容器化、微服务和云原生开发，是非常重要的


#### 什么是字节码?采用字节码的好处是什么?

在 Java 中，**JVM 可以理解的代码就叫做字节码**（即扩展名为 `.class` 的文件），它不面向任何特定的处理器，只面向虚拟机。Java 语言通过字节码的方式，在一定程度上**解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点**。所以， Java 程序运行时相对来说还是高效的（不过，和 C、 C++，Rust，Go 等语言还是有一定差距的），而且，由于字节码并不针对一种特定的机器，因此，Java 程序无须重新编译便可在多种不同操作系统的计算机上运行

**Java 程序从源代码到运行的过程如下图所示**：

![](assets/Pasted%20image%2020240720192000.png)

我们需要格外注意的是 `.class->机器码` 这一步。在这一步 JVM 类加载器首先加载字节码文件，然后通过解释器逐行解释执行，这种方式的执行速度会相对比较慢。而且，有些方法和代码块是经常需要被调用的(也就是所谓的热点代码)，所以后面引进了 **JIT（Just in Time Compilation）** 编译器，而 JIT 属于运行时编译。当 JIT 编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。而我们知道，机器码的运行效率肯定是高于 Java 解释器的。这也解释了我们为什么经常会说 **Java 是编译与解释共存的语言**

引入**JIT**之后：

![](assets/Pasted%20image%2020240720192056.png)

JDK、JRE、JVM、JIT 这四者的关系如下图所示：

![|288](assets/Pasted%20image%2020240720192121.png)

