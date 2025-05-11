# Java基础

## Java的特点

1. “一次编写，到处运行”。Java编译器将源代码编译成字节码（bytecode），该字节码可以在任何安装了Java虚拟机（JVM）的系统上运行。
2. 面向对象。Java是一门严格的面向对象编程语言，几乎一切都是对象。面向对象编程（OOP）（Object Oriented Program）代码更易于维护和复用，OOP的三大特性：继承、封装、多态。
3. 内存管理。Java拥有自己的GC（Garbage Collection）机制，自动管理内存和回收不再使用的对象。开发者不再需要手动管理内存，从而减少内存泄露等问题。

## Java面向对象以及面向对象的三大特性

面向对象（Object-Oriented Programming, OOP）是一种编程思想，它通过**模拟现实世界中的对象**来组织和管理代码。核心是将**数据和**操作数据的**方法绑定在一起**，形成“对象”，然后通过对象之间的交互来完成程序功能。以下是简单理解面向对象的三个基本特性：封装、继承和多态。

 **封装 (Encapsulation)：**

封装就是把对象的属性（数据）和行为（方法）打包在一起，对外隐藏细节，只暴露必要的接口。

 **继承 (Inheritance)**

继承允许一个类（子类）继承另一个类（父类）的属性和方法，复用代码并扩展功能。

 **多态 (Polymorphism)**

多态是指同一个接口或方法，在不同对象上有不同的实现方式。分为编译时多态（重载）和运行时多态（重写）。

**编译时多态（重载）**：方法重载是指**同一类中**可以有多个同名方它们具有不同的参数列表（参数类型、数量或顺序不同）。虽然方法名相同，但根据传入的参数不同，编译器会在编译时确定调用哪个方法。

**运行时多态（重写）**：方法重写（Method Overriding）**子类重写父类**的方法，父类引用指向子类对象时，运行时根据实际对象类型决定调用哪个方法。如 父类 Animal 定义了 sound() 方法，子类 Dog 和 Cat 重写它。

**接口多态（Interface Polymorphism）：**多个类实现同一个接口，接口类型的引用可以指向任意实现类的对象，调用时执行具体实现。

```java
interface Speakable {
    void speak();
}
class Human implements Speakable {
    public void speak() {
        System.out.println("你好");
    }
}
class Robot implements Speakable {
    public void speak() {
        System.out.println("Beep Boop");
    }
}
public class Test {
    public static void main(String[] args) {
        Speakable s1 = new Human();
        Speakable s2 = new Robot();
        s1.speak();  // 输出：你好
        s2.speak();  // 输出：Beep Boop
    }
}
```

总结：

- **封装**：藏起来，只给接口，保护数据安全。
- **继承**：拿来用，还能加点自己的东西，复用代码。
- **多态**：同一个动作，不同对象不同表现，灵活扩展。

## Java中多态解决了什么问题

多态是指子类可以替换父类，在实际的代码运行过程中，调用子类的方法实现。多态这种特性也需要编程语言提供特殊的语法机制来实现，比如继承、接口类。

多态可以提高代码的扩展性和复用性，是很多设计模式、设计原则、编程技巧的代码实现基础。比如策略模式、基于接口而非实现编程、依赖倒置原则、里式替换原则、利用多态去掉冗长的 if-else 语句等等

## JVM特点

1. JVM也是一个软件，在不同平台有不同的版本。
2. java文件文件只能编译成一种字节码，而字节码通过不同版本的JVM可以编译为不同的机器码，从而实现在各平台运行一套java文件。
3. JVM主要工作是解释自己的指令集并映射到本地的CPU指令集和OS的系统调用

## JVM、JDK、JRE三者关系

1. JDK（Java Development Kit）Java开发工具包，含JVM、编译器（javac）、调试器（jdb）等开发工具，以及一系列类库。JDK提供了开发、编译、调试和运行Java程序的全部工具和环境。
2. JRE（Java Runtime Environment）Java运行环境，是java运行所需的最小环境。包含了JVM和一系列类库，用来支持Java程序执行。JRE不包含开发工具，只是包含java运行环境。Java11开始取消外置 JRE。

## Java的解释和编译

**Java：半编译 + 半解释 + JIT 即时编译**

- 点击运行后：

1. `javac` 编译 `Main.java` → `Main.class`。
2. JVM 启动，解释器从 `main()` 的第一行开始解释执行。
3. 如果 `someMethod()` 被调用多次，JIT 可能会编译它。

编译性：java首先被编译器编译为字节码，JIT会把编译过的机器码保存起来；

- 字节码不直接对应机器码，而是面向 JVM 的指令集。
- 这一阶段是 **静态编译**（Ahead-of-Time, AOT），发生在程序运行前。

解释性：JVM中一个方法调用计数器，累积到一定值的时候，就用JIT进行编译生成机器码文件。否则就是用解释器进行解释执行，字节码也是通过解释器进行解释运行。

- 当某个方法或循环被频繁调用（达到阈值），JIT 会将其编译为 **本地机器码**，并缓存起来（存入 **Code Cache**）。
- 后续调用直接执行机器码，跳过解释步骤，大幅提升性能。

java即是编译型也是解释性语言，默认采用的是解释器和编译器混合的模式。

## JIT

JIT（Just-In-Time，即时编译器） 是 Java 虚拟机（JVM）中的一种关键技术，用于提升 Java 程序的运行效率。它的核心思想是**在运行时将字节码动态编译成机器码**，从而避免解释执行的性能开销。以下是详细说明：

## 编译型语言和解释型语言

编译型语言：程序执行之前，整个文件会被编译成字节码或者机器码，生成可执行文件。执行时直接运行编译后的代码，速度快，跨平台性较差。如C，C++

解释型语言：程序执行时，逐行解释执行源代码，不生成独立的可执行文件。通常由解释器动态解释并执行代码，跨平台性能好，执行速度较慢。如Python、JavaScript

## C和Java区别是什么

C：面向过程的语言，强调函数和步骤化编程。手动管理内存（如 `malloc` 和 `free`），灵活性高但易出错（内存泄漏、悬空指针等）。编译为机器码，依赖特定平台（需重新编译不同系统版本）。更接近硬件，性能高，常用于系统级开发（如操作系统、嵌入式）。

Java：面向对象的语言（OOP），强调类、对象、继承、多态等概念（尽管也支持部分函数式特性）。自动垃圾回收（GC），开发者无需手动释放内存，安全性更高，但可能牺牲部分性能。编译为字节码（`.class` 文件），通过 JVM（Java 虚拟机）运行，实现“一次编写，到处运行”。因 JVM 和垃圾回收有一定开销，性能通常低于 C，但 JIT 编译器优化后差距缩小。

## Java数据类型

![img](https://cdn.nlark.com/yuque/0/2025/jpeg/50462032/1743487141758-cbf6f325-8be7-4cd8-9629-99878c9b8cc3.jpeg)

以下是一个关于 Java 8 新特性的表格，简洁地列出了每个特性的名称、描述和示例代码：

| **特性名称**     | **描述**                                                | **示例代码**                                                 |
| ---------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| Lambda 表达式    | 提供简洁的匿名函数表达方式，支持函数式编程              | `(x) -> x * 2` 或 `list.forEach(n -> System.out.println(n));` |
| 函数式接口       | 只有一个抽象方法的接口，可与 Lambda 配合使用            | `Predicate<Integer> isEven = n -> n % 2 == 0;`               |
| Stream API       | 对集合进行函数式操作，支持过滤、映射、归约等            | `list.stream().filter(n -> n > 0).map(n -> n * 2).collect(Collectors.toList());` |
| Optional 类      | 处理可能为空的值，避免 NullPointerException             | `Optional.ofNullable(name).orElse("Default");`               |
| 默认方法         | 接口中可定义带实现的默认方法，增强接口扩展性            | `default void say() { System.out.println("Hi"); }`           |
| 静态方法（接口） | 接口中可定义静态方法，直接通过接口调用                  | `static void hi() { System.out.println("Hi"); }`             |
| 新日期时间 API   | 提供线程安全且易用的日期时间处理，替代 Date 和 Calendar | `LocalDate.now().plusDays(7);`                               |
| 方法引用         | 用 `::` 简化 Lambda 表达式，引用已有方法                | `list.forEach(System.out::println);`                         |
| Nashorn JS 引擎  | 在 JVM 中运行 JavaScript 代码（Java 15 已移除）         | `engine.eval("print('Hello');");`                            |
| 并行数组排序     | 利用多核 CPU 加速数组排序                               | `Arrays.parallelSort(array);`                                |
| Base64 支持      | 提供标准的 Base64 编码和解码工具                        | `Base64.getEncoder().encodeToString("text".getBytes());`     |

### 说明

- 这个表格涵盖了 Java 8 的主要新特性，示例代码是简化的片段，实际使用时可能需要更多上下文。
- 如果你需要更详细的说明或针对某个特性扩展表格内容，可以告诉我！

- Java八种基本数据类型的字节数：1字节(byte、boolean)、 2字节(short、char)、4字节(int、float)、8字节(long、double) 
- 浮点数的默认类型为double（如果需要声明一个常量为float型，则必须要在末尾加上f或F） 
- 整数的默认类型为int（声明Long型在末尾加上l或者L） 
- 八种基本数据类型的包装类：除了char的是Character、int类型的是Integer，其他都是首字母大写 
- char类型是无符号的，不能为负，所以是0开始的

## Java内存相关

对于

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int val) {
        this.val = val;
    }
}

public class Solution {
    public void modify(TreeNode node) {
        node.val = 100; // 修改堆中的值
    }

