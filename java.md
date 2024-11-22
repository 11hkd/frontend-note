# 后端八股-JAVA

### 重载和重写的区别

**重载**：在同一个类中，允许存在多个同名方法，只要它们的参数列表不同（参数个数、参数类型、参数顺序不同），与返回值类型和访问修饰符无关。

**重写**：发生在子类和父类之间，子类中定义了与父类中同名同参数列表的方法，并且返回值类型和异常类型也要和父类保持一致或者是父类返回值类型的子类，访问修饰符的限制要小于等于父类的访问修饰符。

### String 和 StringBuffer、StringBuilder 的区别

**String**、**StringBuffer** 和 **StringBuilder** 都是 Java 中用于处理字符串的类，它们之间存在以下区别：

#### 可变性

- **String**：不可变的字符序列。一旦创建，其内容就不能被改变。对 `String` 对象进行的任何修改操作，实际上都会创建一个新的 `String` 对象来存储修改后的结果，而原始的 `String` 对象保持不变。
- **StringBuffer**：可变的字符序列。可以通过调用其各种方法来直接修改对象内部存储的字符序列，而无需创建新的对象。
- **StringBuilder**：可变的字符序列，与 `StringBuffer` 类似，但在单线程环境下具有更好的性能表现，因为它的方法没有被 `synchronized` 关键字修饰，不需要进行同步处理，执行效率相对较高。

#### 线程安全性

- **String**：由于其不可变性，因此是线程安全的。多个线程可以同时访问同一个 `String` 对象，而不会导致数据不一致的问题。
- **StringBuffer**：线程安全的类，其内部的大多数方法都使用了 `synchronized` 关键字进行修饰，保证了在多线程环境下对同一个 `StringBuffer` 对象进行操作时的线程安全性。
- **StringBuilder**：非线程安全的类，在多线程环境下如果多个线程同时访问和修改同一个 `StringBuilder` 对象，可能会导致数据不一致或其他并发问题。

#### 性能表现

- **String**：由于每次修改都会创建新的对象，因此在频繁修改字符串的场景下，性能相对较差，会产生大量的临时对象，增加了垃圾回收的压力。
- **StringBuffer**：虽然是可变的，但由于其线程安全的特性，在多线程环境下每次操作都需要进行同步处理，这在一定程度上会影响性能，所以在单线程环境下性能不如 `StringBuilder`。
- **StringBuilder**：在单线程环境下，由于不需要进行同步处理，其性能要优于 `StringBuffer`，适合在单线程中频繁修改字符串的场景。

#### 适用场景

- **String**：适用于字符串内容不需要经常改变的场景，例如常量定义、配置信息等。由于其不可变性和线程安全性，在多线程环境下共享字符串数据时非常方便。
- **StringBuffer**：适用于多线程环境下需要对字符串进行频繁修改的场景，例如在网络编程中对数据的拼接和处理等，其线程安全的特性可以保证数据的一致性。
- **StringBuilder**：适用于单线程环境下对字符串进行频繁修改的场景，如在循环中对字符串进行大量的拼接操作等，可以提高程序的性能。

### String 不可变的原因

- **安全性**：字符串在许多应用中被广泛用于表示不可变的数据，如文件名、URL、密码等。如果 `String` 是可变的，那么这些重要的数据就可能会被意外或恶意地修改，从而导致安全漏洞。
- **哈希码的一致性**：`String` 类重写了 `hashCode()` 方法，其哈希码是根据字符串的内容计算得出的。如果 `String` 是可变的，那么当字符串的内容发生变化时，其哈希码也会改变，这将导致在使用哈希表存储和查找 `String` 对象时出现问题，例如无法正确地获取和更新与该字符串相关联的值。
- **字符串常量池的优化**：Java 中的字符串常量池是为了提高字符串的使用效率和节省内存空间而设计的。由于 `String` 是不可变的，相同内容的字符串可以在常量池中共享，避免了重复创建相同字符串对象的开销。如果 `String` 是可变的，那么字符串常量池的这种优化机制将无法实现，因为一个字符串对象的内容可能会随时改变，无法保证其在常量池中的唯一性和一致性。
- **线程安全**：不可变性使得 `String` 对象在多线程环境下可以安全地共享，无需额外的同步操作，从而提高了程序的性能和并发处理能力。

