# 016：游戏优化（5倍性能提升）🚀

![](img/d2b281400e0d90c00f492f0adbc6943a_1.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_3.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_5.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_7.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_9.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_11.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_13.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_15.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_17.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_19.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_21.png)

## 概述

![](img/d2b281400e0d90c00f492f0adbc6943a_23.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_25.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_27.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_29.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_31.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_33.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_35.png)

在本节课中，我们将学习如何分析和优化游戏的性能。我们将从一个运行缓慢的菜单界面开始，通过构建性能分析器、理解代码瓶颈，并应用一系列优化技术，最终将性能提升5倍。我们将重点关注渲染函数，特别是旋转矩形和位图绘制，它们是性能的主要瓶颈。

![](img/d2b281400e0d90c00f492f0adbc6943a_37.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_39.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_41.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_43.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_45.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_47.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_49.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_51.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_53.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_55.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_57.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_59.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_61.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_63.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_65.png)

---

![](img/d2b281400e0d90c00f492f0adbc6943a_67.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_69.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_71.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_73.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_75.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_77.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_79.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_81.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_83.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_85.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_87.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_89.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_91.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_93.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_95.png)

## 当前状态与问题

![](img/d2b281400e0d90c00f492f0adbc6943a_97.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_99.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_100.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_102.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_104.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_106.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_108.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_110.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_112.png)

上一节课我们创建了一个带有各种动画的主菜单。然而，我们遇到了性能问题。在屏幕右上角，帧率计数器显示每帧耗时（毫秒）。在游戏中，我们运行得很快，但在菜单中，帧率却低得多，大约每秒11帧。

![](img/d2b281400e0d90c00f492f0adbc6943a_114.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_116.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_118.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_120.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_122.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_123.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_125.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_127.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_129.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_131.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_133.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_135.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_137.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_139.png)

此外，还存在一个碰撞问题，在低帧率下尤为明显。力场效果有时不会关闭，这影响了玩家的视觉和声音效果。

![](img/d2b281400e0d90c00f492f0adbc6943a_141.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_143.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_145.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_147.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_149.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_151.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_153.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_155.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_157.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_159.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_160.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_161.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_163.png)

## 修复已知问题

![](img/d2b281400e0d90c00f492f0adbc6943a_165.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_167.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_169.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_171.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_173.png)

在开始优化之前，我们先修复一些小问题。

![](img/d2b281400e0d90c00f492f0adbc6943a_175.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_177.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_179.png)

1.  **力场效果不关闭**：当切换到菜单时，我们应该重置力场。
    ```c
    // 切换到菜单模式时
    if (game_mode == MENU_MODE) {
        force_field_active = false;
    }
    ```

2.  **碰撞问题**：我们使用了一个“目标位置”进行碰撞检测，这可能导致一个球同时摧毁多个方块。为了解决这个问题，我们修改了碰撞检测逻辑，确保在球的位置更新后，重新对所有方块进行碰撞检测。
    ```c
    // 碰撞检测伪代码
    for (每个球) {
        desired_position = 计算球的期望位置;
        for (每个方块) {
            if (球与方块碰撞(desired_position)) {
                摧毁方块;
                更新球的实际位置;
                // 重要：由于球的位置改变了，需要重新检测与剩余方块的碰撞
                break; // 跳出当前方块循环，但外层球循环会继续
            }
        }
    }
    ```
    一个更优的解决方案是使用空间分区（如网格），只检测球附近的方法，但这留待以后优化。

![](img/d2b281400e0d90c00f492f0adbc6943a_181.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_183.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_185.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_187.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_189.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_190.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_192.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_194.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_196.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_198.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_199.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_201.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_203.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_205.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_207.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_209.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_211.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_213.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_215.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_217.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_218.png)

3.  **关卡保存问题**：修复了太空侵略者关卡完成后，进度未正确保存的问题。

![](img/d2b281400e0d90c00f492f0adbc6943a_220.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_222.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_224.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_226.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_228.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_230.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_232.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_234.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_236.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_237.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_239.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_241.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_243.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_245.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_247.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_249.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_251.png)

---