    public static void main(String[] args) {
        TreeNode node = new TreeNode(1); // node 是栈上的引用，指向堆上的对象
        Solution solution = new Solution();
        solution.modify(node);
        System.out.println(node.val); // 输出 100
    }
}
栈（Stack）             堆（Heap）                   方法区（Method Area）
+----------------+      +----------------+         +----------------+
| node -> 0x100  | -->  | TreeNode       |         | TreeNode 类    |
| solution -> 0x200| -->| val: 100      |         | Solution 类    |
| modify 栈帧:   |      | left: null    |          +----------------+
|   node -> 0x100|      | right: null   |
+----------------+      | Solution 对象  |
                        +----------------+
```

stack里存的是 **局部变量** 指向一个堆的地址

每当创建一个新实例（对象）时候 在heap开辟一块内存区域 存储**new创建的对象及其字段值**

方法区存的是**加载过的类的元数据**（包括结构、字节码、静态变量、常量池），可以理解为“出现过的类”的静态信息。

## Java数据类型转换

1. 自动类型转换（隐式转换）

从较小的类型（占用字节数少）自动转换为较大的类型（占用字节数多），无需显式声明，由编译器自动完成。

1. 强制类型转换（显式转换）

从较大的类型转换为较小的类型，或在类型之间不兼容时，需要显式使用 (目标类型) 语法进行转换。 

数据溢出：与数据丢失相反，当将一个范围较小的数据类型转换为一个范围较大的数据类型时，可能会发生数据溢出。例如，将一个int类型的值转换为long类型时，转换结果会填充额外的高位空间，但原始数据仍然保持不变。

数据丢失：当将一个范围较大的数据类型转换为一个范围较小的数据类型时，可能会发生数据丢失。例如，将一个long类型的值转换为int类型时，如果long值超出了int类型的范围，转换结果将是截断后的低位部分，高位部分的数据将丢失。

精度损失：在进行浮点数类型的转换时，可能会发生精度损失。由于浮点数的表示方式不同，将一个单精度浮点数(float)转换为双精度浮点数(double)时，精度可能会损失。

1. 基本类型与包装类的转换

- 装箱（Boxing）：基本类型转为包装类（如 int → Integer）。
- 拆箱（Unboxing）：包装类转为基本类型（如 Integer → int）。

自动装箱/拆箱方便但有开销（如创建对象）

1. 对象类型之间的转换

- 向上转型（Upcasting）：子类转为父类，自动完成。
- 向下转型（Downcasting）：父类转为子类，需要强制转换。

**向下转型需谨慎**：必须确保对象实际类型匹配，否则抛出 ClassCastException。

## Java中BigDecimal和Double

Double 在处理浮点数时存在精度丢失的问题，而 BigDecimal 可以提供任意精度的精确计算。Double 是浮点数，而BigDecimal 直接以十进制运算，结果精确无误差。

## Java中的包装类

Integer对应是int类型的包装类，就是把int类型包装成**Object**对象，对象封装有很多好处，**可以把属性也就是数据跟处理这些数据的方法结合在一起**，比如Integer就有parseInt()等方法来专门处理int型相关的数据。

另一个非常重要的原因就是**在Java中绝大部分方法或类都是用来处理类类型对象的**，如ArrayList集合类就只能以类作为他的存储对象，而这时如果想把一个int型的数据存入list是不可能的，必须把它包装成类，也就是Integer才能被List所接受。所以Integer的存在是很必要的。

*在泛型中：*

在Java中，泛型只能使用引用类型，而不能使用基本类型。

例如，假设我们有一个列表，我们想要将其元素排序，并将排序结果存储在一个新的列表中。如果我们使用基本数据类型int，无法直接使用Collections.sort()方法。但是，如果我们使用Integer包装类，我们就可以轻松地使用Collections.sort()方法。

*在转换中：*

在Java中，基本类型和引用类型不能直接进行转换，必须使用包装类来实现。例如，将一个int类型的值转换为String类型，必须首先将其转换为Integer类型，然后再转换为String类型。

*在集合中：*

Java集合中只能存储对象，而不能存储基本数据类型。

**注意：不管是读写效率，还是存储效率，基本类型都比包装类高效。**

## Java Integer缓存的原理

Integer 类内部维护了一个缓存池，默认缓存范围是 -128 到 127 之间的整数。这个缓存池是在类加载时通过 IntegerCache 静态内部类初始化的。当你使用自动装箱或 Integer.valueOf() 方法创建这些范围内的 Integer 对象时，Java 会直接返回缓存中的对象，而不是每次都创建一个新的实例。

缓存的作用：

- **性能优化**：小范围的整数（如 -128 到 127）在程序中非常常用，通过缓存可以减少对象创建的开销。
- **内存节省**：复用相同的对象，避免重复分配内存。

```java
public class IntegerCacheTest {
    public static void main(String[] args) {
        Integer a = 100;  // 自动装箱，等价于 Integer.valueOf(100)
        Integer b = 100;
        System.out.println(a == b);  // true，引用相同，来自缓存

        Integer c = 200;
        Integer d = 200;
        System.out.println(c == d);  // false，引用不同，新建对象
    }
}
```

## Java抽象类 普通类 接口

**抽象类 (Abstract Class)**：用 abstract 关键字修饰的类，不能直接实例化，通常包含抽象方法（没有实现的方法），也可能包含具体方法。

**特点**： 

- **不能实例化**，必须通过子类继承并实现所有抽象方法后才能使用。
- 可以包含**抽象方法**（无实现，子类必须实现）和**具体方法**（有实现，子类可直接使用）。
- 可以有成员变量、构造方法和普通方法。
- 支持单继承，一个类只能继承一个抽象类。
- 提供部分实现，适合定义一类事物的通用模板。

 **接口 (Interface)：**用 interface 关键字定义，是一种完全抽象的类型（Java 8 之前），只包含抽象方法和常量，Java 8 后支持默认方法和静态方法。

**特点**： 

- **不能实例化**，必须通过实现类（用 implements）来使用。
- 默认方法都是 public abstract（无需显式声明），变量默认是 public static final（常量）。
- Java 8 引入 default 方法（有默认实现）和 static 方法。
- 一个类可以实现多个接口（多实现），弥补 Java 单继承的限制。
- 不允许有构造方法，也不能有实例变量。

| 特性           | 抽象类              | 接口                                      |
| -------------- | ------------------- | ----------------------------------------- |
| **关键字**     | abstract            | interface                                 |
| **实例化**     | 不能                | 不能                                      |
| **方法类型**   | 抽象方法 + 具体方法 | 抽象方法 + 默认方法 + 静态方法 + 私有方法 |
| **成员变量**   | 可有任意类型        | 只能是 public static final（常量）        |
| **构造方法**   | 有                  | 无                                        |
| **继承/实现**  | 单继承 (extends)    | 多实现 (implements)                       |
| **访问修饰符** | 任意                | 默认 public                               |
| **使用场景**   | 模板 + 部分实现     | 行为规范 + 多功能组合                     |

在 Java 中，**抽象类不能加 final 修饰**，因为 abstract 和 final 这两个关键字的含义是互相矛盾的，编译器会报错。

**abstract 的含义**： 

- 抽象类是用 abstract 关键字定义的，目的是作为一个模板，让子类继承并实现其中的抽象方法。
- 抽象类的核心特性是**允许被继承**，否则它的存在就没有意义。

**final 的含义**： 

- final 用在类上时，表示这个类**不能被继承**，它是最终的实现。
- 比如 String 类是 final 的，任何类都无法继承它。

**抽象类可以通过以下方式间接使用：通过子类实例化/匿名内部类**

## Java中静态变量和静态方法

**静态变量 (Static Variables)：**用 static 关键字修饰的变量，也叫类变量，属于类而不是某个对象。

**特点**： 

1. **共享性**：所有类的实例共享同一个静态变量，修改它会影响所有对象。
2. **生命周期**：静态变量在类加载时分配内存，随着类的生命周期存在，直到程序结束或类被卸载。
3. **访问方式**：可以通过类名直接访问（推荐），也可以通过对象访问（不推荐）。
4. **初始化**：如果没有显式初始化，默认值为类型的默认值（如 int 是 0，Object 是 null）。

**静态方法 (Static Methods)：**用 static 关键字修饰的方法，也叫类方法，属于类而不是某个对象。

**特点**： 

1. **无需实例化**：可以通过类名直接调用，不需要创建对象。
2. **限制**：静态方法只能直接访问静态变量和静态方法，不能直接访问实例变量或实例方法，因为它们与对象无关。
3. **用途**：通常用于工具方法或与类相关的操作，不依赖对象状态。

静态变量和静态方法的区别

| 特性         | 静态变量                 | 静态方法             |
| ------------ | ------------------------ | -------------------- |
| **本质**     | 表示类的共享数据         | 表示类的行为或操作   |
| **存储内容** | 数据值（如 int、String） | 方法逻辑（代码块）   |
| **调用限制** | 可被静态/实例方法访问    | 只能直接访问静态成员 |
| **典型用途** | 计数器、全局状态         | 工具函数、工厂方法   |

**总结**

- **静态变量**：类的共享数据，所有实例共用，通过类名访问。
- **静态方法**：类的独立操作，无需对象，直接调用，限制访问静态成员。
- 它们的设计是为了方便类级别的操作，减少对实例的依赖，提升代码效率。

## Java中的 final 作用

修饰变量 (Final Variable)：将变量变为常量，一旦赋值后不能被修改。

**特点**： 

- **基本类型**：值不可变。
- **引用类型**：引用不可变（不能指向其他对象），但对象内容可以改变。
- 必须在声明时初始化，或者在构造方法/静态代码块中初始化（如果是实例变量或静态变量）。

修饰方法 (Final Method)：禁止子类重写（override）该方法。

**特点**： 

- 方法的实现被“锁定”，子类只能调用，不能修改其逻辑。
- 但子类仍可以继承并使用该方法。

 修饰类 (Final Class)：禁止类被继承。

**特点**： 

- 该类是“最终版本”，不能有子类。
- 所有方法隐式变为 final（但字段仍需显式声明 final 才能不可变）。

| 修饰对象 | 作用         | 典型例子            |
| -------- | ------------ | ------------------- |
| **变量** | 不可重新赋值 | final int x = 5;    |
| **方法** | 不可被重写   | final void method() |
| **类**   | 不可被继承   | final class MyClass |

## Java泛型

泛型是 Java 编程语言中的一个重要特性，它允许类、接口和方法在定义时使用一个或多个类型参数，这些类型参数在使用时可以被指定为具体的类型。

泛型的主要目的是在编译时提供更强的类型检查，并且在编译后能够保留类型信息，避免了在运行时出现类型转换异常。

泛型的核心概念

1. **类型参数**：如 <T>、E（元素）、K（键）、V（值），只是占位符，使用时指定具体类型。
2. **类型擦除**：Java 泛型在编译时会擦除类型信息，运行时变为原始类型（如 Object），这是为了向后兼容。 

- 编译前：List<String>
- 编译后：List（原始类型）

1. **通配符**：如 <?>（任意类型）、<? extends T>（上界）、<? super T>（下界），用于灵活处理类型。

总结

- **泛型是什么**：让类、接口、方法支持参数化类型，提升灵活性。
- **为什么需要**： 

1. 提高类型安全性，避免运行时错误。
2. 消除繁琐的类型转换。
3. 增强代码复用性。
4. 支持编译时检查。
5. 代码更具可读性和设计意图。

## Java的异常

Java 异常（Exception）是程序运行时发生的错误或意外情况，用于处理非正常执行流程。以下是简要介绍：

![img](https://cdn.nlark.com/yuque/0/2025/webp/50462032/1744269216054-eb849342-ac39-4c13-8d92-3d89ba6a423d.webp)

### **异常的分类**

Java 中的异常都继承自 Throwable 类，分为两类：

- **Error**：严重问题，通常由 JVM 抛出（如 OutOfMemoryError），程序一般无法处理。
- **Exception**：程序可处理的异常，分为： 

- **受检异常（Checked Exception）**：编译时必须处理，如 IOException。
- **非受检异常（Unchecked Exception）**：运行时异常，如 NullPointerException，继承自 RuntimeException。

```latex
Throwable
├── Error（如 VirtualMachineError、OutOfMemoryError）
└── Exception
    ├── RuntimeException（如 NullPointerException、IllegalArgumentException）
    └── 其他受检异常（如 IOException、SQLException）
