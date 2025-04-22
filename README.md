# STM32 Collaboration Project Template

本仓库旨在为基于 STM32 芯片的嵌入式开发寻找一个合适的版本管理及多人协作方案。

> 进行 STM32 嵌入式开发的过程中我们通常会使用 STM32CubeMX 和 HAL 库，STM32CubeMX 生成代码时会产生大量的驱动代码和初始化代码，这些代码会严重影响基于 Git 的版本管理方案。故而需要寻找某种方案减小其影响
>
> 由于驱动和初始化代码的变更可以在 `.ioc` 文件（`ini`  格式）中完整的体现，而通常我们会搭载 FreeRTOS 平台，从而不需要在 `main.c` 文件中做变更，而中断处理可以通过重写 HAL 回调函数实现，从而可以不修改任何生成的代码（除了需要在 `main.c` 中添加一个 `entry.h` 的入口头文件）.故而我们可以将 STM32CubeMX 生成的代码全部添加到 `.gitignore` 文件中，只保留 `.ioc` 文件。
>
> 如此 Git 便只会追踪用户代码的变更而忽略初始化程序和驱动的变化。

## 注意事项

由于多分支同时修改 `.ioc` 文件极易引起 Conflicts，且不便处理，所以应当至多有一个分支涉及硬件配置变更，合并之后其他开发者应立即 pull 以保持最新。

远程仓库的 `.ioc` 应当处于一个确定的 toolchain 下（例如 Makefile），所有开发者应当确保提交的 `.ioc` 文件没有改变 toolchain 的配置。可以关注 .ioc 中的 `ProjectManager.*`，通常这些配置不会再发生变动，其中与 toolchain 有关的项是 `ProjectManager.PreviousToolchain` 和 `ProjectManager.TargetToolchain`

## Git 库中的目录结构

```
project/
├── UserCode/
│   ├── entry.h
│   └── your-code
├── project.ioc
├── .gitignore
├── .gitattributes
├── .editorconfig
└── README.md
```