![](img/d2b281400e0d90c00f492f0adbc6943a_253.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_254.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_256.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_258.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_260.png)

## 建立性能基准

![](img/d2b281400e0d90c00f492f0adbc6943a_262.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_264.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_266.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_268.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_270.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_272.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_274.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_276.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_278.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_280.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_281.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_282.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_284.png)

为了有效优化，我们需要一个准确的性能基准。我们将游戏窗口设为全屏（4K分辨率），这会使性能问题暴露无遗。在全屏模式下，菜单的帧时间高达66毫秒，游戏体验非常卡顿。

![](img/d2b281400e0d90c00f492f0adbc6943a_286.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_288.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_290.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_291.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_293.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_295.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_297.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_298.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_300.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_302.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_304.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_306.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_308.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_310.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_312.png)

我们的目标是：**在4K全屏下，将菜单的帧时间优化到16毫秒（约60FPS）**。

![](img/d2b281400e0d90c00f492f0adbc6943a_314.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_316.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_318.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_320.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_321.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_323.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_325.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_326.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_328.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_330.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_332.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_334.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_335.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_336.png)

---

![](img/d2b281400e0d90c00f492f0adbc6943a_338.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_340.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_342.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_344.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_345.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_347.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_349.png)

## 构建性能分析器 🔍

![](img/d2b281400e0d90c00f492f0adbc6943a_351.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_353.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_355.png)

优化第一步是了解时间花在哪里。我们将构建一个简单的性能分析器来测量关键部分的执行时间。

![](img/d2b281400e0d90c00f492f0adbc6943a_357.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_359.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_361.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_363.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_365.png)

分析器将测量以下部分：
*   **总帧时间**
*   **输入处理时间**
*   **游戏逻辑更新（不包括渲染）时间**
*   **缓冲区交换（Flip）时间**
*   **音频处理时间**
*   **各个渲染函数的时间**（如绘制矩形、旋转矩形、绘制位图等）

![](img/d2b281400e0d90c00f492f0adbc6943a_367.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_369.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_371.png)

以下是分析器的核心结构：

![](img/d2b281400e0d90c00f492f0adbc6943a_373.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_375.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_377.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_379.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_381.png)


![](img/d2b281400e0d90c00f492f0adbc6943a_383.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_385.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_387.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_389.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_391.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_393.png)

```c
// 性能分析数据结构
typedef struct {
    u64 elapsed_time; // 总耗时（微秒或毫秒）
    u32 hit_count;    // 被调用的次数
} ProfileData;

// 分析器实例
ProfileData g_profile_data[PROFILE_SLOT_COUNT];
// 开始分析一个代码块
void profile_begin(int slot_index) {
    g_profile_data[slot_index].start_time = os_get_performance_counter();
}
// 结束分析一个代码块
void profile_end(int slot_index) {
    u64 end_time = os_get_performance_counter();
    u64 elapsed = end_time - g_profile_data[slot_index].start_time;
    g_profile_data[slot_index].elapsed_time += elapsed;
    g_profile_data[slot_index].hit_count++;
}
```

![](img/d2b281400e0d90c00f492f0adbc6943a_395.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_397.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_399.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_401.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_402.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_403.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_405.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_407.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_409.png)

我们在游戏循环的关键位置插入`profile_begin`和`profile_end`调用，并在屏幕上绘制结果。

![](img/d2b281400e0d90c00f492f0adbc6943a_411.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_413.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_415.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_417.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_419.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_421.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_423.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_425.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_427.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_429.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_431.png)

---

![](img/d2b281400e0d90c00f492f0adbc6943a_433.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_434.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_436.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_438.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_440.png)

## 分析性能瓶颈

![](img/d2b281400e0d90c00f492f0adbc6943a_442.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_444.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_446.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_448.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_450.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_452.png)

启用分析器后，我们运行全屏菜单，得到了清晰的数据：
*   **总帧时间**：~66毫秒
*   **游戏逻辑**：~1毫秒（占比很小）
*   **渲染**：~65毫秒（占绝大部分）
*   在渲染中，`draw_rotated_rect`（绘制旋转矩形）和`draw_bitmap`（绘制位图）是最大的两个耗时函数。