```

### **异常处理机制**

Java 使用 try-catch-finally 块处理异常：

- **try**：包裹可能抛出异常的代码。
- **catch**：捕获并处理特定异常。
- **finally**：无论是否发生异常都执行（可选），常用于清理资源。
- **throw**：手动抛出异常。
- **throws**：在方法声明中指定可能抛出的异常。

如果异常是未检查异常或者在方法内部被捕获和处理了，那么就不需要使用throws。

未检查异常（unchecked exceptions）是继承自RuntimeException类或Error类的异常，编译器不强制要求进行异常处理。因此，对于这些异常，不需要在方法签名中使用throws来声明。示例包括NullPointerException、ArrayIndexOutOfBoundsException等。

另一种常见情况是，在方法内部捕获了可能抛出的异常，并在方法内部处理它们，而不是通过throws子句将它们传递到调用者。这种情况下，方法可以处理异常而无需在方法签名中使用throws。

### **自定义异常**

可以通过继承 Exception 或 RuntimeException 创建自定义异常

# 反射

## 什么是反射？

Java 反射机制是在运行状态中，对于任意一个类，都能够知道这个类中的所有属性和方法，对于任意一个

对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为

Java 语言的反射机制。

反射具有以下特性：

1. 运行时类信息访问：反射机制允许程序在运行时获取类的完整结构信息，包括类名、包名、父类、实现的接口、构造函数、方法和字段等。
2. 动态对象的创建：可以使用反射API动态地创建对象实例，即使在编译时不知道具体的类名。这是通过Class类的newInstance()方法或Constructor对象的newInstance()方法实现的。
3. 动态方法的调用：可以在运行时动态地调用对象的方法，包括私有方法。这通过Method类的invoke()方法实现，允许你传入对象实例和参数值来执行方法。
4. 访问和修改字符值：反射还允许程序在运行时访问和修改对象的字段值，即使是私有的。这是通过Field类的get()和set()方法完成的。

![img](https://cdn.nlark.com/yuque/0/2025/png/50462032/1744265823133-ecc97c29-5eec-428d-bb80-ec7bc9ed739f.png)

- **Class****类**：反射的入口点。通过Class.forName("类名")或对象.getClass()获取类的Class对象。
- **Field****类**：表示类的字段，可以获取或设置字段值。
- **Method****类**：表示类的方法，可以通过反射调用方法。
- **Constructor类**：表示类的构造函数，可以动态创建对象。

反射是作用在唯一的Class对象上。Class对象是类的元数据在JVM中的唯一表示，反射通过操作这个Class对象，得以动态地实例化出多个与该类类型相同的实例对象。

- 反射作用在唯一的Class对象上。
- 因为有了这个Class对象作为基础，反射可以通过其构造函数动态实例化出多个与该类相同的对象。
- 这些实例对象是独立的，但都与同一个Class对象关联。

```java
import java.lang.reflect.Field;

class User {
    private String name; // 私有字段，没有setter/getter
}

