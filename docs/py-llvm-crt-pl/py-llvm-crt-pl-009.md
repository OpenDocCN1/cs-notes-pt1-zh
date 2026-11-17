# 009：函数调用 🧩


在本节课中，我们将学习如何为我们的编程语言实现函数调用功能。我们将创建一个新的抽象语法树节点来处理函数调用，并更新解析器和编译器来支持它。最终，我们将能够编译并运行一个简单的程序，其中主函数调用另一个函数并返回其结果。

---




## 概述


上一节我们实现了函数定义。本节中，我们来看看如何调用这些函数。我们将通过实现一个新的“调用表达式”AST节点，并在解析器中识别左括号`(`作为函数调用的标志来完成此功能。目前，我们暂不支持传递参数，这将是下一节的内容。


## 实现调用表达式 AST 节点




首先，我们需要在抽象语法树中定义一个新的节点类型来表示函数调用。



以下是创建 `CallExpression` 类的代码：





```python
class CallExpression(Expression):
    def __init__(self, function, arguments):
        self.function = function  # 一个标识符字面量，表示函数名
        self.arguments = arguments  # 参数表达式列表（目前为空）
        self.type = NodeType.CALL_EXPRESSION

    def to_json(self):
        return {
            "type": self.type.value,
            "function": self.function.to_json(),
            "arguments": [arg.to_json() for arg in self.arguments]
        }
```



这个节点包含两个主要部分：要调用的函数名（一个标识符）和一个参数列表（目前是空列表）。



## 更新解析器以识别函数调用




接下来，我们需要修改解析器，使其在遇到标识符后的左括号`(`时，能将其解析为函数调用，而不是其他操作（比如乘法）。



以下是需要更新的解析器部分：




1.  在优先级映射表中，为左括号`(`令牌关联`CALL`优先级。
2.  添加一个新的中缀解析函数 `parse_call_expression`。



首先，在优先级映射中添加条目：


```python
# 在 parser 类的 __init__ 方法或类似位置
self.precedence_mapping = {
    ...
    TokenType.LEFT_PAREN: Precedence.CALL,
}
```



然后，添加中缀解析函数：


```python
def parse_call_expression(self, function):
    """解析函数调用表达式"""
    # 创建调用表达式节点，目前参数列表为空
    expr = CallExpression(function, [])
    # 期望下一个令牌是右括号，目前不支持参数
    if not self._expect_peek(TokenType.RIGHT_PAREN):
        return None
    return expr
```




最后，将这个函数注册到中缀解析函数映射中：



```python
self.infix_parse_functions = {
    ...
    TokenType.LEFT_PAREN: self.parse_call_expression,
}
```





现在，解析器就能将类似 `test()` 的结构识别为函数调用了。



## 更新编译器以生成调用代码



解析器生成AST后，编译器需要知道如何将 `CallExpression` 节点转换为LLVM IR代码。



以下是编译器中需要添加的 `visit_call_expression` 方法：


```python
def visit_call_expression(self, node):
    """编译函数调用表达式"""
    # 获取函数名
    func_name = node.function.value
    # 获取参数列表（目前为空）
    args = node.arguments

    # 从当前作用域中查找函数定义
    func, return_type = self.env.lookup(func_name)

    # 使用LLVM构建器生成函数调用指令
    return_value = self.builder.call(func, [])
    # 返回调用结果
    return return_value, return_type
```



同时，需要在 `resolve_value` 方法中添加对 `CALL_EXPRESSION` 节点的处理，以调用上述访问方法。



## 测试函数调用功能




完成以上步骤后，我们可以编写一个简单的测试程序。



测试代码示例：
```
function test() {
    return 69;
}

function main() {
    return test(); // 调用 test 函数
}
```



运行编译器后，程序应成功执行并返回 `69`。这证明我们的函数调用机制已基本实现。


## 总结

本节课中我们一起学习了如何实现编程语言中的函数调用功能。我们创建了 `CallExpression` AST节点，更新了解析器来识别调用语法，并修改了编译器来生成调用其他函数所需的LLVM IR代码。目前，函数调用还不支持参数，这是我们接下来要解决的问题。

![](img/d33bc1c40be3f2bd2c963d796c889bd8_119.png)

在下一节中，我们将为函数添加参数和实参传递的功能，使函数变得更加实用和强大。