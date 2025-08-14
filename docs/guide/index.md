---
outline: deep
---

# 介绍

<br>

<span text-brand-yellow text-xl>Shiki</span> <span op75>(式，日语中表示 [“样式”](https://jisho.org/word/%E5%BC%8F))</span> 是一个基于 TextMate 语法和主题的美观且强大的语法高亮工具，使用与 VS Code 相同的语法高亮引擎。它能为几乎所有主流编程语言提供非常精准且快速的语法高亮。

无需维护自定义的正则表达式，无需维护自定义 CSS，无需维护自定义 HTML。并且可以与你的 VS Code 中主题和语言支持的更新保持同步。

顺便一提，正如你所预期的那样，本网站中的所有代码块都是由 Shiki 高亮的 :)

## 特性

- 所有语法/主题/WASM 都以 ESM 格式提供，按需懒加载，且对打包工具友好
- 便携且与运行环境无关。不依赖 Node.js API 或文件系统，可运行于任何现代 JavaScript 运行时
- 支持 ESM（[使用 CDN](/guide/install#使用-cdn) [或者使用 CJS](/guide/install#使用-cjs)）
- [可按需组合打包语言/主题](/guide/bundles#fine-grained-bundle)
- [支持双颜色主题](/guide/dual-themes)
- [支持 `hast`](/guide/transformers#codetohast)
- [Transformers API](/guide/transformers)
- [Decorations API](/guide/decorations)
- [TypeScript Twoslash 集成](/packages/twoslash)
- [兼容性构建](/guide/compat)

## 在线演练场

这里有一个小型在线演练场，方便你试用 Shiki 的代码高亮效果。本教程中的其他代码块是在构建时渲染并静态发布的，而这个演练场是在浏览器端实时渲染的。主题和语言会按需加载。

<ShikiMiniPlayground />

[安装 Shiki](/guide/install) 以在你的项目中使用它。

## 包大小

你可以在 [pkg-size.dev/shiki](https://pkg-size.dev/shiki) 上详细查看包大小信息。

截至 `v1.1.6`（2024 年 2 月 22 日测量）：

| 包名                | 体积（压缩后） | 体积（gzip 后） | 说明                                                           |
| ------------------- | -------------: | --------------: | -------------------------------------------------------------- |
| `shiki`             |         6.9 MB |         1.3 MB  | 所有主题和语言按异步块加载                                     |
| `shiki/bundle/full` |         6.9 MB |         1.3 MB  | 与 `shiki` 相同                                                |
| `shiki/bundle/web`  |         4.2 MB |         748 KB  | 所有主题和常用 Web 语言按异步块加载                            |
| `shiki/core`        |         106 KB |          34 KB  | 核心引擎，不包含任何主题或语言，可自行组合                     |
| `shiki/wasm`        |         623 KB |         231 KB  | 内嵌为 base64 字符串的 WASM 二进制文件                         |
