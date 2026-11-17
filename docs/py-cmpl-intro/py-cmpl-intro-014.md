# 014：电子邮件切片器练习 📧

在本节课中，我们将创建一个电子邮件切片器程序。这个程序将演示如何利用字符串索引和切片功能，将一个完整的电子邮件地址拆分为用户名和域名两部分。这是一个非常实用的练习，能帮助你巩固对字符串操作的理解。


## 概述


我们将编写一个Python程序，接收用户输入的电子邮件地址，然后自动提取出其中的用户名和域名。核心步骤包括：获取用户输入、定位“@”符号的位置、使用字符串切片功能进行分割。


## 程序构建步骤

### 1. 获取用户输入


首先，我们需要获取用户输入的电子邮件地址。我们将使用 `input()` 函数来实现。


```python
email = input("Enter your email: ")
```



运行这行代码时，程序会显示提示信息“Enter your email:”，并等待用户输入。用户输入的电子邮件地址将被存储在名为 `email` 的变量中。



### 2. 定位“@”符号

为了分割电子邮件，我们需要知道“@”符号在字符串中的位置。字符串对象有一个内置的 `index()` 方法，可以返回指定字符第一次出现的索引位置。

```python
at_index = email.index('@')
```




这里，`email.index('@')` 会返回“@”符号在 `email` 字符串中的索引号，并将其赋值给变量 `at_index`。



### 3. 分割用户名和域名


有了“@”符号的位置，我们就可以使用字符串切片来提取用户名和域名了。


*   **提取用户名**：用户名是从字符串开头到“@”符号之前的部分。
    ```python
    username = email[:at_index]
    ```
    切片 `[:at_index]` 表示从索引0开始（可省略），到 `at_index` 结束（不包含该索引的字符）。


*   **提取域名**：域名是从“@”符号之后到字符串结尾的部分。为了不包含“@”符号本身，起始索引需要是 `at_index + 1`。
    ```python
    domain = email[at_index + 1:]
    ```
    切片 `[at_index + 1:]` 表示从 `at_index` 的下一个字符开始，一直到字符串末尾。


### 4. 输出结果

最后，我们使用f-string格式化字符串来清晰地输出结果。



```python
print(f"Your username is {username} and domain is {domain}.")
```


## 代码优化


上面的代码分步清晰，易于理解。我们也可以将步骤合并，写出更简洁（但可能稍难阅读）的代码：



```python
email = input("Enter your email: ")
username = email[:email.index('@')]
domain = email[email.index('@') + 1:]
print(f"Your username is {username} and domain is {domain}.")
```


这种方法减少了变量 `at_index` 的定义，直接将 `email.index('@')` 的调用嵌入到切片操作中。两种方式在功能上是完全等效的，你可以根据对代码可读性的偏好进行选择。




## 程序运行示例

假设我们运行程序并输入电子邮件 `bro123@gmail.com`。

1.  程序提示：`Enter your email:`
2.  用户输入：`bro123@gmail.com`
3.  程序计算：
    *   `email.index('@')` 返回 6（“@”在字符串中的位置）。
    *   `username = email[:6]` 得到 `"bro123"`。
    *   `domain = email[7:]` 得到 `"gmail.com"`。
4.  程序输出：`Your username is bro123 and domain is gmail.com.`

## 总结



本节课我们一起学习了如何构建一个电子邮件切片器。通过这个练习，我们实践了以下几个核心Python概念：
*   使用 `input()` 函数获取用户输入。
*   使用字符串的 `index()` 方法查找特定字符的位置。
*   使用字符串切片操作 `[start:end]` 来提取子字符串。
*   使用f-string进行格式化的输出。



![](img/7146291205a13f306b1523a6127ad869_50.png)

这个程序虽然简单，但很好地串联了字符串处理的基础知识，是巩固学习成果的绝佳练习。