public class ReflectionTest {
    public static void main(String[] args) throws Exception {
        User user = new User();
        Field field = User.class.getDeclaredField("name"); // 获取name字段的Field对象
        field.setAccessible(true); // 绕过私有限制
        field.set(user, "Alice");  // 设置name字段值为"Alice"
        System.out.println(field.get(user)); // 获取name字段值，输出: Alice
    }
}
```

## 反射在你平时写代码或者框架中的应用场景有哪些?

1. 加载数据库驱动

我们的项目底层数据库有时是用mysql，有时用oracle，需要动态地根据实际情况加载驱动类，这个时候反射就有用了，假设 com.mikechen.java.myqlConnection，com.mikechen.java.oracleConnection这两个类我们要用。

这时候我们在使用 JDBC 连接数据库时使用 Class.forName()通过反射加载数据库的驱动程序，如果是mysql则传入mysql的驱动类，而如果是oracle则传入的参数就变成另一个了。

1. 框架开发中的应用

反射是许多Java框架的核心技术，因为框架需要处理未知的类和对象。

Spring框架 - 依赖注入 (Dependency Injection)

Spring利用反射扫描注解，动态“复制”出Bean（复制人），并嵌套注入依赖，交给容器管理供运行时使用。

- **场景**：Spring通过反射根据配置文件或注解（如@Autowired）动态创建bean实例并注入依赖。
- **实现**：Spring用Class.forName()加载类，通过getDeclaredConstructor()创建实例，再用Field.set()注入属性值。
- **例子**：你定义一个@Component类，Spring在运行时通过反射实例化它并管理。

1. 动态加载类（插件系统）

- **场景**：实现一个插件机制，运行时根据配置加载未知的类。
- **实现**：用Class.forName()加载类名，然后通过反射创建实例并调用方法。
- **例子**：一个游戏程序允许用户上传自定义AI策略类，反射加载这些类并执行。

1. 序列化/反序列化（JSON、XML）

- **场景**：将对象转为JSON或从JSON还原为对象。
- **实现**：反射读取类的字段名和值，生成JSON；或者根据JSON键值对用反射设置对象字段。
- **例子**：Gson/Jackson库用反射解析User对象的name和age字段。

1. 动态代理（Dynamic Proxy）

- **场景**：实现AOP（面向切面编程）或拦截器。
- **实现**：结合java.lang.reflect.Proxy，通过反射动态生成代理类并调用方法。
- **例子**：日志拦截器，在方法调用前后用反射插入日志逻辑。

````java
public class ReflectionDemo {

    public static void main(String[] args) {
        try {
            // --- 目标1：模拟 Student s = new Student("小米", 18); ---

            // 步骤 1: 获取 Student 类的 Class 对象。这是反射的入口。
            //   使用者: java.lang.Class 类 (的静态方法 forName)
            //   使用方法: Class.forName("类的全限定名")
            //   作用: JVM根据类名字符串查找并加载对应的类，返回该类的Class对象。
            System.out.println("反射步骤 1: 获取 Student 类的 Class 对象");
            Class<?> studentClass = Class.forName("com.example.Student"); // 假设 Student 类在 com.example 包下

            // 步骤 2: 获取 Student 类中参数为 (String, int) 的构造方法。
            //   使用者: 上一步获取到的 studentClass (Class 对象)
            //   使用方法: getDeclaredConstructor(Class<?>... parameterTypes)
            //   作用: 根据指定的参数类型列表查找对应的构造方法。
            //          (如果构造方法是public的，也可以用 getConstructor)
            System.out.println("反射步骤 2: 获取 Student 的构造方法 (String, int)");
            Constructor<?> studentConstructor = studentClass.getDeclaredConstructor(String.class, int.class);

            // 步骤 3: (如果构造方法不是public，比如是private或protected，则需要此步骤) 设置构造方法为可访问。
            //   使用者: 上一步获取到的 studentConstructor (Constructor 对象)
            //   使用方法: setAccessible(true)
            //   作用: 破坏封装，允许访问非public的构造方法。对于public构造方法，此步非必需。
            //   studentConstructor.setAccessible(true); // 如果构造函数是私有的，取消这行注释

            // 步骤 4: 使用获取到的构造方法创建 Student 类的实例，并传入构造参数 "小米" 和 18。
            //   使用者: 上一步获取到的 studentConstructor (Constructor 对象)
            //   使用方法: newInstance(Object... initargs)
            //   作用: 调用构造方法，创建对象实例。
            System.out.println("反射步骤 4: 通过构造方法创建 Student 实例");
            Object studentInstance = studentConstructor.newInstance("小米", 18); // 这就相当于执行了 new Student("小米", 18);
                                                                               // studentInstance 现在就是反射创建的 Student 对象 s

            // --- 目标2：模拟 String name = s.getName(); ---

            // 步骤 5: 获取 Student 类中的 getName 方法。
            //   使用者: studentClass (Class 对象) (或者也可以用 studentInstance.getClass() 获取)
            //   使用方法: getMethod(String name, Class<?>... parameterTypes)
            //   作用: 根据方法名和参数类型列表查找对应的public方法 (包括从父类继承的)。
            //          (如果要找非public或仅限本类声明的方法，用 getDeclaredMethod)
            System.out.println("反射步骤 5: 获取 Student 的 getName 方法");
            Method getNameMethod = studentClass.getMethod("getName"); // getName() 通常是public且无参数

            // 步骤 6: (如果方法不是public，比如是private或protected，则需要此步骤) 设置方法为可访问。
            //   使用者: 上一步获取到的 getNameMethod (Method 对象)
            //   使用方法: setAccessible(true)
            //   作用: 破坏封装，允许调用非public的方法。对于public方法，此步非必需。
            //   getNameMethod.setAccessible(true); // 如果getName是私有的，取消这行注释

            // 步骤 7: 调用 studentInstance 对象的 getName 方法。
            //   使用者: 上一步获取到的 getNameMethod (Method 对象)
            //   使用方法: invoke(Object obj, Object... args)
            //   作用: 在指定的对象 obj 上调用此方法，并传入参数 args。
            //          obj: 要调用哪个对象的该方法 (这里是 studentInstance)
            //          args: 方法的参数 (getName 无参，所以不传或传空数组/null)
            System.out.println("反射步骤 7: 调用实例的 getName 方法");
            Object nameObject = getNameMethod.invoke(studentInstance); // 这就相当于执行了 s.getName();
                                                                       // nameObject 现在是 getName() 方法的返回值

            // 步骤 8: 将 invoke 方法返回的 Object 类型结果转换为期望的 String 类型。
            String name = (String) nameObject;

            // 步骤 9: 打印结果验证。
            System.out.println("反射步骤 9: 打印获取到的名字");
            System.out.println("通过反射获取到的名字: " + name);

        } catch (ClassNotFoundException e) {
            System.err.println("错误: 类未找到 - " + e.getMessage());
        } catch (NoSuchMethodException e) {
            System.err.println("错误: 方法未找到 - " + e.getMessage());
        } catch (InstantiationException e) {
            System.err.println("错误: 实例化失败 - " + e.getMessage());
        } catch (IllegalAccessException e) {
            System.err.println("错误: 非法访问 - " + e.getMessage());
        } catch (InvocationTargetException e) {
            // 当被调用的方法本身抛出异常时，会封装在 InvocationTargetException 中
            System.err.println("错误: 调用目标方法时发生异常 - " + e.getTargetException().getMessage());
        }
    }
}

````



# 注解

## Java注解的原理

注解本质是一个继承了Annotation的特殊接口，其具体实现类是Java运行时生成的动态代理类。

注解是一个标记接口，编译成字节码元数据，运行时通过反射读取和处理。

我们通过反射获取注解时，返回的是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。

## Java注解解析的底层实现

Java注解的底层通过编译时嵌入字节码（在.class文件中，注解以RuntimeVisibleAnnotations（运行时可见）或RuntimeInvisibleAnnotations（运行时不可见）的形式存储）、运行时动态代理实现（当类被类加载器加载时，带有RUNTIME保留策略的注解信息会被JVM读取并保留），由反射API读取并驱动框架（如Spring）执行动态行为。

| **阶段**     | **关键行为**                                                 | **技术实现**                                                 |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **编译期**   | 注解写入 `**.class**` 文件                                   | `**RuntimeVisibleAnnotations**` / `**RuntimeInvisibleAnnotations**` 属性 |
| **编译期**   | 注解处理器（如 Lombok）修改代码                              | APT（Abstract Processor Tool）                               |
| **类加载时** | JVM 解析 `**RuntimeVisibleAnnotations**` 并存入 `**Class**` 对象 | 方法区元数据                                                 |
| **运行时**   | 反射读取注解（如 Spring）                                    | `**getAnnotations()**` / `**getDeclaredAnnotations()**`      |
| **运行时**   | 动态代理基于注解实现功能（如 `**@Transactional**`）          | JDK 动态代理 / CGLIB                                         |

## Java注解的作用域

Java 注解的作用域由 @Retention 决定生命周期（SOURCE 仅源代码，CLASS 到字节码，RUNTIME 到运行时）和 @Target 决定适用范围（类、方法、字段等），共同控制注解的可见性和应用场景。

# Object

## == 与 equals 有什么区别

### **==**

- **作用**：比较两个操作数的**引用**（对于基本类型则是值）。
- **适用范围**： 

- **基本数据类型**（如 int、char、double）：比较实际值是否相等。
- **引用类型**（如对象）：比较两个引用是否指向内存中的同一对象（即地址是否相同）。

- **特点**：不关心对象的内容，只看引用是否相同。

比较的是两个字符串内存地址（堆内存）的数值是否相等，属于数值比较；

### **equals**

- **作用**：比较两个对象的**内容**是否相等。
- **适用范围**：仅用于对象（引用类型）。
- **特点**： 

- 默认情况下（未重写时），equals 是 Object 类的方法，行为与 == 相同，比较引用。
- 许多类（如 String、Integer）重写了 equals，使其比较内容而非引用。

比较的是两个字符串的内容，属于内容比较。

## hashcode和equals方法有什么关系

在 Java 中，hashCode 和 equals 方法密切相关，它们都定义在 Object 类中，主要用于对象比较和哈希数据结构（如 HashMap、HashSet）的运作。

- **equals**：比较两个对象是否“相等”，默认比较引用，可被重写以比较内容。
- **hashCode**：返回对象的哈希码（一个整数），用于快速定位对象，默认基于内存地址，可被重写。

### 契约关系

Java 规范规定 hashCode 和 equals 必须满足以下契约：

- **一致性**：如果 a.equals(b) 返回 true，则 a.hashCode() 必须等于 b.hashCode()。
- **非对称性**：如果 a.equals(b) 返回 false，a.hashCode() 和 b.hashCode() 可以相同（哈希冲突），但应尽量避免。
- **稳定性**：在一个对象的生命周期中，只要 equals 使用的属性未变，hashCode 必须始终返回相同值。

**相等的对象必须有相同的哈希码，但哈希码相同的对象不一定相等（equals）**。

```java
String s1 = new String("hello");
String s2 = new String("hello");
System.out.println(s1.equals(s2));     // true
System.out.println(s1.hashCode() == s2.hashCode()); // true
```

s1 和 s2 内容相等，equals 返回 true，它们的 hashCode 也必须相同（**String 类已重写**）。

### 实际作用

- **equals**：判断两个对象在逻辑上是否相同。
- **hashCode**：优化哈希表性能，先用哈希码定位桶（bucket），再用 equals 精确比较。
- **关系示例**： 
- 在 HashMap 中，查找键时先用 hashCode 确定位置，再用 equals 确认是否匹配。

### **默认实现**

- Object 类中： 

- equals：比较引用（==）。
- hashCode：基于对象内存地址生成。

- 如果不重写，两个内容相同但引用不同的对象会有不同 hashCode，不满足一致性。

### **重写规则**

- **必须同时重写**：如果重写了 equals（如比较对象内容），必须重写 hashCode，确保契约成立。

### **违反契约的后果**

- 只重写 equals 不重写 hashCode： 

- 在 HashSet 中，两个“相等”的对象可能被认为是不同的，导致重复存储。

- 只重写 hashCode 不重写 equals： 

- HashMap 可能找不到键，因为 equals 仍比较引用。

## 哈希冲突

**拉链法（Separate Chaining）**

- 将相同哈希码的对象放入同一个“桶”中，用链表（或类似结构）存储。
- Java 的 HashMap 和 HashSet 主要采用此方法。

总结

- Java 中哈希冲突通过**拉链法**（链表）解决，JDK 8 后结合**红黑树**优化性能。
- 冲突不可避免，但可以通过设计良好的 hashCode 和调整 HashMap 参数减少影响。
- 核心是：冲突后仍靠 equals 区分对象，确保功能正确。

## String、StringBuffer、StringBuilder的区别和联系

| 特性         | String                   | StringBuffer       | StringBuilder      |
| ------------ | ------------------------ | ------------------ | ------------------ |
| **可变性**   | 不可变（Immutable）      | 可变（Mutable）    | 可变（Mutable）    |
| **线程安全** | 是（因不可变）           | 是（同步方法）     | 否（非同步）       |
| **性能**     | 拼接慢（每次生成新对象） | 较慢（因同步开销） | 最快（无同步开销） |
| **引入版本** | Java 1.0                 | Java 1.0           | Java 5 (1.5)       |
| **典型场景** | 固定字符串操作           | 多线程字符串拼接   | 单线程字符串拼接   |

**String**

- **不可变**：每次修改（如 + 或 concat）都会创建新对象，原对象不变。
- **线程安全**：因不可变性，天生线程安全。
- **性能**：频繁拼接（如循环中）效率低，因涉及多次对象创建和垃圾回收。

**StringBuffer**

- **可变**：通过内部字符数组操作字符串，修改时不创建新对象。
- **线程安全**：方法（如 append、insert）使用 synchronized 修饰，适合多线程环境。
- **性能**：比 String 快，但因同步开销略慢于 StringBuilder。

**StringBuilder**

- **可变**：与 StringBuffer 类似，内部也是字符数组。
- **非线程安全**：方法无同步，适合单线程使用。
- **性能**：最优，因无同步开销。

StringBuffer 和 StringBuilder 都继承自 AbstractStringBuilder 类，共享相同的底层实现（字符数组）。

String 是独立的类，不继承上述体系。

方法同步：在 Java 中，**方法同步**（Method Synchronization）是指通过某种机制确保一个方法在同一时刻只能被一个线程执行，以避免多线程并发访问时出现数据不一致或线程干扰的问题。同步通常用于保护共享资源（如对象的数据）在多线程环境下的正确性。

# Java8 新特性

| **特性名称**     | **描述**                                                | **示例代码**                                                 |
| ---------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| Lambda 表达式    | 提供简洁的匿名函数表达方式，支持函数式编程              | `(x) -> x * 2` 或 `list.forEach(n -> System.out.println(n));` |
| 函数式接口       | 只有一个抽象方法的接口，可与 Lambda 配合使用            | `Predicate<Integer> isEven = n -> n % 2 == 0;`               |
| Stream API       | 对集合进行函数式操作，支持过滤、映射、归约等            | `list.stream().filter(n -> n > 0).map(n -> n * 2).collect(Collectors.toList());` |
| Optional 类      | 处理可能为空的值，避免 NullPointerException             | `Optional.ofNullable(name).orElse("Default");`               |
| 默认方法         | 接口中可定义带实现的默认方法，增强接口扩展性            | `default void say() { System.out.println("Hi"); }`           |
| 静态方法（接口） | 接口中可定义静态方法，直接通过接口调用                  | `static void hi() { System.out.println("Hi"); }`             |
| 新日期时间 API   | 提供线程安全且易用的日期时间处理，替代 Date 和 Calendar | `LocalDate.now().plusDays(7);`                               |
| 方法引用         | 用 `::` 简化 Lambda 表达式，引用已有方法                | `list.forEach(System.out::println);`                         |
| Nashorn JS 引擎  | 在 JVM 中运行 JavaScript 代码（Java 15 已移除）         | `engine.eval("print('Hello');");`                            |
| 并行数组排序     | 利用多核 CPU 加速数组排序                               | `Arrays.parallelSort(array);`                                |
| Base64 支持      | 提供标准的 Base64 编码和解码工具                        | `Base64.getEncoder().encodeToString("text".getBytes());`     |



## Lambda 表达式

Lambda 表达式是一个**匿名函数**（没有名称的函数），可以将其理解为一段可以传递的代码块。它通常用来实现函数式接口（只有一个抽象方法的接口）的行为。Lambda 表达式通过简洁的语法替代了传统的匿名内部类，使代码更简洁、可读性更高。

### 与方法引用的关系

Lambda 表达式和方法引用（::）密切相关。如果 Lambda 只是调用已有方法，可以用方法引用进一步简化：

- Lambda：x -> System.out.println(x)
- 方法引用：System.out::println

### 总结

- **定义**：Lambda 表达式是一个匿名函数，用于实现函数式接口。
- **语法**：(参数) -> 主体，灵活多变。
- **用途**：简化代码、支持函数式编程、与 Stream API 配合。
- **限制**：依赖函数式接口，外部变量需事实 final。

## Java中stream的API

Java 8 引入的 **Stream API**（位于 java.util.stream 包中）是一个强大的工具，用于以函数式编程的方式处理集合数据。它提供了一种声明式的方式来操作数据，支持过滤、映射、排序、归约等操作，并且可以轻松实现并行处理。

**Stream 的特点**

- **惰性求值**：中间操作不会立即执行，只有遇到终端操作时才会触发计算。
- **一次性使用**：Stream 只能消费一次，消费后不可重用。
- **并行支持**：通过 parallelStream() 或 parallel() 方法轻松实现并行处理。

**Stream 的三个阶段**

- **创建**：从数据源生成 Stream。
- **中间操作**：对数据进行转换或过滤，返回新的 Stream。
- **终端操作**：触发执行并产生最终结果，关闭 Stream。

### 创建 Stream 的方法

1. **从集合创建**

```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();         // 串行流
Stream<String> parallelStream = list.parallelStream(); // 并行流
```

1. 从数组创建

```java
String[] array = {"a", "b", "c"};
Stream<String> stream = Arrays.stream(array);
```

1. 直接创建

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4);
```

