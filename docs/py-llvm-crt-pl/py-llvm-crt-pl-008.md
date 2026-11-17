# 008：条件语句与布尔值 🚦


在本节课中，我们将为我们的编程语言添加条件判断能力。我们将实现 `if` 语句、条件运算符（如 `==`、`<`、`>=` 等）以及布尔字面量 `true` 和 `false`。课程结束时，我们将能够编译并运行包含条件逻辑的代码。


## 概述


我们将从扩展词法分析器开始，使其能识别新的关键字和运算符。接着，我们会更新抽象语法树（AST）和解析器，以构建条件语句的语法结构。最后，在编译器后端，我们将为这些新结构生成相应的 LLVM IR 代码，并实现布尔类型的全局常量。



---




## 1. 扩展词法分析器



首先，我们需要在 `Token` 类中添加新的词法单元类型，并在词法分析器中识别它们。




### 1.1 添加新的 Token 类型



在 `token.py` 文件中，我们需要添加以下新的词法单元类型：



*   **比较运算符**：`<`、`>`、`==`、`!=`、`<=`、`>=`
*   **关键字**：`if`、`else`
*   **布尔字面量**：`true`、`false`




以下是需要添加到 `TokenType` 枚举类中的代码：



```python
# 在 TokenType 枚举类中添加
LT = “LT”        # <
GT = “GT”        # >
EQ = “EQ”        # ==
NEQ = “NEQ”      # !=
LTE = “LTE”      # <=
GTE = “GTE”      # >=
IF = “IF”        # if
ELSE = “ELSE”    # else
TRUE = “TRUE”    # true
FALSE = “FALSE”  # false
```



### 1.2 更新关键字字典



在词法分析器中，我们需要将字符串关键字映射到对应的 `TokenType`。




```python
# 在 lexer.py 的关键字字典中添加
keywords = {
    “if”: TokenType.IF,
    “else”: TokenType.ELSE,
    “true”: TokenType.TRUE,
    “false”: TokenType.FALSE,
    # ... 其他已有关键字
}
```




### 1.3 识别比较运算符


在词法分析器的 `_next_token` 方法中，我们需要添加处理比较运算符的逻辑。这通常涉及查看下一个字符以区分单字符和双字符运算符（例如 `=` 和 `==`）。



以下是处理 `<` 和 `<=` 的示例：



```python
# 在 lexer.py 的 _next_token 方法中添加
elif self.current_char == ‘<’:
    if self.peek() == ‘=’:
        self.advance()
        token = Token(TokenType.LTE, ‘<=‘)
    else:
        token = Token(TokenType.LT, ‘<’)
```




你需要为 `>`、`=` 和 `!` 添加类似的逻辑，以处理 `>`、`>=`、`==` 和 `!=`。




---


## 2. 更新抽象语法树（AST）





接下来，我们需要定义两种新的 AST 节点：`IfStatement` 和 `BooleanLiteral`。




### 2.1 创建 IfStatement 节点




`IfStatement` 节点包含三个部分：条件表达式、条件为真时执行的代码块（consequence），以及可选的 `else` 代码块（alternative）。




```python
# 在 ast.py 中添加
class IfStatement(ASTNode):
    def __init__(self, condition, consequence, alternative=None):
        self.condition = condition
        self.consequence = consequence
        self.alternative = alternative
        self.node_type = “IfStatement”

    def to_json(self):
        return {
            “node”: self.node_type,
            “condition”: self.condition.to_json(),
            “consequence”: self.consequence.to_json(),
            “alternative”: self.alternative.to_json() if self.alternative else None
        }
```




### 2.2 创建 BooleanLiteral 节点




`BooleanLiteral` 节点非常简单，它只存储一个布尔值。




```python
# 在 ast.py 中添加
class BooleanLiteral(ASTNode):
    def __init__(self, value):
        self.value = value
        self.node_type = “BooleanLiteral”

    def to_json(self):
        return {
            “node”: self.node_type,
            “value”: self.value
        }
```




---


## 3. 更新解析器



现在，我们需要让解析器能够根据新的词法单元构建对应的 AST 节点。




### 3.1 添加运算符优先级


在解析器中，我们需要为比较运算符设置优先级，以确保表达式被正确解析。





```python
# 在 parser.py 的 PRECEDENCE 字典中添加
PRECEDENCE = {
    TokenType.EQ: 10,   # ==
    TokenType.NEQ: 10,  # !=
    TokenType.LT: 20,   # <
    TokenType.GT: 20,   # >
    TokenType.LTE: 20,  # <=
    TokenType.GTE: 20,  # >=
    # ... 其他已有优先级
}
```


### 3.2 解析前缀表达式




我们需要注册处理 `if`、`true` 和 `false` 前缀的函数。




```python
# 在 parser.py 的 PREFIX_PARSERS 字典中添加
PREFIX_PARSERS = {
    TokenType.IF: parse_if_statement,
    TokenType.TRUE: parse_boolean,
    TokenType.FALSE: parse_boolean,
    # ... 其他已有解析器
}
```




### 3.3 实现 parse_if_statement 函数



这个函数负责解析 `if` 语句的完整结构。




