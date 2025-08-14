---
outline: deep
---

# 明暗双颜色主题

Shiki 支持输出明暗双主题，甚至可以支持多主题。Shiki 的双主题实现方式是使用 CSS 变量，将每个语法 token 的颜色同时存储下来。

要启用双主题，只需在 `codeToHtml` 中将 `theme` 改为 `themes`，并提供 `light` 和 `dark` 两个键即可：

```ts twoslash
import { codeToHtml } from 'shiki'

const code = await codeToHtml('console.log("hello")', {
  lang: 'javascript',
  themes: { // [!code hl:4]
    light: 'min-light',
    dark: 'nord',
  }
})
````

生成的 HTML 示例（[演示预览](https://htmlpreview.github.io/?https://raw.githubusercontent.com/shikijs/shiki/main/packages/shiki/test/out/dual-themes.html)）：

```html
<pre
  class="shiki shiki-themes min-light nord"
  style="background-color:#ffffff;--shiki-dark-bg:#2e3440ff;color:#24292eff;--shiki-dark:#d8dee9ff"
  tabindex="0"
>
  <code>
    <span class="line">
      <span style="color:#1976D2;--shiki-dark:#D8DEE9">console</span>
      <span style="color:#6F42C1;--shiki-dark:#ECEFF4">.</span>
      <span style="color:#6F42C1;--shiki-dark:#88C0D0">log</span>
      <span style="color:#24292EFF;--shiki-dark:#D8DEE9FF">(</span>
      <span style="color:#22863A;--shiki-dark:#ECEFF4">"</span>
      <span style="color:#22863A;--shiki-dark:#A3BE8C">hello</span>
      <span style="color:#22863A;--shiki-dark:#ECEFF4">"</span>
      <span style="color:#24292EFF;--shiki-dark:#D8DEE9FF">)</span>
    </span>
  </code>
</pre>
```

如果要让它跟随你网站的主题切换，还需要额外添加一小段 CSS。

## 基于媒体查询的暗色模式

```css
@media (prefers-color-scheme: dark) {
  .shiki,
  .shiki span {
    color: var(--shiki-dark) !important;
    background-color: var(--shiki-dark-bg) !important;
    /* 可选，如果你还希望同步字体样式 */
    font-style: var(--shiki-dark-font-style) !important;
    font-weight: var(--shiki-dark-font-weight) !important;
    text-decoration: var(--shiki-dark-text-decoration) !important;
  }
}
```

## 基于类名的暗色模式

```css
html.dark .shiki,
html.dark .shiki span {
  color: var(--shiki-dark) !important;
  background-color: var(--shiki-dark-bg) !important;
  /* 可选，如果你还希望同步字体样式 */
  font-style: var(--shiki-dark-font-style) !important;
  font-weight: var(--shiki-dark-font-weight) !important;
  text-decoration: var(--shiki-dark-text-decoration) !important;
}
```

## 多主题

除了双主题，还可以支持任意数量的主题。在 `themes` 对象中加入多个主题，并用 `defaultColor` 指定默认主题。

```ts twoslash
import { codeToHtml } from 'shiki'

const code = await codeToHtml('console.log("hello")', {
  lang: 'javascript',
  themes: {
    light: 'github-light',
    dark: 'github-dark',
    dim: 'github-dimmed',
    // 可以继续添加
  },

  // 可选配置
  defaultColor: 'light',
  cssVariablePrefix: '--shiki-'
})
```

生成的 `span` 会带有对应主题的 CSS 变量：

```html
<span style="color:#1976D2;--shiki-dark:#D8DEE9;--shiki-dim:#566575">console</span>
```

接下来需要根据父元素的 `data-theme` 设置变量值，例如：

```css
[data-theme='dark'] .shiki,
[data-theme='dark'] .shiki span {
  background-color: var(--s-dark-bg) !important;
  color: var(--s-dark) !important;
}

[data-theme='dim'] .shiki,
[data-theme='dim'] .shiki span {
  background-color: var(--s-dim-bg) !important;
  color: var(--s-dim) !important;
}
```

[演示预览](https://htmlpreview.github.io/?https://raw.githubusercontent.com/shikijs/shiki/main/packages/shiki/test/out/multiple-themes.html)

### 无默认颜色

如果你想完全自定义颜色，或者不想用 `!important` 覆盖样式，可以将 `defaultColor` 设为 `false`。

```ts twoslash
import { codeToHtml } from 'shiki'

const code = await codeToHtml('console.log("hello")', {
  lang: 'javascript',
  themes: {
    light: 'vitesse-light',
    dark: 'vitesse-dark',
  },
  defaultColor: false, // <--
})
```

这样生成的 token 会像这样：

```html
<span style="--shiki-dark:#D8DEE9;--shiki-light:#2E3440">console</span>
```

此时 HTML 不会自带颜色，需要你自行编写 CSS 来设置。

你还可以用 CSS 变量控制主题，更多内容可参考 [@mayank99](https://github.com/mayank99) 在 [issue #6](https://github.com/antfu/shikiji/issues/6) 中的研究与示例。

## `light-dark()` 函数

你还可以使用 [`light-dark()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/light-dark) 函数，避免手动维护 CSS 变量。

将 `defaultColor` 设置为 `light-dark()` 即可使用。在这种模式下，必须同时提供 `light` 和 `dark` 主题。

```ts twoslash
import { codeToHtml } from 'shiki'

const code = await codeToHtml('console.log("hello")', {
  lang: 'javascript',
  themes: {
    light: 'min-light',
    dark: 'nord',
  },
  defaultColor: 'light-dark()', // [!code hl]
})
```

:::info 兼容性提示
`light-dark()` 是较新的 CSS 功能，部分旧版浏览器可能不支持。可在 [Can I use?](https://caniuse.com/?search=css-light-dark) 查看支持情况。
:::