1. 从文件或 I/O 创建

```java
try (Stream<String> lines = Files.lines(Paths.get("file.txt"))) {
    lines.forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}
```

1. **无限流**

- Stream.iterate：生成无限序列。
- Stream.generate：通过 Supplier 生成。

```java
Stream<Integer> infinite = Stream.iterate(0, n -> n + 1); // 0, 1, 2, ...
Stream<Double> randoms = Stream.generate(Math::random);   // 随机数流
```

### Stream API 的常用方法

Stream 的操作分为**中间操作**（返回新的 Stream）和**终端操作**（产生结果或副作用）。以下是常用方法的分类和说明：

1. 中间操作（Intermediate Operations）

这些操作是惰性的，不会立即执行，直到遇到终端操作。

| **方法**           | **描述**                       | **示例**                                 |
| ------------------ | ------------------------------ | ---------------------------------------- |
| filter(Predicate)  | 过滤符合条件的元素             | stream.filter(x -> x > 0)                |
| map(Function)      | 将元素映射为新值               | stream.map(String::toUpperCase)          |
| flatMap(Function)  | 将每个元素映射为一个流并扁平化 | stream.flatMap(x -> Stream.of(x, x+1))   |
| distinct()         | 去除重复元素                   | stream.distinct()                        |
| sorted()           | 按自然顺序排序                 | stream.sorted()                          |
| sorted(Comparator) | 按指定比较器排序               | stream.sorted(Comparator.reverseOrder()) |
| limit(long)        | 限制返回元素数量               | stream.limit(3)                          |
| skip(long)         | 跳过前 n 个元素                | stream.skip(2)                           |
| peek(Consumer)     | 查看元素（调试用），不改变流   | stream.peek(System.out::println)         |

1. 终端操作（Terminal Operations）

这些操作触发 Stream 的执行，完成后 Stream 不可再用。

| **方法**                  | **描述**                                 | **示例**                            |
| ------------------------- | ---------------------------------------- | ----------------------------------- |
| forEach(Consumer)         | 对每个元素执行操作                       | stream.forEach(System.out::println) |
| collect(Collector)        | 将流收集为集合或其他结构                 | stream.collect(Collectors.toList()) |
| reduce(T, BinaryOperator) | 归约操作，合并元素为单一结果             | stream.reduce(0, Integer::sum)      |
| count()                   | 返回流中元素数量                         | stream.count()                      |
| anyMatch(Predicate)       | 检查是否至少有一个元素满足条件           | stream.anyMatch(x -> x > 0)         |
| allMatch(Predicate)       | 检查是否所有元素都满足条件               | stream.allMatch(x -> x > 0)         |
| noneMatch(Predicate)      | 检查是否没有元素满足条件                 | stream.noneMatch(x -> x < 0)        |
| findFirst()               | 返回第一个元素（Optional）               | stream.findFirst()                  |
| findAny()                 | 返回任意一个元素（Optional，适合并行流） | stream.findAny()                    |
| toArray()                 | 将流转换为数组                           | stream.toArray(String[]::new)       |

1. Collectors 工具类

Collectors 提供了丰富的收集器，用于将 Stream 转换为集合或其他结构。

- 转为集合：toList()、toSet()、toMap()
- 分组：groupingBy()
- 拼接：joining()
- 统计：summingInt()、averagingInt()

### Stream并行流

**并行流（Parallel Stream）** 是 Stream API 提供的一种功能，允许通过多线程并行处理数据，以充分利用多核处理器的计算能力，从而提高性能。

**定义**：并行流是 Stream API 的一种执行模式，它将数据分割成多个子任务，分配给多个线程并行执行，最终合并结果。

**与串行流的区别**： 

- **串行流（Sequential Stream）**：在一个线程中按顺序处理所有数据。
- **并行流（Parallel Stream）**：将数据分成多个部分，交给多个线程同时处理。

**实现基础**：并行流依赖 Java 7 引入的 Fork/Join 框架，通过线程池（通常是 ForkJoinPool.commonPool()）管理线程。

**将普通流转换为并行流** 用 parallel() 方法。

### 并行流的工作原理

并行流的执行过程可以分为以下步骤：

1. **任务分解（Fork）**： 

- 数据源被分割成多个子集（分而治之），每个子集分配给一个线程。
- 分割基于数据结构（如数组或链表）和任务类型。

1. **并行执行**： 

- 每个线程独立处理自己的子集，执行中间操作（如 filter、map）。
- 使用 ForkJoinPool 中的工作线程，默认线程数与 CPU 核心数相关。

1. **结果合并（Join）**： 

- 各线程处理完子任务后，结果被合并为最终输出（如通过 reduce 或 collect）。

底层依赖：

- **Fork/Join 框架**：通过递归分解任务并合并结果。
- **线程池**：默认使用 ForkJoinPool.commonPool()，线程数通常为 Runtime.getRuntime().availableProcessors()（CPU 核心数）。

## CompletableFuture的使用

### 一、什么是 CompletableFuture？

- **定义**：CompletableFuture 是一个表示异步计算结果的类，可以显式地完成（设置结果或异常），并支持在任务完成后执行回调。
- **与 Future 的区别**： 

- Future：只能通过阻塞（如 get()）获取结果，无法直接添加回调。
- CompletableFuture：支持非阻塞回调、链式操作和组合多个异步任务。

- **核心特点**： 

- 非阻塞：通过回调处理结果。
- 链式操作：支持流式调用（如 thenApply、thenAccept）。
- 异常处理：提供 exceptionally 等方法。

### 二、基本用法

CompletableFuture 的基本使用分为以下几个步骤：

1. **创建 CompletableFuture**。
2. **异步执行任务**。
3. **处理结果或异常**。

### 三、核心方法

CompletableFuture 提供了丰富的 API，主要分为以下几类：

#### 1. 创建和执行

| **方法**              | **描述**             | **示例**                                   |
| --------------------- | -------------------- | ------------------------------------------ |
| supplyAsync(Supplier) | 异步执行并返回结果   | supplyAsync(() -> "Result")                |
| runAsync(Runnable)    | 异步执行无返回值任务 | runAsync(() -> System.out.println("Done")) |

