---
layout: note
title: Home
permalink: /notes/cmake
---

# CMake语法

`include(${CMAKE_CURRENT_LIST_DIR}/*)`可以在**CMakeLists.txt**中显示引用写在其他文件中的cmake内容

[target_include_directories](https://cmake.org/cmake/help/v3.0/command/target_include_directories.html)可以对`add_executable`指明include目录，避免出现在同一个项目中有的executable不能使用该目录时，要remove的情况；同样的`target_compile_definitions`

```cmake
add_library(custom_compile_flags INTERFACE)
target_compile_features(custom_compile_flags INTERFACE cxx_std_17)
target_link_libraries(${Your Executable} PUBLIC compile_flags)
```
或者仅使用`target_compile_features(${Your Executable} INTERFACE cxx_std_17)`即可指定某个目标使用特定的C/C++标准

CMake有三种导入[add_executable](https://cmake.org/cmake/help/latest/command/add_executable.html#add-executable)的方式

```cmake
set(GEM5_OPT "${CMAKE_CURRENT_LIST_DIR}/build/X86/gem5.opt")
add_executable(gem5opt IMPORTED)
set_property(TARGET gem5opt PROPERTY IMPORTED_LOCATION ${GEM5_OPT})
```

将`gem5.opt`导入后可以进行测试，使用`ctest`框架

```cmake
enable_testing()
add_test(NAME ${NAME} COMMAND gem5opt ${ARGS})
```

方便在clion等IDE中方便用GUI测试，也可以在cmake的`build`目录中跑`ctest`

使用`add_compile_options`而不是`set(CMAKE_C_FLAGS)`来管理编译选项

`add_custom_target`的`COMMAND`不需要用双引号括起来