![](img/d2b281400e0d90c00f492f0adbc6943a_454.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_456.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_457.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_458.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_459.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_461.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_463.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_465.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_467.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_469.png)

**结论**：优化重点在于渲染，特别是`draw_rotated_rect`和`draw_bitmap`函数。

![](img/d2b281400e0d90c00f492f0adbc6943a_471.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_473.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_475.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_477.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_479.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_481.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_483.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_485.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_487.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_489.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_491.png)

---

![](img/d2b281400e0d90c00f492f0adbc6943a_493.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_495.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_497.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_499.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_501.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_503.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_505.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_507.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_509.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_511.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_513.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_515.png)

## 优化旋转矩形绘制 🔄

![](img/d2b281400e0d90c00f492f0adbc6943a_517.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_519.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_521.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_523.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_525.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_527.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_529.png)

`draw_rotated_rect`函数为每个像素计算它是否在旋转后的矩形内。原始算法使用了分离轴定理，但实现上可以优化。

![](img/d2b281400e0d90c00f492f0adbc6943a_531.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_533.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_535.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_537.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_539.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_541.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_543.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_545.png)

### 原始算法分析
原始代码为每个像素计算了3次向量减法和4次点积，以判断像素相对于矩形四个顶点的位置。

![](img/d2b281400e0d90c00f492f0adbc6943a_547.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_549.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_551.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_553.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_555.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_557.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_559.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_561.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_563.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_565.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_567.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_569.png)

### 优化步骤
1.  **减少计算量**：我们不需要相对于四个顶点都计算。只需要计算像素相对于矩形一个角点（如`p0`）的向量，然后用这个向量与矩形的两条边（轴）进行点积。
    *   优化前：3次减法，4次点积。
    *   优化后：1次减法，2次点积。
    ```c
    // 优化后伪代码
    V2 pixel_relative = pixel_pos - rect_corner_p0;
    float proj_x = dot(pixel_relative, normalized_axis_x); // 轴需要归一化
    float proj_y = dot(pixel_relative, normalized_axis_y);
    if (proj_x >= 0 && proj_x <= axis_x_length &&
        proj_y >= 0 && proj_y <= axis_y_length) {
        // 像素在矩形内，进行绘制
    }
    ```

![](img/d2b281400e0d90c00f492f0adbc6943a_571.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_573.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_575.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_577.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_579.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_581.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_583.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_585.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_587.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_589.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_591.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_593.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_595.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_596.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_598.png)

2.  **内联函数与展开计算**：将向量运算（如点积、减法）手动内联展开，避免函数调用开销，并帮助编译器更好地优化。
    ```c
    // 将点积函数调用展开为直接计算
    // dot(a, b) -> a.x*b.x + a.y*b.y
    float proj_x = (pixel_pos.x - p0.x) * axis_x_norm.x +
                   (pixel_pos.y - p0.y) * axis_x_norm.y;
    ```

![](img/d2b281400e0d90c00f492f0adbc6943a_600.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_602.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_603.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_605.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_607.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_609.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_611.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_613.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_615.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_617.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_618.png)

3.  **预计算**：矩形的轴、归一化后的轴、轴长度等可以在循环外预计算一次，避免每像素重复计算。

![](img/d2b281400e0d90c00f492f0adbc6943a_620.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_622.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_624.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_626.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_628.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_630.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_632.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_634.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_636.png)

### 优化效果
经过上述优化，`draw_rotated_rect`的性能显著提升。在全屏菜单中，其耗时从约25毫秒降低到约14毫秒。

![](img/d2b281400e0d90c00f492f0adbc6943a_638.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_640.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_642.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_644.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_646.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_648.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_649.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_651.png)

---

![](img/d2b281400e0d90c00f492f0adbc6943a_653.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_655.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_657.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_659.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_661.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_663.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_665.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_666.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_667.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_669.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_670.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_672.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_674.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_676.png)

## 优化位图绘制 🖼️

![](img/d2b281400e0d90c00f492f0adbc6943a_678.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_680.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_682.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_684.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_686.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_688.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_690.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_692.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_694.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_696.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_698.png)