#### 2. 结果处理（回调）

| **方法**             | **描述**                       | **示例**                                  |
| -------------------- | ------------------------------ | ----------------------------------------- |
| thenApply(Function)  | 对结果应用转换函数，返回新结果 | thenApply(s -> s.toUpperCase())           |
| thenAccept(Consumer) | 消费结果，无返回值             | thenAccept(System.out::println)           |
| thenRun(Runnable)    | 任务完成后执行操作，无输入输出 | thenRun(() -> System.out.println("Done")) |

#### 3. 组合多个 CompletableFuture

| **方法**                                 | **描述**                                    | **示例**                                                     |
| ---------------------------------------- | ------------------------------------------- | ------------------------------------------------------------ |
| thenCompose(Function)                    | 将一个 CompletableFuture 的结果传递给另一个 | thenCompose(s -> CompletableFuture.supplyAsync(() -> s + " World")) |
| thenCombine(CompletionStage, BiFunction) | 合并两个 CompletableFuture 的结果           | thenCombine(other, (a, b) -> a + b)                          |
| allOf(CompletableFuture...)              | 等待所有任务完成                            | CompletableFuture.allOf(f1, f2)                              |
| anyOf(CompletableFuture...)              | 等待任一任务完成                            | CompletableFuture.anyOf(f1, f2)                              |

#### 4. 异常处理

| **方法**                | **描述**                   | **示例**                                              |
| ----------------------- | -------------------------- | ----------------------------------------------------- |
| exceptionally(Function) | 处理异常，返回默认值       | exceptionally(t -> "Error: " + t.getMessage())        |
| handle(BiFunction)      | 处理结果或异常，返回新结果 | handle((result, ex) -> ex != null ? "Error" : result) |

#### 5. 手动完成

| **方法**                         | **描述**     | **示例**                                              |
| -------------------------------- | ------------ | ----------------------------------------------------- |
| complete(T)                      | 手动设置结果 | future.complete("Done")                               |
| completeExceptionally(Throwable) | 手动设置异常 | future.completeExceptionally(new Exception("Failed")) |

**例子**

```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class CompletableFutureDetailedExample {
    // 自定义线程池
    private static final ExecutorService executor = Executors.newFixedThreadPool(3);

    public static void main(String[] args) {
        try {
            // 1. 异步查询用户姓名
            CompletableFuture<String> nameFuture = CompletableFuture.supplyAsync(() -> {
                System.out.println("查询姓名线程: " + Thread.currentThread().getName());
                simulateDelay(1); // 模拟耗时操作
                return "Alice";
            }, executor);

            // 2. 异步查询用户年龄（可能抛出异常）
            CompletableFuture<Integer> ageFuture = CompletableFuture.supplyAsync(() -> {
                System.out.println("查询年龄线程: " + Thread.currentThread().getName());
                simulateDelay(2);
                // 模拟异常情况
                if (Math.random() > 0.5) {
                    throw new RuntimeException("年龄查询失败");
                }
                return 25;
            }, executor);

            // 3. 对姓名进行转换（链式操作）
            CompletableFuture<String> processedNameFuture = nameFuture.thenApply(name -> {
                System.out.println("处理姓名线程: " + Thread.currentThread().getName());
                return name.toUpperCase(); // 转换为大写
            });

            // 4. 异常处理（为年龄查询添加备用逻辑）
            CompletableFuture<Integer> safeAgeFuture = ageFuture.exceptionally(throwable -> {
                System.out.println("年龄查询异常: " + throwable.getMessage());
                return 0; // 默认年龄
            });

            // 5. 组合姓名和年龄
            CompletableFuture<String> combinedFuture = processedNameFuture.thenCombine(safeAgeFuture, (name, age) -> {
                System.out.println("组合结果线程: " + Thread.currentThread().getName());
                return String.format("用户: %s, 年龄: %d", name, age);
            });

            // 6. 添加完成后的回调
            combinedFuture.thenAccept(result -> {
                System.out.println("最终结果: " + result);
            }).exceptionally(throwable -> {
                System.out.println("组合过程异常: " + throwable.getMessage());
                return null;
            });

            // 7. 等待所有任务完成（仅用于演示，实际生产中尽量避免阻塞）
            combinedFuture.get(5, TimeUnit.SECONDS);

            // 8. 检查任务状态
            System.out.println("姓名任务是否完成: " + nameFuture.isDone());
            System.out.println("年龄任务是否异常: " + ageFuture.isCompletedExceptionally());

        } catch (Exception e) {
            System.err.println("主线程异常: " + e.getMessage());
        } finally {
            // 关闭线程池
            executor.shutdown();
            try {
                if (!executor.awaitTermination(2, TimeUnit.SECONDS)) {
                    executor.shutdownNow();
                }
            } catch (InterruptedException e) {
                executor.shutdownNow();
            }
        }
    }
}
```

# 序列化

## 怎么把一个对象从一个jvm转移到另一个jvm

在 Java 中，将一个对象从一个 JVM（Java Virtual Machine）转移到另一个 JVM 本质上是一个**对象序列化**和**网络传输**的过程，因为每个 JVM 都有独立的内存空间，对象无法直接跨 JVM 共享。常见的实现方式是通过序列化将对象转换为字节流，然后通过网络传输到目标 JVM，最后在目标 JVM 上反序列化还原对象。

**序列化（Serialization）**： 

- 将对象转换为字节流（或其他可传输的格式，如 JSON）。
- Java 提供了内置的 java.io.Serializable 接口来支持序列化。

**网络传输**： 

- 使用 Socket、RMI（远程方法调用）、HTTP 或消息队列（如 RabbitMQ、Kafka）将字节流发送到目标 JVM。

**反序列化（Deserialization）**： 

- 在目标 JVM 上将字节流还原为对象。

以下是具体的实现流程：

1. 确保对象可序列化

- 对象类必须实现 Serializable 接口。
- 如果对象包含不可序列化的字段，需标记为 transient，或自定义序列化逻辑。
- 在源 JVM 中序列化对象

- 使用 ObjectOutputStream 将对象写入字节流。
- 通过网络传输字节流

- 使用 Socket、RMI 或其他通信方式将字节流发送到目标 JVM。
- 在目标 JVM 中反序列化

- 使用 ObjectInputStream 将字节流还原为对象。

以下是实现方式方法：**Socket 通信** 、**RMI**、**HTTP/REST API**、**消息队列**

**自己实现：使用别人轮子 Jackson Fastjson**

# 设计模式

## volatile和sychronized如何实现单例模式

在 Java 中，单例模式（Singleton Pattern）是一种常见的设计模式，确保一个类只有一个实例，并提供全局访问点。在多线程环境下，实现线程安全的单例模式需要特别注意并发问题。volatile 和 synchronized 是两种常用的关键字，可以用来确保单例模式的线程安全性。

### 使用 synchronized 实现单例模式

1. 懒汉式（Lazy Initialization）

- **特点**：在第一次调用时才创建实例。
- **线程安全**：使用 synchronized 同步方法或代码块。

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {} // 私有构造器，防止外部实例化

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

原理分析

**同步方法**： 

- 使用 synchronized 修饰整个 getInstance 方法，确保同一时刻只有一个线程能执行。
- 简单但性能较低，每次调用都会加锁，即使实例已创建。

### 使用 volatile + synchronized 实现单例模式（优化 DCL）

双重检查锁（带 volatile）

```java
public class Singleton {
    // 使用 volatile 防止指令重排序
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) { // 第一次检查
            synchronized (Singleton.class) {
                if (instance == null) { // 第二次检查
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

原理分析

1. **指令重排序问题**： 

- instance = new Singleton() 不是原子操作，分三步： 

1. 分配内存。
2. 初始化对象。
3. 将 instance 指向内存地址。

- 在无 volatile 的情况下，JVM 可能重排序为 1 -> 3 -> 2。另一个线程可能在步骤 3 后看到 instance != null，但对象尚未初始化，导致访问未完成的对象。

1. **volatile** **的作用**： 

- **禁止指令重排序**：确保 instance 赋值前对象已完全初始化。
- **内存可见性**：保证一个线程修改 instance 后，其他线程立即可见。

1. **synchronized** **的作用**： 

- 确保只有一个线程进入创建实例的代码块。

### 使用 volatile 实现单例模式（静态内部类）

```java
public class Singleton {
    private Singleton() {}

