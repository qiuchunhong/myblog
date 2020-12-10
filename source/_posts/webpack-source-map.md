---
title: source-map
categories: webpack
tags: [source-map] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 一种 提供源代码到构建后代码映射 技术 (如果构建后代码出错了，通过映射可以追踪源代码错误)
---

```
devtool:source-map
devtool:[inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map
```

**参数详解：**

- `source-map`：外部
  错误代码准确信息 和 源代码的错误位置
- `inline-source-map`：内联
  只生成一个内联 source-map
  错误代码准确信息 和 源代码的错误位置
- `hidden-source-map`：外部
  错误代码错误原因，但是没有错误位置
  不能追踪源代码错误，只能提示到构建后代码的错误位置
- `eval-source-map`：内联
  每一个文件都生成对应的 source-map,都在 eval
  错误代码准确信息 和 源代码的错误位置
- `nosources-source-map`：外部
  错误代码准确信息，但是没有任何源代码信息
- `cheap-source-map`：外部
  错误代码准确信息 和 源代码的错误位置
  只能精确到行
- `cheap-module-source-map`：外部
  错误代码准确信息 和 源代码的错误位置
  module 会将 loader 的 source map 加入

## 内联和外部的区别：1.外部生成了文件，内联没有 2.内联构建速度更快

### 开发环境：速度快，调度更友好

1. 速度快(eval>inline>cheap>...)

- eval-cheap-source-map
- eval-source-map

2. 调度更友好

- source-map
- cheap-module-source-map
- cheap-souce-map
  --> eval-source-map / eval-cheap-module-source-map

### 生产环境：源代码要不要隐藏？调度要不要更友好

内联会让代码体积变大，所以在生产环境不用内联
nosources-source-map 全部隐藏
hidden-source-map 只隐藏源代码，会提示构建后代码错误信息
--> source-map / cheap-module-source-map
