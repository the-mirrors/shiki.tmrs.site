---
outline: deep
---

# 安装与基本使用

<Badges name="shiki" />

## 安装

你可以通过 npm 仓库安装 Shiki，或者[使用 CDN](#使用-cdn)：
::: code-group

```sh [npm]
npm install -D shiki
```

```sh [yarn]
yarn add -D shiki
```

```sh [pnpm]
pnpm add -D shiki
```

```sh [bun]
bun add -D shiki
```

```sh [deno]
deno add npm:shiki
```

:::

## 使用集成

我们还提供了一些集成插件：

- [markdown-it 插件](/packages/markdown-it)
- [Rehype 插件](/packages/rehype)
- [TypeScript Twoslash 集成](/packages/twoslash)
- [Monaco Editor 语法高亮](/packages/monaco)
- [CLI 工具](/packages/cli)
- [常用变换器](/packages/transformers)

## 使用方法

### 简写函数

使用 `shiki` 最快的方式是使用简写，它们会按需加载必要的主题和语言，并自动在内存中缓存。

将代码片段传入 `codeToHtml` 函数，并指定 `lang` 和 `theme`，它将会返回一个高亮的 HTML 字符串，你可以直接将它们嵌入页面中。这些生成的 HTML 包含每个 token 的内联样式，因此无需额外的 CSS 即可渲染所有的样式。

```ts twoslash
import { codeToHtml } from 'shiki'

const code = 'const a = 1' // 输入代码
const html = await codeToHtml(code, {
  lang: 'javascript',
  theme: 'vitesse-dark'
})

console.log(html) // 高亮后的 HTML 字符串
```

你也可以使用 `codeToTokens` 或 `codeToHast` 获取中间数据结构，然后自行渲染：

```ts twoslash theme:min-dark
import { codeToTokens } from 'shiki'

const { tokens } = await codeToTokens('<div class="foo">bar</div>', {
  lang: 'html',
  theme: 'min-dark'
})
```

```ts twoslash theme:catppuccin-mocha
import { codeToHast } from 'shiki'

const hast = await codeToHast('.text-red { color: red; }', {
  lang: 'css',
  theme: 'catppuccin-mocha'
})
```

### 使用 Highlighter

Shiki 提供的[简写函数](#简写函数)是异步执行的，其使用 WASM 并按需加载主题和语言。在某些情况下，你可能需要同步地进行语法高亮，因此 Shiki 提供了 `createHighlighter` 函数来创建一个 Highlighter 实例，之后可同步调用。

其用法与 `codeToHtml` 基本相同，每个主题和语言文件都是动态导入的 ES 模块。注意，为了获得最佳性能，建议**显式列出**所需的主题和语言。

```ts twoslash theme:nord
import { createHighlighter } from 'shiki'

// `createHighlighter` 是异步的，它会初始化内部环境
// 并加载指定的主题和语言。
const highlighter = await createHighlighter({
  themes: ['nord'],
  langs: ['javascript'],
})

// 之后可以同步地使用 `highlighter.codeToHtml`
// 调用已加载的主题和语言。
const code = highlighter.codeToHtml('const a = 1', {
  lang: 'javascript',
  theme: 'nord'
})
```

:::info 重要提示
Highlighter 实例应作为**长生命周期的单例**使用。建议在某处缓存该实例，并在整个应用中复用。应该避免在高频调用的函数或循环中反复调用 `createHighlighter`。

若在 Node.js 环境中运行，推荐使用 [简写函数](#简写函数)，它会自动管理 Highlighter 实例以及主题和语言的动态加载。
:::

此外，如果希望在 Highlighter 创建后加载主题或语言，可以使用 `loadTheme` 和 `loadLanguage` 方法。

```ts twoslash
import { createHighlighter } from 'shiki'

const highlighter = await createHighlighter({ themes: [], langs: [] })
// ---cut---
// 创建后加载主题和语言
await highlighter.loadTheme('vitesse-light')
await highlighter.loadLanguage('css')
```

自 Shiki v1.0 起，要求所有主题和语言必须显式加载。

```ts theme:slack-dark twoslash
import { createHighlighter } from 'shiki'

const highlighter = await createHighlighter({
  themes: ['slack-dark'],
  langs: ['css']
})

highlighter.codeToHtml(
  'const a = 1',
  { lang: 'javascript', theme: 'slack-dark' }
)
// @error: 抛出错误，`javascript` 未加载

await highlighter.loadLanguage('javascript') // 加载该语言

// 现在可以正常工作
```

如果希望一次性加载所有主题和语言（并不推荐），可以遍历 `bundledLanguages` 和 `bundledThemes` 中的所有键。

```ts twoslash theme:poimandres
import { bundledLanguages, bundledThemes, createHighlighter } from 'shiki'

const highlighter = await createHighlighter({
  themes: Object.keys(bundledThemes),
  langs: Object.keys(bundledLanguages),
})

highlighter.codeToHtml('const a = 1', {
  lang: 'javascript',
  theme: 'poimandres'
})
```

### 细粒捆绑预设

导入 `shiki` 时，所有主题和语言都会作为异步代码块被打包。通常情况下，只要你不使用它们，它们就不会被加载，因此不需要考虑。但在某些场景下你希望控制打包的内容，你可以使用核心模块自行组合打包。

更多细节请参阅[细粒捆绑预设](/guide/bundles#fine-grained-bundle)。

### 捆绑预设

我们也提供了一些预配置的包方案，便于直接使用，详情请见[捆绑预设](/guide/bundles)。

### 使用 CJS

`shiki` 仅以 ESM 格式发布，以减小包体积。但你仍然可以在 CJS 中使用，因为 Node.js 支持在 CJS 中动态导入 ESM 模块。

例如，以下 ESM 代码：

```ts twoslash
// ESM
import { createHighlighter } from 'shiki'

async function main() {
  const highlighter = await createHighlighter({
    themes: ['vitesse-dark'],
    langs: ['javascript'],
  })

  const code = highlighter.codeToHtml('const a = 1', {
    theme: 'vitesse-dark',
    lang: 'javascript',
  })
}
```

可以改写为 CJS：

```ts twoslash
// CJS
async function main() {
  const { createHighlighter } = await import('shiki')

  const highlighter = await createHighlighter({
    themes: ['vitesse-dark'],
    langs: ['javascript'],
  })

  const code = highlighter.codeToHtml('const a = 1', {
    theme: 'vitesse-dark',
    lang: 'javascript'
  })
}
```

### 使用 CDN

若想通过 CDN 在浏览器中使用 `shiki`，可以使用 [esm.run](https://esm.run) 或 [esm.sh](https://esm.sh)。

```html theme:rose-pine
<body>
  <div id="foo"></div>

  <script type="module">
    // 请务必指定确切版本
    import { codeToHtml } from 'https://esm.sh/shiki@3.0.0'
    // 或
    // import { codeToHtml } from 'https://esm.run/shiki@3.0.0'

    const foo = document.getElementById('foo')
    foo.innerHTML = await codeToHtml('console.log("Hi, Shiki on CDN :)")', {
      lang: 'js',
      theme: 'rose-pine'
    })
  </script>
</body>
```

这种方式效率很高，仅按需加载所需的语言和主题。以上代码片段只会发起四个请求（`shiki`、`@shikijs/themes/vitesse-light`、`@shikijs/langs/javascript`、`shiki/wasm.mjs`），总共传输约 200KB 数据。

[演示](https://jsfiddle.net/t7brz23v/)

### Cloudflare Workers

Cloudflare Workers [不支持从二进制数据初始化 WebAssembly](https://community.cloudflare.com/t/fixed-cloudflare-workers-slow-with-moderate-sized-webassembly-bindings/184668/3)，因此默认的 WASM 构建无法工作。你需要将 WASM 文件作为资源上传并直接导入。

同时，也建议使用[细粒捆绑预设](#细粒捆绑预设)方法来减小包体积。

```ts twoslash theme:nord
// @noErrors
import js from '@shikijs/langs/javascript'
import nord from '@shikijs/themes/nord'
import { createHighlighterCore, loadWasm } from 'shiki/core'

// 将 WASM 作为资源导入
await loadWasm(import('shiki/onig.wasm'))

export default {
  async fetch() {
    const highlighter = await createHighlighterCore({
      themes: [nord],
      langs: [js],
    })

    return new Response(highlighter.codeToHtml('console.log(\'shiki\');', {
      theme: 'nord',
      lang: 'js'
    }))
  },
}
```
