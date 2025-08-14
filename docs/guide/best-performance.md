---
outline: deep
---

# 性能优化最佳实践

本指南将帮助你提升 Shiki 的使用性能。

## 缓存 Highlighter 实例

创建一个 Highlighter 实例的开销很大。大多数情况下，你应该在应用中只创建一次实例并复用（单例模式），而不是每次高亮时都重新创建。

示例：

```ts
import { createHighlighterCore } from 'shiki/core'

const highlighterPromise = createHighlighterCore({ /* ... */ })

export async function highlightCode(code: string, lang: string) {
  const highlighter = await highlighterPromise
  return highlighter.codeToHtml(code, lang)
}
````

当不再需要 highlighter 时，可以调用 `dispose()` 手动释放资源（不会自动被 GC 回收，必须显式调用）。

```ts
highlighter.dispose()
```

## 细粒捆绑预设

预构建的 bundle 主要是为了方便使用，适合在 Node.js 环境中对包体积不敏感的场景。如果是构建 Web 应用或在资源受限的环境中，建议使用精细化模块，减少包体积和内存占用。

**避免直接引入 `shiki`、`shiki/bundle/full`、`shiki/bundle/web`**。

应改为按需引入精细化模块，例如 `shiki/core`、`shiki/engine/javascript`、`@shikijs/langs/typescript`、`@shikijs/themes/dark-plus` 等：

```ts
import { createHighlighterCore } from 'shiki/core'
import { createJavaScriptRegexEngine } from 'shiki/engine/javascript'

const highlighter = await createHighlighterCore({
  themes: [
    import('@shikijs/themes/nord'),
    import('@shikijs/themes/dark-plus'),
    // ...
  ],
  langs: [
    import('@shikijs/langs/typescript'),
    import('@shikijs/langs/javascript'),
    // ...
  ],
  engine: createJavaScriptRegexEngine()
})
```

如果想更方便地组合精细化模块，可以使用 [`shiki-codegen`](/packages/codegen) 自动生成配置。
更多细节见[细粒捆绑预设](/guide/bundles#细粒捆绑预设)。

## 使用简写

`createHighlighter` 和 `createHighlighterCore` 会**一次性**加载所有主题和语言，以保证后续高亮操作是同步的。但这会增加启动时间，尤其是在主题和语言较多时。

简写会在内部维护一个 Highlighter 实例，并在需要时才加载对应主题和语言，适合能接受异步高亮的场景。

```ts
import { codeToHtml } from 'shiki'

// 调用时只会加载 `javascript` 语言和 `nord` 主题
const html = await codeToHtml('const a = 1', {
  lang: 'javascript',
  theme: 'nord'
})
```

你也可以基于细粒捆绑创建自己的简写，参考[使用细粒捆绑预设创建简写](/guide/shorthands#使用细粒捆绑创建简写)。

## JavaScript 引擎与预编译语言

Shiki 提供了 [两种正则引擎](/guide/regex-engines)：[`JavaScript`](/guide/regex-engines#javascript-正则引擎) 和 [`Oniguruma`](/guide/regex-engines#oniguruma-引擎)。

* **Oniguruma**：基于 WebAssembly，从 C 代码编译而来
* **JavaScript**：纯 JS 引擎，将 Oniguruma 风格的正则翻译为原生 JS 正则

如果是打包到 Web 环境，JavaScript 引擎体积更小、启动更快。
配合[预编译语言](/guide/regex-engines#预编译语言)，还能进一步减少包体积和启动时间（前提是目标浏览器支持最新的 RegExp 特性）。

更多详情见 [正则引擎](/guide/regex-engines)。

## 使用 Worker

Shiki 通过正则进行代码高亮，可能会占用较多 CPU。
可以将高亮任务放到 Web Worker / Node Worker 中，避免阻塞主线程。

::: info
🚧 我们还在编写更方便的 Worker 使用指南。
:::
