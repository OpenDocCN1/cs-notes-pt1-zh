# 006：Dart变量与数据类型 📚


在本节课中，我们将要学习Dart编程语言中的两个核心概念：**关键字**和**变量数据类型**。我们将首先了解哪些是Dart中的保留关键字，然后深入探讨如何使用不同的数据类型来声明和存储变量。



## 关键字列表 🚫



上一节我们介绍了标识符的命名规则，本节中我们来看看在Dart中哪些词是**保留关键字**，不能用作标识符名称。


以下是Dart中的关键字列表，应避免使用它们作为变量名、函数名等标识符：



*   abstract
*   as
*   assert
*   async
*   await
*   break
*   case
*   catch
*   class
*   const
*   continue
*   covariant
*   default
*   deferred
*   do
*   dynamic
*   else
*   enum
*   export
*   extends
*   extension
*   external
*   factory
*   false
*   final
*   finally
*   for
*   Function
*   get
*   hide
*   if
*   implements
*   import
*   in
*   interface
*   is
*   library
*   mixin
*   new
*   null
*   on
*   operator
*   part
*   required
*   rethrow
*   return
*   set
*   show
*   static
*   super
*   switch
*   sync
*   this
*   throw
*   true
*   try
*   typedef
*   var
*   void
*   while
*   with
*   yield


此外，还有一些**上下文关键字**和**内置标识符**，它们只在特定位置有特殊含义，同样不建议用作普通标识符。


## 变量与数据类型 🔢



现在，让我们继续学习Dart中的各种变量及其数据类型。之前我们已经使用 `var` 创建过变量，但Dart也支持明确指定数据类型的变量声明，这可以存储特定类型的值。



以下是Dart中主要的数据类型：

*   **数字**：用于存储数值。
*   **字符串**：用于存储一系列字符。
*   **布尔值**：用于存储 `true` 或 `false`。
*   **列表**：用于存储一组有序的值。
*   **映射**：用于存储键值对。
*   **符文**：用于表示字符串中的Unicode代码点。
*   **符号**：一种不常用的数据类型。

接下来，我们将逐一详细了解这些数据类型。



### 数字类型

顾名思义，数字类型用于存储任何数值或整数数据。数值可以是整数，也可以是浮点数。



**创建数字变量：**

*   **整数**：使用 `int` 关键字声明一个存储整数的变量。
    ```dart
    int age = 25;
    ```
*   **浮点数**：使用 `double` 关键字声明一个存储双精度浮点数的变量。
    ```dart
    double salary = 50000.75;
    ```



### 字符串类型

字符串类型用于存储一系列字符序列，可以包含字母、数字或特殊字符。任何你想以文本格式存储的内容都可以使用字符串变量。




**创建字符串变量：**


通常使用 `String` 关键字或 `var` 进行声明。
```dart
String address = ‘123 Main Street, City’;
```

### 布尔类型

布尔类型用于存储逻辑值，只能是 `true` 或 `false`。在计算机内部，它们通常用 `1` 表示 `true`，`0` 表示 `false`。

**创建布尔变量：**

使用 `bool` 关键字进行声明。
```dart
bool isEmployeeActive = true;
```





### 列表类型


列表类型变量用于存储一组对象的集合，是一个有序的值组。


**创建列表变量：**



使用 `List` 关键字声明，并将值放在方括号 `[]` 中，值之间用逗号分隔。
```dart
List<int> oddNumbers = [1, 3, 5, 7, 9];
```

### 映射类型

映射类型用于存储键值对。每个键都映射到一个特定的值。



**创建映射变量：**

使用 `Map` 关键字声明，并将键值对放在花括号 `{}` 中。键和值之间用冒号 `:` 分隔，不同的键值对用逗号分隔。
```dart
Map<String, String> department = {
  ‘department1’: ‘Accounts’,
  ‘department2’: ‘Sales’
};
```

## 总结 📝


![](img/3f20b337152865f062f0c1d8f5b04804_57.png)

本节课中我们一起学习了Dart编程基础中关于变量声明的重要知识。我们首先明确了Dart语言中的**保留关键字**列表，这些词不能用作标识符。接着，我们深入探讨了Dart的各种**数据类型**，包括用于存储数值的`int`和`double`，用于存储文本的`String`，用于存储真假的`bool`，用于存储集合的`List`，以及用于存储键值对的`Map`，并学习了它们的基本声明语法。理解这些基础概念是编写正确、高效Dart代码的关键第一步。