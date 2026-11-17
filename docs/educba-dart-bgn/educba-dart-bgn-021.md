# 021：列表与字符串操作


在本节课中，我们将学习Dart中字符串的常用操作方法，包括字符串连接、大小写转换、修剪空格、比较、替换和子字符串提取。最后，我们将初步了解Dart中的列表概念。




## 字符串连接与用户输入




上一节我们介绍了变量的基本使用，本节中我们来看看如何接收用户输入并进行字符串操作。



首先，我们创建一个程序来接收用户的名字和地址，并使用加号（`+`）运算符连接字符串。




```dart
import 'dart:io';
void main() {
  String fName;
  String lName;
  String address;

  print('请输入您的名字：');
  fName = stdin.readLineSync()!;

  print('请输入您的姓氏：');
  lName = stdin.readLineSync()!;

  print('请输入您的地址：');
  address = stdin.readLineSync()!;

  print('您的姓名是：' + fName + lName);
  print('您的地址是：' + address);
}
```

运行上述代码会发现，输出的姓名中名字和姓氏连在一起，没有空格。这是因为`+`运算符只是简单地将字符串拼接起来。



为了解决这个问题，我们需要在连接时手动添加一个空格。

```dart
print('您的姓名是：' + fName + ' ' + lName);
```

现在，输出的姓名中名字和姓氏之间就有了空格。



## 常用的字符串方法

除了连接，Dart还为字符串提供了多种实用的方法。以下是一些核心方法的介绍。

以下是几个关键的字符串方法及其用法：



1.  **`toLowerCase()`**: 将字符串中的所有字符转换为小写。
    ```dart
    String name = fName + ' ' + lName;
    print(name.toLowerCase()); // 输出小写姓名
    ```


2.  **`toUpperCase()`**: 将字符串中的所有字符转换为大写。
    ```dart
    print(name.toUpperCase()); // 输出大写姓名
    ```



3.  **`trim()`**: 移除字符串开头和结尾的所有空白字符（如空格、制表符）。
    ```dart
    String nameWithSpaces = '  $name  ';
    print(nameWithSpaces.trim()); // 输出去除首尾空格的姓名
    ```



4.  **`compareTo()`**: 比较两个字符串。如果相等返回`0`，否则返回非零值（通常是`1`或`-1`）。
    ```dart
    print(name.compareTo('Prianka')); // 比较姓名与“Prianka”
    ```

5.  **`replaceAll()`**: 将字符串中所有匹配指定模式的部分替换为新的子串。
    ```dart
    print(name.replaceAll('ya', 'yaa')); // 将所有的“ya”替换为“yaa”
    ```


6.  **`substring()`**: 从字符串中提取指定起始索引（包含）到结束索引（不包含）的子串。如果只提供一个参数，则提取到字符串末尾。
    ```dart
    print(name.substring(4)); // 从索引4（第5个字符）开始提取到末尾
    ```

## 综合示例程序


让我们将以上所有方法整合到一个完整的示例程序中。




```dart
import 'dart:io';
void main() {
  String fName;
  String lName;
  String address;

  print('请输入您的名字：');
  fName = stdin.readLineSync()!;

  print('请输入您的姓氏：');
  lName = stdin.readLineSync()!;

  print('请输入您的地址：');
  address = stdin.readLineSync()!;

  String fullName = fName + ' ' + lName;

  print('您的姓名是：$fullName');
  print('您的地址是：$address');
  print('小写姓名：${fullName.toLowerCase()}');
  print('大写姓名：${fullName.toUpperCase()}');
  print('修剪后姓名：“${fullName.trim()}”');
  print('与“Prianka”比较：${fullName.compareTo(“Prianka”)}');
  print('替换“ya”为“yaa”：${fullName.replaceAll(“ya”, “yaa”)}');
  print('从索引4开始的子串：${fullName.substring(4)}');
}
```

运行此程序，输入“Prianka”、“Sharma”和一个地址，可以看到各种字符串方法的效果。`trim()`方法会移除首尾空格，但不会移除中间的空格。`compareTo()`在完全匹配时返回`0`。`substring(4)`会从字符串的第5个字符（索引从0开始）开始截取。



## 引入列表概念

在学习了数字和字符串的操作后，我们常常需要处理一组相关的数据。Dart中的**列表（List）** 就是一种用于存储有序元素集合的对象。

列表允许我们将多个值（可以是同类型或不同类型）组合到一个变量中，并通过索引来访问它们。这为管理数据集合提供了极大的便利。



![](img/dc3e5f6b83b3f0ed6b5504728d3c5cd7_49.png)

本节课中我们一起学习了Dart中字符串的核心操作方法，包括使用`+`进行连接，以及`toLowerCase()`、`toUpperCase()`、`trim()`、`compareTo()`、`replaceAll()`和`substring()`等内置方法。我们还初步接触了**列表（List）**的概念，它是组织多个数据元素的强大工具。掌握这些基础知识是进行更复杂Dart编程的关键步骤。