`draw_bitmap`函数同样存在优化空间，特别是其内部的像素映射和Alpha混合循环。

![](img/d2b281400e0d90c00f492f0adbc6943a_700.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_702.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_704.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_706.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_708.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_710.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_712.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_714.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_715.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_717.png)

### 优化步骤
1.  **展开映射计算**：将计算UV坐标（纹理坐标）的`map_to_normal_range`函数内联展开，并预计算范围（range）。
    ```c
    // 预计算
    float range_x = (src_rect.x1 - src_rect.x0);
    float range_y = (src_rect.y1 - src_rect.y0);
    float inv_range_x = 1.0f / range_x;
    float inv_range_y = 1.0f / range_y;

    // 循环内内联计算
    float u = (pixel_x - src_rect.x0) * inv_range_x;
    float v = (pixel_y - src_rect.y0) * inv_range_y;
    int tex_x = (int)(u * bitmap_width);
    int tex_y = (int)(v * bitmap_height);
    ```

![](img/d2b281400e0d90c00f492f0adbc6943a_719.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_721.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_723.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_725.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_727.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_729.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_731.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_733.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_735.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_737.png)

2.  **优化Alpha混合**：Alpha混合（颜色插值）是每像素都要进行的繁重操作。我们通过预乘Alpha和提取颜色分量来减少循环内的计算。
    ```c
    // 预计算源颜色和Alpha
    float src_r = (float)((source_color >> 16) & 0xFF);
    float src_g = (float)((source_color >> 8) & 0xFF);
    float src_b = (float)(source_color & 0xFF);
    float alpha = alpha_multiplier * (1.0f / 255.0f); // 转换为浮点

    float inv_alpha = 1.0f - alpha;
    // 预乘
    src_r *= alpha;
    src_g *= alpha;
    src_b *= alpha;

    // 循环内简化混合
    pixel_color = ... // 从位图获取目标像素
    float dst_r = (float)((pixel_color >> 16) & 0xFF);
    float dst_g = (float)((pixel_color >> 8) & 0xFF);
    float dst_b = (float)(pixel_color & 0xFF);

    // 混合公式: result = src * alpha + dst * (1 - alpha)
    // 因为src已预乘alpha，所以是: result = src + dst * inv_alpha
    float out_r = src_r + dst_r * inv_alpha;
    float out_g = src_g + dst_g * inv_alpha;
    float out_b = src_b + dst_b * inv_alpha;
    ```

![](img/d2b281400e0d90c00f492f0adbc6943a_739.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_741.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_743.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_745.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_747.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_749.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_751.png)

### 优化效果
位图绘制的耗时也得到了降低。结合旋转矩形的优化，菜单的总帧时间大幅下降。

![](img/d2b281400e0d90c00f492f0adbc6943a_753.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_755.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_757.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_759.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_761.png)

---

## “作弊”式优化：用几何体代替位图 🎨

![](img/d2b281400e0d90c00f492f0adbc6943a_763.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_765.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_767.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_769.png)

分析发现，主菜单中最大的位图是静态的Logo。绘制一个大的、带Alpha通道的位图非常耗时。

![](img/d2b281400e0d90c00f492f0adbc6943a_771.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_773.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_775.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_777.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_779.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_780.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_782.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_784.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_785.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_787.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_789.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_791.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_793.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_795.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_797.png)

一个有效的“游戏开发技巧”是：**用更简单的几何图形来近似复杂的位图**。我们决定用一系列旋转的矩形和文本绘制来替换静态的Logo位图。
*   原来：绘制一张大的透明PNG。
*   现在：用几个不同颜色、不同角度的旋转矩形拼出Logo的轮廓，再在上面绘制文字。

![](img/d2b281400e0d90c00f492f0adbc6943a_799.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_801.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_803.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_805.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_807.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_809.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_811.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_813.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_815.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_817.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_819.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_821.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_823.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_825.png)

这样做虽然牺牲了一些视觉精度，但换来了巨大的性能提升，因为绘制多个小矩形的开销远小于采样和混合一个大位图。

![](img/d2b281400e0d90c00f492f0adbc6943a_827.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_829.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_831.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_833.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_835.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_837.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_839.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_841.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_843.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_844.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_846.png)

