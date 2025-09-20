# 主题颜色控制

## 多种颜色值支持

通常，TextMate 主题要求标签的颜色值是有效的十六进制颜色值，这个限制来自于 [`vscode-textmate`](https://github.com/microsoft/vscode-textmate)。然而，在 shiki v1.0 中，我们引入了一个自动解决的方法，通过用占位符替换非十六进制颜色值，并在标签化时将其替换回来。这样可以让你使用具有多种颜色值的主题进行渲染，而不必担心其技术细节：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"6918cd70cd50e56b0bb5f1f013d51abf82b36938731ef12bdcb7c1283bd6524f","data":"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"}
import { createHighlighter } from 'shiki'

const highlighter = await createHighlighter({
  langs: ['javascript'],
  themes: [
    {
      name: 'my-theme',
      settings: [
        {
          scope: ['comment'],
          settings: {
            // 使用 `rgb`、`hsl`、`hsla`，// [!code hl:3]
            // 或者任何你的渲染器支持的颜色。
            foreground: 'rgb(128, 128, 128)'
          }
        },
        {
          scope: ['string'],
          settings: {
            foreground: 'var(--code-string)' // CSS 变量 // [!code hl:1]
          }
        },
        // 更多
      ],
      // 背景和前景颜色 // [!code hl:3]
      bg: 'var(--code-bg)',
      fg: 'var(--code-fg)'
    }
  ]
})

const html = highlighter.codeToHtml('const foo = "bar"', { lang: 'javascript', theme: 'my-theme' })
```

::: info 注意
谨慎使用，这将与 TextMate 主题不兼容。

这也可能会使主题与非 Web 用例不兼容，例如 [`shiki-cli`](/packages/cli) 和 [`shiki-monaco`](/packages/monaco)。
:::

了解如何 [载入主题](./load-theme)。

## 颜色替换

你还可以使用 `colorReplacements` 选项来替换主题的颜色值。它在你想要使用具有不同颜色的主题时非常有用。它在主题对象、 `codeToHast` 以及 `codeToHtml` 的选项上可用。

`colorReplacements` 对象需要符合颜色与颜色一对一的形式，键是你想要替换的原颜色，值是新的颜色：

```js
const html = await codeToHtml(
  code,
  {
    lang: 'js',
    theme: 'min-dark',
    colorReplacements: {
      '#ff79c6': '#189eff'
    }
  }
)
```

此外，`colorReplacements` 可以包含制定作用域的替换。对于多主题的情况下，这样可以很方便的替换指定主题的颜色：

```js
const html = await codeToHtml(
  code,
  {
    lang: 'js',
    themes: { dark: 'min-dark', light: 'min-light' },
    colorReplacements: {
      'min-dark': {
        '#ff79c6': '#189eff'
      },
      'min-light': {
        '#ff79c6': '#defdef'
      }
    }
  }
)
```

该选项只在 `colorReplacements` 上可用，在主题对象上则不可用。

## CSS 变量主题

shiki 提供了一个工厂函数助手（Factory Function Helper）`createCssVariablesTheme`，用于更方便地创建使用 CSS 变量的主题。请注意，这个主题形式比大多数其他主题的细粒度要低，并且需要在你的应用程序中定义 CSS 变量。这是为了更方便地从 Shiki 的 [`css-variables` 主题](https://github.com/shikijs/shiki/blob/main/docs/themes.md#theming-with-css-variables) 迁移而提供的。但为了更好的显示效果，我们建议使用 [多种颜色值支持](#多种颜色值支持) 或 [颜色替换](#颜色替换) 来覆盖现有的主题。

此主题形式**不包含在默认设置**中，必须显式注册：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"da1a6c5d7b596629e080dd65eef3b7f9ebedbfc9cc8e48dd83982aabc90799d6","data":"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="}
import { createHighlighter } from 'shiki'
import { createCssVariablesTheme } from 'shiki/core'

// 创建一个自定义的 CSS 变量主题，以下是默认值。
const myTheme = createCssVariablesTheme({ // [!code hl:6]
  name: 'css-variables',
  variablePrefix: '--shiki-',
  variableDefaults: {},
  fontStyle: true
})

const highlighter = await createHighlighter({
  langs: ['javascript'],
  themes: [myTheme] // 注册主题 // [!code hl]
})

const html = highlighter.codeToHtml('const foo = "bar"', {
  lang: 'javascript',
  theme: 'css-variables' // 使用主题 // [!code hl]
})
```

CSS 变量的示例如下：

```css
:root {
  --shiki-foreground: #eeeeee;
  --shiki-background: #333333;
  --shiki-token-constant: #660000;
  --shiki-token-string: #770000;
  --shiki-token-comment: #880000;
  --shiki-token-keyword: #990000;
  --shiki-token-parameter: #aa0000;
  --shiki-token-function: #bb0000;
  --shiki-token-string-expression: #cc0000;
  --shiki-token-punctuation: #dd0000;
  --shiki-token-link: #ee0000;

  /* Only required if using lang: 'ansi' */
  --shiki-ansi-black: #000000;
  --shiki-ansi-black-dim: #00000080;
  --shiki-ansi-red: #bb0000;
  --shiki-ansi-red-dim: #bb000080;
  --shiki-ansi-green: #00bb00;
  --shiki-ansi-green-dim: #00bb0080;
  --shiki-ansi-yellow: #bbbb00;
  --shiki-ansi-yellow-dim: #bbbb0080;
  --shiki-ansi-blue: #0000bb;
  --shiki-ansi-blue-dim: #0000bb80;
  --shiki-ansi-magenta: #ff00ff;
  --shiki-ansi-magenta-dim: #ff00ff80;
  --shiki-ansi-cyan: #00bbbb;
  --shiki-ansi-cyan-dim: #00bbbb80;
  --shiki-ansi-white: #eeeeee;
  --shiki-ansi-white-dim: #eeeeee80;
  --shiki-ansi-bright-black: #555555;
  --shiki-ansi-bright-black-dim: #55555580;
  --shiki-ansi-bright-red: #ff5555;
  --shiki-ansi-bright-red-dim: #ff555580;
  --shiki-ansi-bright-green: #00ff00;
  --shiki-ansi-bright-green-dim: #00ff0080;
  --shiki-ansi-bright-yellow: #ffff55;
  --shiki-ansi-bright-yellow-dim: #ffff5580;
  --shiki-ansi-bright-blue: #5555ff;
  --shiki-ansi-bright-blue-dim: #5555ff80;
  --shiki-ansi-bright-magenta: #ff55ff;
  --shiki-ansi-bright-magenta-dim: #ff55ff80;
  --shiki-ansi-bright-cyan: #55ffff;
  --shiki-ansi-bright-cyan-dim: #55ffff80;
  --shiki-ansi-bright-white: #ffffff;
  --shiki-ansi-bright-white-dim: #ffffff80;
}
```

如果你是从 Shiki 的 v0 版本迁移来的，以下是一些对 Shiki 的 `css-variables` 条目的更名供你参考：

| Shiki v0                   | Shiki v1.0           |
| -------------------------- | -------------------- |
| `--shiki-color-text`       | `--shiki-foreground` |
| `--shiki-color-background` | `--shiki-background` |
| `--shiki-color-ansi-*`     | `--shiki-ansi-*`     |