    // 静态内部类持有单例实例
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

原理分析

1. **类加载机制**： 

- Java 类加载是线程安全的，SingletonHolder 只有在首次调用 getInstance() 时才加载。
- JVM 保证静态字段（如 INSTANCE）在类初始化时只赋值一次。

1. **volatile** **的作用**： 

- 通常不需要，因为类加载本身已确保线程安全。
- 在极端场景下（如反射攻击或复杂类加载器），可用 volatile 增强可见性。

优点

- **懒加载**：只有调用 getInstance() 时才创建实例。
- **线程安全**：无需显式同步，依赖 JVM 类加载机制。
- **简洁高效**：性能最佳，无锁开销。

### 饿汉式（结合 volatile，可选）

```java
public class Singleton {
    // 类加载时立即初始化
    private static volatile Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

原理分析

- **饿汉式**：类加载时创建实例，非懒加载。
- **volatile**： 

- 确保 instance 的内存可见性（多线程环境下）。
- 防止指令重排序（尽管类加载已足够安全，volatile 是可选增强）。

优点

- 简单，线程安全。
- 无需同步，性能高。

缺点

- 非懒加载，可能浪费资源（如果实例未被使用）。

### 总结

1. **synchronized**： 

- 适合简单场景（如同步方法）或配合 DCL。
- 提供互斥锁，确保线程安全。

1. **volatile**： 

- 解决指令重排序和内存可见性问题。
- 在 DCL 中必不可少，在其他方式中可选。

1. **推荐方式**： 

- **静态内部类**：最优雅，推荐默认使用。
- **DCL（带 volatile）**：高并发场景。
- **饿汉式**：无需懒加载时。



# I/O

## Java怎么实现网络IO高并发编程

可以用 Java NIO ，是一种同步非阻塞的I/O模型，也是I/O多路复用的基础。

传统的BIO里面socket.read()，如果TCP RecvBuffer里没有数据，函数会一直阻塞，直到收到数据，返回读到的数据， 如果使用BIO要想要并发处理多个客户端的i/o，那么会使用多线程模式，一个线程专门处理一个客户端 io，这种模式随着客户端越来越多，所需要创建的线程也越来越多，会急剧消耗系统的性能。

![img](https://cdn.nlark.com/yuque/0/2025/png/50462032/1744272738702-01cecfbf-ab8c-4783-90e0-d0255a2b1941.png)

NIO 是基于I/O多路复用实现的，它可以只用一个线程处理多个客户端I/O，如果你需要同时管理成千上万的连接，但是每个连接只发送少量数据，例如一个聊天服务器，用NIO实现会更好一些。

![img](https://cdn.nlark.com/yuque/0/2025/png/50462032/1744272743230-76a86a88-21c4-4290-81c7-964a73939af6.png)

## BIO、NIO、AIO区别是什么

BIO（blocking IO）：就是传统的 java.io 包，它是基于流模型实现的，交互的方式是同步、阻塞方式，也就是说在读入输入流或者输出流时，在读写动作完成之前，线程会一直阻塞在那里，它们之间的调用是可靠的线性顺序。优点是代码比较简单、直观；缺点是 IO 的效率和扩展性很低，容易成为应用性能瓶颈。

NIO（non-blocking IO） ：Java 1.4 引入的 java.nio 包，提供了 Channel、Selector、Buffer 等新的抽象，可以构建多路复用的、同步非阻塞 IO 程序，同时提供了更接近操作系统底层高性能的数据操作方式。

AIO（Asynchronous IO） ：是 Java 1.7 之后引入的包，是 NIO 的升级版本，提供了异步非堵塞的 IO 操作方式，所以人们叫它 AIO（Asynchronous IO），异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。

## NIO是怎么实现的

NIO是一种同步非阻塞的IO模型，所以也可以叫NON-BLOCKINGIO。同步是指线程不断轮询IO事件是否就绪，非阻塞是指线程在等待IO的时候，可以同时做其他任务。

同步的核心就Selector（I/O多路复用），Selector代替了线程本身轮询IO事件，避免了阻塞同时减少了不必要的线程消耗；非阻塞的核心就是通道和缓冲区，当IO事件就绪时，可以通过写到缓冲区，保证IO的成功，而无需线程阻塞式地等待。

NIO由一个专门的线程处理所有IO事件，并负责分发。事件驱动机制，事件到来的时候触发操作，不需要阻塞的监视事件。线程之间通过wait,notify通信，减少线程切换。

NIO主要有三大核心部分：Channel(通道)，Buffer(缓冲区), Selector。传统IO基于字节流和字符流进行操作，而NIO基于Channel和Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。

Selector(选择区)用于监听多个通道的事件（比如：连接打开，数据到达）。因此，单个线程可以监听多个数据通道。

![img](https://cdn.nlark.com/yuque/0/2025/webp/50462032/1744272827433-32dfebd7-f8b6-48f9-94fe-8a16030c678d.webp)

# Native方法

在Java中，native方法是一种特殊类型的方法，它允许Java代码调用外部的本地代码，即用C、C++或其他语言编写的代码。native关键字是Java语言中的一种声明，用于标记一个方法的实现将在外部定义。

在Java类中，native方法看起来与其他方法相似，只是其方法体由native关键字代替，没有实际的实现代码。例如：

```java
public class NativeExample {
    public native void nativeMethod();
}
```

要实现native方法，你需要完成以下步骤：

生成JNI头文件：使用javah工具从你的Java类生成C/C++的头文件，这个头文件包含了所有native方法的原型。

编写本地代码：使用C/C++编写本地方法的实现，并确保方法签名与生成的头文件中的原型匹配。

编译本地代码：将C/C++代码编译成动态链接库（DLL，在Windows上），共享库（SO，在Linux上）

加载本地库：在Java程序中，使用System.loadLibrary()方法来加载你编译好的本地库，这样JVM就能找到并调用native方法的实现了。

# 栈帧地址值equals() ==

````java
class Dog{
    private String name;
    private Integer age;
    public Dog(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
    @Override
    public boolean equals(Object obj) {
        // 1. 检查是否为同一个对象的引用
        if (this == obj) {
            return true;
        }

        // 2. 检查 obj 是否为 null，以及 obj 是否是 Dog 类型的实例
        //    使用 getClass() != obj.getClass() 比 instanceof 更严格，确保是 Dog 而不是其子类。
        //    如果允许子类比较，可以使用 instanceof Dog。
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }

        // 3. 将 obj 类型转换为 Dog
        Dog otherDog = (Dog) obj;

        // 4. 比较属性值
        //    对于对象类型的属性 (如 String, Integer)，使用 .equals() 方法进行比较。
        //    使用 Objects.equals() 可以优雅地处理属性值为 null 的情况。
        return Objects.equals(this.name, otherDog.name) &&
               Objects.equals(this.age, otherDog.age);
        //如果是int age 就只用 && obj.age == otherDog.age ;
    }

    // 当重写 equals() 方法时，通常也需要重写 hashCode() 方法。
    // 这是为了保证 "相等的对象必须具有相等的哈希码" 这一约定。
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}

Dog a = new Dog("cat",32);
Dog b = new Dog("cat",31);
Dog c = new Dog("cat",32);
int d = 2;
Intger e = 2;
````

栈stack: a , b , c , e ， d=2

堆heap:  Dog("cat",32)  Dog("cat",32) new Dog("cat",31)  2

a==c 返回false (比较的是地址值 因为new出来的实例 指向不同堆地址)

a.equals(c) 返回true (比较的是对象属性) (前提看Dog类是否用的Object的equals方法 是否重写了 默认比较的地址值也就是和 == 一样的结果)

d==e 返回true(自动拆箱成int)（`==` 对于基本数据类型比较的是它们的值）

e.equals(d) 返回true (`Integer` 类的 `equals()` 方法会比较所包装的数值。`d` (值为2) 与 `e` 所包装的数值 (2) 相等。在 `equals` 方法内部，如果传入的是 `int`，它会被视为与 `Integer` 对象内部的 `int` 值进行比较。)

# 设计模式

设计模式通常分为三大类：

1. **创建型模式 (Creational Patterns)**：与对象的创建有关，旨在以灵活和解耦的方式创建对象。
2. **结构型模式 (Structural Patterns)**：处理类和对象的组合，以形成更大的结构。
3. **行为型模式 (Behavioral Patterns)**：关注对象之间的职责分配和通信。

**创建型模式 (Creational Patterns)** - 共 5 种

1. **单例模式 (Singleton Pattern)**
   - **介绍**：确保一个类只有一个实例，并提供一个全局访问点来获取这个唯一的实例。
   - **例子**：你提到的 MyBatis 中的 `ErrorContext` 和 `LogFactory`，以及常见的数据库连接池、应用程序的配置对象等。
2. **工厂方法模式 (Factory Method Pattern)**
   - **介绍**：定义一个用于创建对象的接口，但让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。
   - **例子**：JDBC 中的 `Connection.createStatement()`，集合框架中的迭代器 `iterator()` 方法。
3. **抽象工厂模式 (Abstract Factory Pattern)**
   - **介绍**：提供一个接口，用于创建一系列相关或相互依赖的对象，而无需指定它们具体的类。
   - **例子**：更换UI主题（如同时更换按钮、文本框、窗口的风格），数据库访问层切换不同数据库（如`SQL ServerFactory`、`OracleFactory`）。
4. **建造者模式 (Builder Pattern)**
   - **介绍**：将一个复杂对象的构建与其表示分离，使得同样的构建过程可以创建不同的表示。它一步一步创建一个复杂的对象，允许用户只通过指定复杂对象的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节。
   - **例子**：你提到的 MyBatis 中的 `SqlSessionFactoryBuilder`、`XMLConfigBuilder` 等，以及 `StringBuilder`、`DocumentBuilder`。
5. **原型模式 (Prototype Pattern)**
   - **介绍**：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。当直接创建对象的代价比较大时，则采用这种模式。
   - **例子**：`Object.clone()` 方法（尽管Java中的 `clone()` 有其复杂性），需要创建大量相似对象，如细胞分裂。

**结构型模式 (Structural Patterns)** - 共 7 种

1. **适配器模式 (Adapter Pattern)**
   - **介绍**：将一个类的接口转换成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
   - **例子**：你提到的 MyBatis 中 Log 接口的适配，`java.io.InputStreamReader(InputStream)` (将字节流适配成字符流)，`java.util.Arrays.asList()`。
2. **桥接模式 (Bridge Pattern)**
   - **介绍**：将抽象部分与它的实现部分分离，使它们都可以独立地变化。它通过提供抽象化和实现化之间的桥接结构，来实现二者的解耦。
   - **例子**：JDBC 驱动程序，不同品牌的汽车（抽象）和不同类型的引擎（实现）可以自由组合。
3. **组合模式 (Composite Pattern)**
   - **介绍**：将对象组合成树形结构以表示“部分-整体”的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。
   - **例子**：你提到的 MyBatis 中的 `SqlNode`，图形用户界面中的窗口和容器，文件系统中的文件和文件夹。
4. **装饰者模式 (Decorator Pattern)**
   - **介绍**：动态地给一个对象添加一些额外的职责。就增加功能来说，装饰者模式相比生成子类更为灵活。
   - **例子**：你提到的 MyBatis 中 Cache 的装饰者，Java I/O 中的 `BufferedInputStream(FileInputStream)`，`Collections.synchronizedList()`。
5. **外观模式 (Facade Pattern)**
   - **介绍**：为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。
   - **例子**：JDBC 中的 `DriverManager.getConnection()` (背后隐藏了驱动加载、连接建立等复杂性)，SLF4J 作为一个日志门面。
6. **享元模式 (Flyweight Pattern)**
   - **介绍**：运用共享技术有效地支持大量细粒度的对象，以减少内存占用和提高性能。它通过共享不变的部分，将变化的部分作为外部状态传入。
   - **例子**：Java 中的 `String` 常量池，数据库连接池中的连接对象，围棋的棋子。
7. **代理模式 (Proxy Pattern)**
   - **介绍**：为其他对象提供一种代理以控制对这个对象的访问。
   - **例子**：你提到的 MyBatis 中 `MapperProxy` 和 `ConnectionLogger` (JDK动态代理)，Spring AOP 中的代理，Hibernate 的延迟加载，RPC (远程过程调用)。

**行为型模式 (Behavioral Patterns)** - 共 11 种

1. **责任链模式 (Chain of Responsibility Pattern)**
   - **介绍**：为请求创建了一个接收者对象的链。这种模式使得请求的发送者和接收者进行解耦。请求沿着链传递，直到有一个对象处理它为止。
   - **例子**：Java Web 中的 Filter 链，Servlet 的 `doFilter`，异常处理机制。
2. **命令模式 (Command Pattern)**
   - **介绍**：将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可撤销的操作。
   - **例子**：线程池中的 `Runnable` 接口，GUI 中的按钮点击事件处理，事务操作。
3. **解释器模式 (Interpreter Pattern)**
   - **介绍**：给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。
   - **例子**：正则表达式引擎 (`java.util.regex.Pattern`)，SQL 解析器，编译器中的语法分析。
4. **迭代器模式 (Iterator Pattern)**
   - **介绍**：提供一种方法顺序访问一个聚合对象中各个元素, 而又无须暴露该对象的内部表示。
   - **例子**：你提到的 `PropertyTokenizer`，Java 集合框架中的 `Iterator` 接口。
5. **中介者模式 (Mediator Pattern)**
   - **介绍**：用一个中介对象来封装一系列的对象交互。中介者使各个对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
   - **例子**：`java.util.Timer` (调度任务)，GUI 中的对话框与各组件的协调，聊天室。
6. **备忘录模式 (Memento Pattern)**
   - **介绍**：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。
   - **例子**：文本编辑器的撤销/重做功能，游戏存档。
7. **观察者模式 (Observer Pattern)**
   - **介绍**：定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。也称为发布-订阅模式。
   - **例子**：Java中的 `java.util.Observer` 和 `java.util.Observable` (已废弃，推荐使用 `PropertyChangeListener` 或其他事件机制)，GUI 中的事件监听器 (如 `ActionListener`)，消息队列。
8. **状态模式 (State Pattern)**
   - **介绍**：允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了它的类。
   - **例子**：TCP 连接的状态（建立连接、监听、关闭等），订单状态流转。
9. **策略模式 (Strategy Pattern)**
   - **介绍**：定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化。
   - **例子**：`java.util.Comparator` 接口（用于 `Collections.sort()`），不同的支付方式，不同的压缩算法。
10. **模板方法模式 (Template Method Pattern)**
    - **介绍**：在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
    - **例子**：你提到的 MyBatis 中的 `BaseExecutor` 和 `BaseTypeHandler`，`HttpServlet` 中的 `doGet()`、`doPost()` 方法，`AbstractList` 中的一些通用方法。
11. **访问者模式 (Visitor Pattern)**
    - **介绍**：表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。
    - **例子**：编译器中对抽象语法树（AST）的不同处理阶段（如类型检查、代码生成），对复杂对象结构（如XML文档）执行多种不同的操作。

# 工厂模式BeanFactory

**工厂模式的核心思想：**

- **封装对象的创建过程**：将创建对象的具体逻辑（使用哪个类、如何初始化等）放到一个单独的“工厂”中。
- **解耦**：调用者（客户端代码）不需要知道它所使用的对象的具体实现类是什么，只需要知道它需要什么类型（接口或抽象类）的对象。这使得更换或添加新的具体实现变得容易，而不需要修改客户端代码。
- **提高灵活性和可维护性**：当创建逻辑复杂或需要根据不同条件创建不同对象时，工厂模式能让代码更清晰、更易于管理。

**常见的工厂模式变体：**

1. **简单工厂模式 (Simple Factory Pattern)**：一个工厂类，根据传入的参数来决定创建哪种产品类的实例。严格来说，它不属于 GoF 23 种设计模式之一，但非常常用。

   Java

   ```
   // 简单工厂
   public class ToyFactory {
       public static Toy createToy(String type) {
           if ("car".equals(type)) {
               return new ToyCar();
           } else if ("doll".equals(type)) {
               return new ToyDoll();
           }
           return null;
       }
   }
   // 客户端
   Toy myCar = ToyFactory.createToy("car");
   ```

2. **工厂方法模式 (Factory Method Pattern)**：定义一个用于创建对象的接口（工厂接口），但让实现该接口的子类（具体工厂）来决定实例化哪一个类。这使得一个类的实例化延迟到其子类。

   Java

   ```
   // 抽象产品
   interface Toy {}
   class ToyCar implements Toy {}
   // 抽象工厂
   interface ToyFactory {
       Toy createToy();
   }
   // 具体工厂
   class CarFactory implements ToyFactory {
       public Toy createToy() {
           return new ToyCar();
       }
   }
   // 客户端
   ToyFactory carFactory = new CarFactory();
   Toy myCar = carFactory.createToy();
   ```

3. **抽象工厂模式 (Abstract Factory Pattern)**：提供一个接口，用于创建一系列相关或相互依赖的对象，而无需指定它们具体的类。这个工厂能生产一“族”产品。

   Java

   ```
   // 假设有两种产品：车和娃娃，每种产品都有现代款和复古款
   interface Car extends Toy {}
   class ModernCar implements Car {}
   class VintageCar implements Car {}
   
