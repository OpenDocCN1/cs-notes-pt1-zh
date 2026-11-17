# 010：Pass基础设施 🛠️





在本节课中，我们将要学习MLIR中Pass（通行证）的基础设施。Pass是编译器中执行转换和优化的核心单元。理解Pass的工作原理是后续进行方言降级（Lowering）的关键前提。


## 概述




上一节我们简要介绍了如何在MLIR中创建一个新的方言（SLIR），并使用它生成中间表示（IR）。这是进入MLIR世界的第一步。作为下一步，我们需要使用这个方言，并将其降级到MLIR内置的其他方言，甚至直接降级到LLVM IR。




但在进行降级之前，我们需要更多地了解Pass基础设施。因此，本节课将专注于讲解Pass。



## 什么是Pass？🤔





Pass是LLVM和MLIR中都有的一个概念，它是两者中进行转换和优化的基本单位。虽然两者都有Pass的概念且非常相似（API几乎相同），但由于MLIR是一种更高级的IR，可以包含LLVM IR所没有的操作和结构，这使得MLIR中的Pass与LLVM中的略有不同。但其核心概念是相同的：**Pass是一个用于将一种形式的IR转换或优化为另一种形式的实体**。




今天我们将主要讨论MLIR中的Pass，但所讲的大部分内容也适用于LLVM Pass。





## 编译器中的Pass





如果我们观察任何编译器，其主要工作流程是：读取代码 -> 进行处理 -> 生成结果。这个“进行处理”的过程，从宏观上看很简单：读取文件，将源代码转换为抽象语法树（AST），再将AST转换为某种IR，如此循环，直到得到目标代码。





显然，整个编译过程可以分解为更小的逻辑片段。我们可以将其抽象地视为一个**函数管道（Pipeline）或函数组合**。每个函数负责读取前一个函数的输出作为输入，进行一些优化或转换，生成新的输出，并将其传递给下一个函数。



基本上，我们现在讨论的每个封装了特定逻辑的抽象单元，就类似于Pass。因此，**Pass是MLIR和LLVM中在IR层面进行操作、读取相应IR、进行转换并生成新IR的抽象**。





为了分类和组合这些Pass，我们需要**Pass管理器（Pass Manager）**，或者我喜欢称之为**管道（Pipeline）**（MLIR文档有时也这样称呼）。我们可以用不同的Pass创建转换管道，每个Pass都包含对IR进行某种转换的逻辑。我们可以将它们分组到Pass管理器中，创建管道，甚至可以嵌套多个管道以实现复杂的转换流程。






## Pass的类型与创建




在MLIR中，Pass作用于操作（Operation）层面。操作是转换的主要抽象单元。一个Pass一次操作一个操作（取决于Pass类型）。我们不能编写作用于特定属性或跨操作的Pass。





创建Pass主要有两种方式：纯C++或通过ODS（操作描述规范）。目前Serene编译器中的所有Pass都是专门用于将SLIR和其他方言相互降级或降级到LLVM IR的，因此尚未使用ODS。但未来使用ODS编写Pass可能会比C++更方便，因为它能避免大量样板代码。





### 操作特定Pass（Operation-Specific Pass）






这种Pass只针对特定类型的操作。以下是一个定义操作特定Pass的示例（取自官方文档）：




```cpp
class MyFunctionPass : public PassWrapper<MyFunctionPass, OperationPass<FuncOp>> {
  void runOnOperation() override {
    // 获取当前操作，这里保证是FuncOp类型
    FuncOp op = getOperation();
    // 可以遍历操作内部的嵌套操作并进行转换
    op.walk([&](Operation *nestedOp) {
      // 对嵌套操作进行处理
    });
  }
};
```





在这个例子中，`MyFunctionPass` 只作用于 `FuncOp`（函数操作）。当Pass管理器在IR上应用Pass时，每当遇到一个 `FuncOp`，就会对这个操作应用此Pass。




### 操作无关Pass（Operation-Agnostic Pass）





这种Pass可以运行在任何操作上，其逻辑不针对特定操作。定义方式与操作特定Pass类似，区别在于不传递模板参数来指定操作类型。





```cpp
class MyGenericPass : public PassWrapper<MyGenericPass, OperationPass<>> {
  void runOnOperation() override {
    // 获取当前操作，这是一个通用的Operation指针
    Operation *op = getOperation();
    // 在此进行转换
  }
};
```



### Pass的规则




Pass需要遵循一系列规则，主要是为了适应MLIR的多线程执行模型，避免数据竞争和不必要的问题。例如：
*   Pass不应拥有全局可变状态。
*   Pass不应修改其当前正在处理的操作之外的其他操作的状态（除非该操作嵌套在当前操作内）。
建议查阅官方文档以了解完整的规则列表。





### Pass注册



注册Pass不是强制性的。注册的主要目的是允许通过命令行接口（CLI）以文本形式构造和调用特定的Pass。在Serene编译器中，由于现有的Pass都是降级所必需的，因此没有注册它们。但注册Pass的一个常见用例是：将一组Pass分组（例如，用于优化级别O2），然后通过一个CLI标志启用整个Pass管理器，这非常方便。


## Pass管理器与管道




Pass管理器用于组织和运行Pass管道。以下是如何定义和使用Pass管理器的示例（基于官方文档）：












```cpp
// 创建一个顶层的Pass管理器，作用于MLIR模块（ModuleOp）
PassManager pm(ctx);
pm.addPass(std::make_unique<MyModulePass>());
// 创建一个嵌套的Pass管理器，仅作用于特定方言的模块操作
OpPassManager &nestedModulePM = pm.nest<ModuleOp>();
nestedModulePM.addPass(std::make_unique<MyDialectModulePass>());
// 进一步嵌套，创建一个作用于函数操作的Pass管理器
OpPassManager &nestedFunctionPM = nestedModulePM.nest<FuncOp>();
nestedFunctionPM.addPass(std::make_unique<MyFunctionPass>());
// 获取要转换的MLIR模块（例如 `moduleOp`）
// 运行Pass管道，转换是原地（in-place）进行的
if (failed(pm.run(moduleOp))) {
    // 处理失败情况
}
```





通过这种方式，我们创建了一个Pass管理器的树形结构（管道树），每一层针对IR树的不同层级进行操作。最终，顶层的Pass管理器会在模块上启动，依次应用所有Pass。



## 其他相关实体





除了Pass，Pass基础设施中还有其他重要实体：
*   **分析（Analysis）**：类似于Pass，但不进行转换。它们的主要目的是收集关于IR的度量和数据，Pass可以查询这些分析结果来辅助其转换逻辑。
*   **Pass仪器（Pass Instrumentation）**：允许我们钩入（Hook into）Pass基础设施。它提供了一些钩子函数（例如，在运行一个Pass之前/之后，在开始一个管道之前/之后），我们可以在这里运行自定义代码（例如，打印每次Pass转换后的IR）。MLIR本身就提供了一些很好的相关功能。





## 总结




本节课我们一起学习了MLIR中Pass基础设施的核心概念。我们了解了Pass是编译器中进行IR转换和优化的基本单元，以及它在编译管道中的作用。我们探讨了两种主要的Pass类型（操作特定和操作无关），学习了创建Pass的基本方法，并了解了如何使用Pass管理器来组织和运行复杂的转换管道。此外，我们还简要介绍了分析与Pass仪器。

![](img/175e489c9e191d7e5631039ecf128480_197.png)

掌握这些知识是下一节学习**方言降级（Lowering）** 的关键基础。在下一节课中，我们将看到如何具体应用Pass来将我们的SLIR方言逐步降级到LLVM IR。