```python
def parse_if_statement(self):
    self.advance()  # 跳过 ‘if’ token
    condition = self.parse_expression(PRECEDENCE[TokenType.LOWEST])
    self.expect_peek(TokenType.LBRACE)  # 期望 ‘{‘
    consequence = self.parse_block_statement()

    alternative = None
    if self.peek_token.type == TokenType.ELSE:
        self.advance()  # 跳过 ‘else’ token
        self.expect_peek(TokenType.LBRACE)  # 期望 ‘{‘
        alternative = self.parse_block_statement()

    return IfStatement(condition, consequence, alternative)
```



### 3.4 实现 parse_boolean 函数


这个函数根据当前词法单元返回一个 `BooleanLiteral` 节点。




```python
def parse_boolean(self):
    value = self.current_token.type == TokenType.TRUE
    return BooleanLiteral(value)
```


---



## 4. 更新编译器后端




最后，我们需要在编译器后端为新的 AST 节点生成 LLVM IR 代码。




### 4.1 添加布尔类型映射



首先，在编译器的类型映射中注册布尔类型。在 LLVM IR 中，布尔类型通常用 `i1`（1位整数）表示。




```python
# 在 compiler.py 的 __init__ 方法中添加
self.type_map[“bool”] = ir.IntType(1)
```



### 4.2 初始化内置布尔常量



我们需要创建全局的 `true` 和 `false` 常量，并将它们添加到符号表中，使其成为语言中的保留关键字。



```python
def _initialize_builtins(self):
    bool_type = self.type_map[“bool”]

    # 创建全局常量 true
    true_var = ir.GlobalVariable(self.module, bool_type, name=“true”)
    true_var.initializer = ir.Constant(bool_type, 1)
    true_var.global_constant = True

    # 创建全局常量 false
    false_var = ir.GlobalVariable(self.module, bool_type, name=“false”)
    false_var.initializer = ir.Constant(bool_type, 0)
    false_var.global_constant = True

    # 将 true 和 false 添加到符号表
    self.env.define(“true”, true_var, bool_type)
    self.env.define(“false”, false_var, bool_type)
```




确保在编译器初始化时调用 `_initialize_builtins()` 方法。




### 4.3 编译 BooleanLiteral 节点



当遇到布尔字面量时，我们直接从符号表中加载对应的全局常量。




```python
def visit_boolean_literal(self, node):
    # 根据节点值决定加载 true 还是 false 常量
    var_name = “true” if node.value else “false”
    var = self.env.lookup(var_name)
    return self.builder.load(var, name=var_name)
```



### 4.4 编译比较运算符



我们需要为整数和浮点数实现比较运算符的代码生成。这通常使用 LLVM 的 `icmp`（整数比较）和 `fcmp`（浮点数比较）指令。



以下是整数相等比较 `==` 的示例：



```python
def visit_infix_expression(self, node):
    left = self.visit(node.left)
    right = self.visit(node.right)

    if node.operator == “==”:
        if isinstance(left.type, ir.IntType) and isinstance(right.type, ir.IntType):
            # 整数比较
            return self.builder.icmp_signed(‘==’, left, right, name=“cmptmp”)
        elif isinstance(left.type, ir.FloatType) and isinstance(right.type, ir.FloatType):
            # 浮点数比较
            return self.builder.fcmp_ordered(‘==’, left, right, name=“fcmp”)
    # … 处理其他运算符 <, >, !=, <=, >=
```


你需要为每个比较运算符（`<`、`>`、`!=`、`<=`、`>=`）实现类似的逻辑。



### 4.5 编译 IfStatement 节点



这是最核心的部分。我们使用 LLVM 的 `if_then` 或 `if_else` 构建器方法来创建条件分支。



```python
def visit_if_statement(self, node):
    condition = self.resolve_value(node.condition)  # 计算条件表达式的值
    bool_type = self.type_map[“bool”]
    # 确保条件结果是布尔类型（i1）
    if condition.type != bool_type:
        condition = self.builder.icmp_signed(‘!=’, condition, ir.Constant(condition.type, 0))

    if node.alternative is None:
        # 只有 if，没有 else
        with self.builder.if_then(condition):
            self.visit(node.consequence)
    else:
        # 有 if 和 else
        with self.builder.if_else(condition) as (then, otherwise):
            with then:
                self.visit(node.consequence)
            with otherwise:
                self.visit(node.alternative)
```


`resolve_value` 方法用于确保表达式的结果是一个可以用于条件判断的值。

---

## 5. 测试与验证




完成所有代码后，我们可以编写一个测试程序来验证功能。


```python
# test.lang
let a = 5;
if (a == 5) {
    a = 69;
} else {
    a = 420;
}
return a;
```




运行编译器，你应该得到正确的 LLVM IR 代码。最终程序应返回 `69`。如果将 `let a = 5;` 改为 `let a = 1;`，程序应返回 `420`。




---




## 总结

在本节课中，我们一起为我们的编程语言添加了核心的条件逻辑功能。我们：



1.  **扩展了词法分析器**，使其能识别 `if`、`else`、`true`、`false` 关键字以及各种比较运算符。
2.  **更新了 AST**，定义了 `IfStatement` 和 `BooleanLiteral` 节点。
3.  **增强了解析器**，使其能够解析条件语句的语法结构。
4.  **实现了编译器后端**，为条件语句和布尔值生成正确的 LLVM IR 代码，包括创建全局布尔常量和处理条件分支。

![](img/1f8d0b70015acef737e03658515eb7f9_286.png)

现在，我们的语言已经具备了基本的流程控制能力。在下一节课中，我们将探索函数调用，让我们的程序能够执行更复杂的操作。