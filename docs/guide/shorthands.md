# 简写（Shorthands）

使用 `shiki` 最简单的方式，就是直接用官方提供的**简写函数**。这些函数会按需加载所需的主题和语言，并自动将它们缓存到内存中。与 `createHighlighter` 和 `createHighlighterCore` 不同，这些操作是**异步**的。

```ts twoslash
import { codeToHtml } from 'shiki'

const code = 'const a = 1' // 输入的代码
const html = await codeToHtml(code, {
  lang: 'javascript',
  theme: 'vitesse-dark'
})

console.log(html) // 高亮后的 HTML 字符串
````

## 使用细粒捆绑创建简写

你也可以用细粒捆绑方式来自定义自己的简写函数。下面是一个使用细粒捆绑创建简写的示例：

```ts
import { createdBundledHighlighter, createSingletonShorthands } from 'shiki/core'
import { createJavaScriptRegexEngine } from 'shiki/engine/javascript'

const BundledLanguage = {
  typescript: () => import('@shikijs/langs/typescript'),
  javascript: () => import('@shikijs/langs/javascript'),
  vue: () => import('@shikijs/langs/vue'),
}

const BundledTheme = {
  'light-plus': () => import('@shikijs/themes/light-plus'),
  'dark-plus': () => import('@shikijs/themes/dark-plus'),
}

// 用细粒捆绑方式创建自定义的 'createHighlighter' 函数
export const createHighlighter = /* @__PURE__ */ createdBundledHighlighter<
  BundledLanguage,
  BundledTheme
>({
  langs: bundledLanguages,
  themes: bundledThemes,
  engine: () => createJavaScriptRegexEngine(),
})

// 创建简写函数
export const {
  codeToHtml,
  codeToHast,
  codeToTokensBase,
  codeToTokens,
  codeToTokensWithThemes,
  getSingletonHighlighter,
  getLastGrammarState,
} = /* @__PURE__ */ createSingletonShorthands(
  createHighlighter,
)
```

你还可以使用 [`shiki-codegen`](/packages/codegen) 来为你自动生成细粒捆绑配置。