综上所述，`String`、`StringBuffer` 和 `StringBuilder` 各有特点，在实际使用中应根据具体的需求和场景来选择合适的类来处理字符串，以达到最佳的性能和功能要求。

### 自动装箱与自动拆箱

**自动装箱**：Java 自动将基本数据类型转换为对应的包装类型的过程。例如，将 `int` 类型的变量自动转换为 `Integer` 类型的对象。

**自动拆箱**：Java 自动将包装类型转换为对应的基本数据类型的过程。例如，将 `Integer` 类型的对象自动转换为 `int` 类型的值。

#### 注意事项

- **性能问题**：虽然自动装箱和拆箱为编程带来了便利，但在一些性能敏感的场景中需要注意其性能开销。因为自动装箱和拆箱实际上是通过调用包装类型的 `valueOf()` 方法和相应的 `xxxValue()` 方法来实现的，频繁的装箱和拆箱操作可能会影响程序的性能。
- **空指针异常**：在使用自动拆箱时，如果包装类型的对象为 `null`，则会抛出 `NullPointerException`。因此，在进行自动拆箱操作之前，需要确保包装类型的对象不为 `null`。
- **缓存机制**：对于 `Byte`、`Short`、`Integer`、`Long` 这几种包装类型，它们都有缓存机制。在一定范围内的值会被缓存起来，当自动装箱时，如果值在缓存范围内，会直接返回缓存中的对象，而不是创建新的对象。例如，`Integer` 的缓存范围是 -128 到 127，当自动装箱的值在这个范围内时，会使用缓存中的 `Integer` 对象。

### == 和 equals 的区别

- **==**：可以比较基本类型的值，不可以比较基本类型的地址，不可以比较引用类型的值可以比较引用类型的地址。
- **equals()**：没有重写时与 `==` 相同，重写后仅可以比较对象内容是否相等。

### Object 类中的方法

1. **toString()**：返回该对象的字符串表示。
2. **hashCode()**：返回该对象的哈希码。
3. **getClass()**：通过调用该对象的 `getClass()` 方法，可以得到这个对象所属的类的相关信息，包括类的名称、父类、实现的接口、类中定义的方法和字段等。
4. **clone()**：克隆出一个对象，属性参数完全相同，除了地址。
5. **finalize()**：垃圾回收机制。
6. **wait()**：暂缓线程的执行。
7. **notify()**：唤醒一个正在等待的线程，加一个 `all` 就是唤醒等待的所有线程。

### 异常处理

Java 中的异常分为两大类，即 `Error` 和 `Exception`。

- **Error**：表示严重的错误，通常是由 Java 虚拟机内部的错误或资源耗尽等问题引起的，一般无法恢复，程序通常会终止运行。
- **Exception**：一般性的异常，可以分为运行时异常（`RuntimeException`）和非运行时异常（受检异常，`Checked Exception`）。

### 获取键盘输入

1. 引入 `Scanner` 类：`Scanner` 类位于 `java.util` 包中，因此需要在代码开头引入该包：`import java.util.Scanner;`
2. 创建 `Scanner` 对象：在需要获取键盘输入的地方，创建 `Scanner` 类的实例，它可以接收来自标准输入流（通常是键盘）的输入。例如：`Scanner scanner = new Scanner(System.in);`
3. 获取不同类型的输入：通过 `Scanner` 类提供的各种方法，可以获取不同类型的输入值。例如，使用 `nextInt()` 方法获取整数输入，`nextDouble()` 方法获取双精度浮点数输入，`next()` 方法获取字符串输入等。

### 抽象类与接口

- **抽象类**：一个类只能继承一个抽象类，使用 `extends` 关键字实现继承。如果子类不是抽象类，则必须实现抽象类中的所有抽象方法。
- **接口**：一个类可以实现多个接口，使用 `implements` 关键字实现接口。实现类必须实现接口中的所有抽象方法。
