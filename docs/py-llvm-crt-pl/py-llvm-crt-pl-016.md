# 016：文件导入 📂




在本节课中，我们将为我们的编程语言实现一个基础的文件导入功能。我们将学习如何解析 `import` 关键字，读取外部文件，并将其中的代码编译到当前环境中。


---




## 概述




上一节我们完成了语言的核心功能。本节中，我们将实现类似 C 语言风格的基础文件导入功能。我们将使用 `import` 关键字，后跟一个包含文件路径的字符串，来导入其他文件中的函数和变量。



## 1. 更新词法分析器（Token）




首先，我们需要在词法分析器中添加对 `import` 关键字的支持。



在 `token.py` 文件的 `TokenType` 枚举中，添加 `IMPORT` 类型：
```python
class TokenType(Enum):
    ...
    IMPORT = 'IMPORT'
```





接着，在关键字映射字典中，将 `"import"` 映射到 `TokenType.IMPORT`。同时，为了增加趣味性，我们也可以为其添加一个别名 `"gib"`。
```python
KEYWORDS = {
    "import": TokenType.IMPORT,
    "gib": TokenType.IMPORT,
    ...
}
```




## 2. 更新抽象语法树（AST）




接下来，我们需要定义一种新的 AST 节点来表示导入语句。





在 `ast.py` 文件的节点类型枚举中，添加 `ImportStatement`：
```python
class NodeType(Enum):
    ...
    IMPORT_STATEMENT = 'IMPORT_STATEMENT'
```





然后，在语句节点区域的下方，创建 `ImportStatement` 类。这个节点只需要一个属性：`file_path`。
```python
@dataclass
class ImportStatement(Node):
    file_path: str

    def __post_init__(self):
        self.node_type = NodeType.IMPORT_STATEMENT
```




## 3. 更新语法分析器（Parser）





现在，我们需要让语法分析器能够识别并解析 `import` 语句。



首先，在 `parser.py` 文件顶部导入新创建的 `ImportStatement` 节点。




然后，在 `parse_statement` 函数中添加一个新的条件分支，用于处理 `TokenType.IMPORT` 令牌：
```python
def parse_statement(self):
    if self.current_token.type == TokenType.IMPORT:
        return self._parse_import_statement()
    ...
```




最后，在语句解析方法块的底部（例如在 `_parse_for_statement` 之后），实现 `_parse_import_statement` 函数：
```python
def _parse_import_statement(self) -> Optional[ImportStatement]:
    # 确保下一个令牌是字符串（文件路径）
    if not self._expect_peek(TokenType.STRING):
        return None

    stmt = ImportStatement(file_path=self.current_token.literal)

    # 确保语句以分号结尾
    if not self._expect_peek(TokenType.SEMICOLON):
        return None

    return stmt
```




## 4. 更新编译器（Compiler）




最后，也是最关键的一步，是在编译器中实现导入语句的逻辑，使其能够读取外部文件并编译其中的代码。





首先，在 `compiler.py` 文件顶部进行必要的导入：
```python
import os
from lexer import Lexer
from parser import Parser
from ast import ImportStatement
```




在编译器的 `__init__` 方法中，添加一个字典来缓存已解析的模块，避免重复导入：
```python
def __init__(self):
    ...
    self.global_parsed_pallets: Dict[str, Program] = {}
```




在 `compile` 方法的 `match` 语句中，为 `NodeType.IMPORT_STATEMENT` 添加一个新的分支，调用 `_visit_import_statement` 函数。





在 `_visit_for_statement` 函数下方，实现 `_visit_import_statement` 函数：
```python
def _visit_import_statement(self, node: ImportStatement) -> None:
    file_path = node.file_path

    # 检查是否已导入过该文件
    if self.global_parsed_pallets.get(file_path) is not None:
        print(f"Warning: File '{file_path}' is already imported.")
        return

    # 构建完整的文件路径并读取内容
    full_path = f"tests/{file_path}"
    try:
        with open(full_path, 'r') as f:
            pallet_code = f.read()
    except FileNotFoundError:
        print(f"Error: Could not find file '{full_path}'")
        exit(1)

    # 对导入的代码进行词法分析和语法分析
    l = Lexer(source=pallet_code)
    p = Parser(lexer=l)
    program = p.parse_program()

    # 检查分析过程中是否有错误
    if len(p.errors) > 0:
        print(f"Error parsing imported pallet '{file_path}':")
        for error in p.errors:
            print(error)
        exit(1)

    # 编译导入的代码（函数和变量将注册到当前作用域）
    self.compile(program)

    # 将解析后的 AST 缓存起来
    self.global_parsed_pallets[file_path] = program
```




## 测试导入功能



现在，我们可以测试导入功能了。假设我们有一个 `math.lime` 文件，内容如下：
```lime
func add(a: int, b: int) -> int {
    return a + b;
}
```




在 `test.lime` 文件中，我们可以导入并使用它：





```lime
import "math.lime";
for i in 0..10 {
    if i == 1 {
        print(i, " + 8 = ", add(i, 8));
    }
}
```




运行主程序，如果一切正常，你将看到输出 `1 + 8 = 9`，这证明 `add` 函数已从 `math.lime` 文件成功导入并执行。



---


## 总结


本节课中，我们一起为我们的编程语言实现了基础的文件导入功能。我们完成了以下工作：
1.  在词法分析器中添加了 `import` 关键字。
2.  在抽象语法树中定义了 `ImportStatement` 节点。
3.  在语法分析器中实现了导入语句的解析逻辑。
4.  在编译器中实现了读取外部文件、分析其代码、并将其编译到当前环境的核心功能。

这个实现是一个全局化的导入系统，它允许你在一个文件中使用另一个文件中定义的函数。你可以在此基础上进行扩展，例如添加相对路径支持、模块化命名空间等。

![](img/6af1ed99976df5c30c5bbff0fda916f2_166.png)

下一节（第17集），我们将学习如何将整个语言导出为一个可执行文件（EXE），以便将其添加到系统路径中，像 Python 或 Go 一样安装和使用。