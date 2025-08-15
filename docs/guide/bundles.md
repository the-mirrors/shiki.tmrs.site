---
outline: deep
---

# 捆绑预设

`shiki` 主入口会通过动态懒加载的方式，将所有支持的主题和语言打包在一起。大多数情况下，这种方式的性能开销可以忽略，因为只有在实际使用某种语法时才会去加载相应的文件。但如果你需要把 Shiki 打进浏览器运行时或 Web Worker，即使这些文件没被加载，它们也会占据构建产物的体积。为此，我们提供了[细粒捆绑预设](#fine-grained-bundle)方式，让你可以按需一个个地引入所需的语言和主题。

::: info
如果你在开发 Web 应用，或者处于性能敏感的环境，建议使用[细粒捆绑预设](#fine-grained-bundle)来减少体积和内存占用。更多优化建议请参见[性能最佳实践](/guide/best-performance)。
:::

## 使用捆绑预设

为了方便，我们也提供了一些预先组合好的打包版本：

### `shiki/bundle/full`

> [包大小](/guide/#包大小)：6.4 MB（压缩后），1.2 MB（gzip），包含异步分块

完整打包版包含所有主题和语言，与主入口 `shiki` 一致。

### `shiki/bundle/web`

> [包大小](/guide/#包大小)：3.8 MB（压缩后），695 KB（gzip），包含异步分块

包含所有主题、常用 Web 语言（HTML、CSS、JS、TS、JSON、Markdown 等）以及部分 Web 框架（Vue、JSX、Svelte 等）。

用法与普通版本一致，`shiki` 的全部功能在该包中都可用：

```ts twoslash
import {
  BundledLanguage,
  BundledTheme,
  codeToHtml,
  createHighlighter
} from 'shiki/bundle/web' // [!code highlight]

const highlighter = await createHighlighter({
  langs: ['html', 'css', 'js'],
  themes: ['github-dark', 'github-light'],
})
````

## 细粒捆绑预设

在默认情况下，导入 `shiki` 会将所有主题和语言作为异步分块打包进去。虽然不使用它们时不会加载，但在某些情况下你可能希望完全控制打包内容，这时可以使用核心模块自己组合。

```ts twoslash
// @noErrors
// 直接导入主题和语言模块，只会打包你引入的内容
import nord from '@shikijs/themes/nord'

// `shiki/core` 不包含任何主题、语言或 wasm 二进制
import { createHighlighterCore } from 'shiki/core'
import { createOnigurumaEngine } from 'shiki/engine/oniguruma'

const highlighter = await createHighlighterCore({
  themes: [
    // 需要传入导入的模块，而不是字符串
    nord,
    // 也可以使用动态导入来进行代码分割
    import('@shikijs/themes/material-theme-ocean')
  ],
  langs: [
    import('@shikijs/langs/javascript'),
    // shiki 会尝试使用模块的默认导出
    () => import('@shikijs/langs/css'),
    // 也可以使用自定义语法的 getter
    async () => JSON.parse(await fs.readFile('my-grammar.json', 'utf-8'))
  ],
  // `shiki/wasm` 包含内嵌为 base64 字符串的 wasm 二进制
  engine: createOnigurumaEngine(import('shiki/wasm'))
})

// 也可以在创建后再加载主题和语言
await highlighter.loadTheme(import('@shikijs/themes/vitesse-light'))

const code = highlighter.codeToHtml('const a = 1', {
  lang: 'javascript',
  theme: 'material-theme-ocean'
})
```

::: info 注释
[简写](/guide/install#简写)仅适用于预设打包版本。细粒化打包中，你可以用 [`createSingletonShorthands`](https://github.com/shikijs/shiki/blob/main/packages/core/src/constructors/bundle-factory.ts#L203) 自己创建，或自行实现类似功能。
:::