   interface Doll extends Toy {}
   class ModernDoll implements Doll {}
   class VintageDoll implements Doll {}
   
   // 抽象工厂 - 能生产一套风格的产品
   interface AbstractToyFactory {
       Car createCar();
       Doll createDoll();
   }
   // 具体工厂 - 生产现代风格的产品
   class ModernToyFactory implements AbstractToyFactory {
       public Car createCar() { return new ModernCar(); }
       public Doll createDoll() { return new ModernDoll(); }
   }
   // 客户端
   AbstractToyFactory modernFactory = new ModernToyFactory();
   Car modernCar = modernFactory.createCar();
   Doll modernDoll = modernFactory.createDoll();
   ```

**Spring 中的 `BeanFactory` 是什么？**

`BeanFactory` 是 Spring 框架的**核心接口**，它是 Spring IoC (Inversion of Control，控制反转) 容器的基础。你可以把它看作是一个**超级智能、高度可配置的“超级工厂”**。

**`BeanFactory` 的主要职责和特性：**

1. **Bean 的工厂和管理者**：
   - 它负责创建、配置和管理应用程序中的对象（在 Spring 中称为 "Bean"）。
   - 你不再需要自己去 `new` 对象，而是告诉 `BeanFactory` 你需要哪个 Bean，它会为你提供该 Bean 的实例。
2. **IoC (控制反转) 的体现**：
   - 传统的对象创建和依赖关系是由开发者在代码中直接控制的（比如 `A a = new A(); B b = new B(a);`）。
   - 使用 `BeanFactory` 后，对象的创建权和依赖关系的注入权都**反转**给了 Spring 容器。开发者只需要声明需要什么（比如通过 XML 配置或注解），容器会负责组装。
3. **依赖注入 (Dependency Injection - DI)**：
   - `BeanFactory` 不仅创建 Bean，还能自动解决 Bean 之间的依赖关系。例如，如果你的 `OrderService` Bean 需要一个 `OrderRepository` Bean，`BeanFactory` 会在创建 `OrderService` 时，自动将一个 `OrderRepository` 的实例“注入”进去。
4. **Bean 的生命周期管理**：
   - 从 Bean 的定义、实例化、属性填充、初始化（调用初始化回调方法如 `@PostConstruct` 或 `afterPropertiesSet()`），到最终的销毁（调用销毁回调方法如 `@PreDestroy` 或 `destroy()`），`BeanFactory` 管理着 Bean 的整个生命周期。
5. **配置驱动**：
   - `BeanFactory` 通过读取配置元数据（通常是 XML 文件、Java 注解如 `@Component`, `@Service`, `@Configuration`, `@Bean`，或 Groovy 脚本等）来了解需要创建哪些 Bean，以及它们之间的关系。
6. **延迟加载 (Lazy Loading) vs. 预实例化 (Eager Instantiation)**：
   - `BeanFactory` 的默认行为通常是延迟加载 Bean，即只有当第一次请求某个 Bean 时（通过 `getBean()` 方法），它才会被真正实例化和初始化。
   - 它的一个更常用的子接口 `ApplicationContext` 则通常在容器启动时就预先实例化所有的单例（Singleton）Bean（这是默认作用域）。

**总结一下 `BeanFactory` 的“工厂”角色：**

- 它是一个生产和管理你应用中所有重要对象（Beans）的中央工厂。
- 你不需要关心这些对象是如何被创建和组装的，只需要从这个“工厂”中获取它们。
- 它使得你的组件之间解耦，更容易测试和维护。

当你看到 `@Autowired` 注解时，背后就是 `ApplicationContext` (作为 `BeanFactory` 的一个实现) 在工作，它为你找到了合适的 Bean 实例并注入到需要的地方。这就是 Spring 作为 IoC 容器和 Bean 工厂的强大之处。