---

![](img/d2b281400e0d90c00f492f0adbc6943a_848.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_850.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_852.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_854.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_856.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_858.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_860.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_862.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_864.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_866.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_868.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_870.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_872.png)

## 最终成果与总结 🏆

![](img/d2b281400e0d90c00f492f0adbc6943a_874.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_876.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_878.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_880.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_882.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_884.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_886.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_888.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_890.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_892.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_893.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_895.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_897.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_899.png)

经过一系列的分析和优化，我们取得了显著的成果：

![](img/d2b281400e0d90c00f492f0adbc6943a_901.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_903.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_905.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_907.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_909.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_911.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_913.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_915.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_917.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_919.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_921.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_923.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_925.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_927.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_929.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_931.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_933.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_935.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_936.png)

| 阶段 | 菜单帧时间 (4K全屏) | 性能提升倍数 |
| :--- | :--- | :--- |
| 优化前 | **~66 毫秒** | 1x (基准) |
| 优化旋转矩形后 | ~44 毫秒 | 1.5x |
| 优化位图与最终调整后 | **~12.4 毫秒** | **~5.3x** |

![](img/d2b281400e0d90c00f492f0adbc6943a_938.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_939.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_941.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_943.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_945.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_946.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_948.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_949.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_950.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_952.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_953.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_955.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_956.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_958.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_960.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_962.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_964.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_966.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_968.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_970.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_972.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_974.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_976.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_978.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_980.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_982.png)

**目标达成！** 我们将菜单性能提升了**5倍多**，从无法流畅运行的66毫秒优化到了非常流畅的12.4毫秒，超过了16毫秒的目标。

![](img/d2b281400e0d90c00f492f0adbc6943a_984.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_986.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_988.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_989.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_991.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_993.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_995.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_996.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_998.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1000.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1002.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1004.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1006.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1008.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1010.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1012.png)

### 本节课总结

![](img/d2b281400e0d90c00f492f0adbc6943a_1014.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1016.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1018.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1020.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1022.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1024.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1026.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1028.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1030.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1032.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1034.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1036.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1038.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1040.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1042.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1044.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1046.png)

在本节课中，我们一起学习了游戏性能优化的完整流程：

![](img/d2b281400e0d90c00f492f0adbc6943a_1048.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1050.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1052.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1054.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1056.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1058.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1060.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1062.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1064.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1066.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1068.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1070.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1072.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1074.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1076.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1078.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1080.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1082.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1084.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1086.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1088.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1089.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1091.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1093.png)

1.  **定位问题**：使用帧率计数器识别性能瓶颈场景（主菜单）。
2.  **建立基准**：在全屏高分辨率下测试，获得准确的性能数据。
3.  **构建工具**：创建性能分析器，量化每个函数和模块的耗时。
4.  **分析瓶颈**：通过分析器数据，精确找到最耗时的函数（`draw_rotated_rect`, `draw_bitmap`）。
5.  **深入理解**：阅读并理解瓶颈函数的算法和计算过程。
6.  **应用优化**：
    *   **算法优化**：简化旋转矩形的包含性检测算法，减少不必要的计算。
    *   **微优化**：内联函数、展开循环、预计算结果、优化Alpha混合公式。
    *   **“作弊”优化**：在必要时，用性能更优的简单方法替代复杂方法（用几何体代替大位图）。
7.  **验证结果**：每次优化后都进行测试，确保功能正确且性能提升。

![](img/d2b281400e0d90c00f492f0adbc6943a_1095.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1097.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1099.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1101.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1103.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1105.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1107.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1108.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1109.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1110.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1112.png)

![](img/d2b281400e0d90c00f492f0adbc6943a_1114.png)

优化是一个迭代过程，需要耐心和分析。我们学到的关键点是：**永远先测量，再优化；优化最热点的代码**。我们的游戏现在即使在4K分辨率下也能流畅运行，为后续添加更多内容打下了坚实的基础。在接下来的课程中，我们还可以探索更高级的优化技术，如SIMD（单指令多数据）并行计算和多线程渲染。