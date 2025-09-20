---
outline: deep
---

# 明暗双颜色主题

Shiki 支持输出明暗双主题，甚至可以支持多主题。Shiki 的双主题实现方式是使用 CSS 变量，将每个语法 token 的颜色同时存储下来。

要启用双主题，只需在 `codeToHtml` 中将 `theme` 改为 `themes`，并提供 `light` 和 `dark` 两个键即可：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"fb15f63f6dc6b9fd5783a292ff08dfdcee24e6bbd3d1aa6a716274a7dabac780","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808AKAQwBsBLZuASgAIBjCGDhoB0GABUIACTQBbVol6NBsJSNLswAcwq8IWNOyFwlAYXFTpXNAHlDx4QB4AQgFcwUVjCgAZZjpuzNoweu6e3lAS+DByMAB8fAC88bwACqQQcuxwME4aWtrxADpg7HJYEKSiqpIy8qyUICLM1UgAnFTeOmj4SACMAAxUaK0hDIggtVYNTRxguIjDU/itzPw05IjtAL4U6NiLBMRkTTT0eILCNeLqaJo6TS1tiAAsAExdMD19byNjMAmU3Ecy0i0+KzWG1OiAAbHsDjg8IQSOQRnQgSwOFw+NN6goVLdeAUdHoDEYTOZLDIbPYKc5wl4fP5AsFQrxGZForEEtwlBksjk8iSiqVypVqmJYDMFE9Ri8AKwKr4/AbLeXjS7U2SyrpgpAQ/irUjrTYDXb7aiHZEnNHUDFMLCZHDVDB8VgBbRKYogABWzCIXH4mkMPrlrQm7VeKu0vSQ0eoAKBHseeoWSAAzFQjVCzYh3girUjJijTuiLpNGE6DGRMHwLLA6Y44ABZNysIxYbzcuJwVweJlRGJxeIAOl6PNM6QjnFYTgASjBBKQoPl7oU9D2YIvtDl7sx6QBBMAYXgAH14AGV1zpfOxNmwAKplIT9iI+Ld6EXxH9NKAQfgEEmQ9eDkZgsH0AAzMRWCqXgwGYXteDQCBkOHeBR1KaIcl4NhYIAdzgXgMAgNxkNQuAcH4dhINPOR207bw0MnXhILgideBCBYTRoKApRgTCwFKAADUS0DgUp8HYbR8A4GTNlHPEdVYQlYD0YBSl4XgUy9XgAHJfTgPSKE05jeyUDSwC0rS5PwNAlD0oh73gXIAFpbLQYzTK0qBWgAawcpyaDgNzfNIPyvKs3gdlKHZuBE0TSlKAB1dhWFYTjvjIA8YEQJKwFE4S7IUUp8iwAJiUwbwkh9QRYNIRAAGJ+hagBuVzXLgKS/PYVywoCxrIKG1qfXiWonAAekogISgK0SzmCIDkGQZoYFwKg7LQLBTAmibd16NwACNFKyKbuvYQyzvYHrGo8/q+qCVhXInXsQAAXTeqhngmFrYRjOM4X+UhNUmF74FBdM/khE1oS2AtLUwYtjlRM4HUrasXTrbTpLs70QGyMB3JxtAwy++UfteP6QG6WNfmVRNgcBPAPIhxZ6ZzGG81eQtEaOUs7XOTEMdrN1eH6vHIBXUnmnJgYAHZlhpgGEw1JnJn61mkCpjnTRhbnPuBWA8HFKpRGAfiZQynZWMyOR9K6672D0/KrhEfjeCSXD8OYe8LfxFS9NdiBvFHWDtEYH0YnSiAfW4YzeEsmzPQc/1AzgYN2EMSKtLBqdzd23hkAAQlqXhZLeN7vOx+SHIJon5Oz6zxf0yWoEbmKwDipo4lGJBQAxb44EcPBxJAHYdiAA"}
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
// @twoslash-cache: {"v":1,"hash":"6d13c5c6025eeb0a00e25d1b667c6c3815ec3b978a5cfb8478aadadf176ed65b","data":"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"}
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
// @twoslash-cache: {"v":1,"hash":"a603cdb1558acfbd3d6826406395c067dd774f17d4cabe66cdbdf134dfb43376","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808AKAQwBsBLZuASgAIBjCGDhoB0GABUIACTQBbVol6NBsJSNLswAcwq8IWNOyFwlAYXFTpXNAHlDx4QB4AQgFcwUVjCgAZZjpuzNoweu6e3lAS+DByMAB8fAC88bwACqQQcuxwME4aWtrxADpg7HJYEKSiqpIy8qyUICLM1UgAnFTeOmj4SACMAAxUaK0hDIggtVYNTRxguIjDU/itzPw05IjtAL4U6NiLBMRkTTT0eILCNeLqaJo6TS1tiAAsAExdMD19byNjMAmU3Ecy0i0+KzWG1OiAAbHsDjg8IQSOQRnQgSwOFw+NN6goVLdeAUdHoDEYTOZLDIbPYKc5wl4fP5AsFQrxGZForEEtwlBksjk8iSiqVypVqmJYDMFE9Ri8AKwKr4/AbLeXjS7U2SyrpgpAQ/irUjrTYDXb7aiHZEnNHUDFMLCZHDVDB8VgBbRKYogABWzCIXH4mkMPrlrQm7VeKu0vSQ0eoAKBHseeoWSAAzFQjVCzYh3girUjJijTuiLpNGE6DGRMHwLLA6Y44ABZNysIxYbzcuJwVweJlRGJxeIAOl6PNM6QjnFYTgASjBBKQoPl7oU9D2YIvtDl7sx6QBBMAYXgAH14AGV1zpfOxNmwAKplIT9iI+Ld6EXxH9NKAQfgEEmQ9eDkZgsH0AAzMRWCqXgwGYXteDQCBkOHeBR1KaIcl4NhYIAdzgXgMAgNxkNQuAcH4dhINPOR207bw0MnXhILgideBCBYTRoKApRgTCwFKAADUS0DgUp8HYbR8A4GTNlHPEdVYQlYD0YBSl4XgUy9XgAHJfTgPSKE05jeyUDSwC0rS5PwNAlD0oh73gXIAFpbLQYzTK0qBWgAawcpyaDgNzfNIPyvKs3gdlKHZuBE0TSlKAB1dhWFYTjvjIA8YEQJKwFE4S7IUUp8iwAJiUwbwkh9QRYNIRAAGJ+hagBuVzXLgKS/PYVywoCxrIKG1qfXiWonAAekogISgK0SzmCIDkGQZoYFwKg7LQLBTAmibd16NwACNFKyKbuvYQyzvYHrGo8/q+qCVhXInXsQAAXTeqhngmFrYRjOM4X+UhNUmF74FBdM/khE1oS2AtLUwYtjlRM4HUrasXTrbTpLs70QCClyYHcnG0DDL75R+jNlm6WNfmVRNgcBPAPIhxZ6ZzGG81eQtEaOUs7XOTEMdrN1eH6vGCZCon+rJ5oKYGBUAA5/t+BMNSZyZ+tZpA/uh00YW5hHrRLW1UYrEAq2dEX63EJsTDbDt2C7SR0L7TkP3QsdYEg5gGIseqAH4lB91hcnPK8b20O8H1YZ9HCcH0PJ9cOfRlyhKoeIoU5ADy+v8xh4pAcOBxgSCwSgP8AKAkBuTF0vfY7MyYFwrAu3YHxyOY/jlCc5heC0eZm+EuqqmEyqMG8bhBNr0h4FECBoI4sGiNaZvwLbjve94MxL0vXhA00ZhDu8OA9ACPiUO0bRIl4Q7Tx3vfbU0WA4EE0oADE4LoRDnb0GjeGEt7BuaB/aj37kRYSHlhJ6AnFZSBJMx5g3AS3DeF9UIcVqCZBCngu6AP8og9CuEcEQAnKQAQbgRBZCbivWeKCOCb04NvXe+9WicGPvAPKQk5pFQaKVaaVkRATxgDVYE9UmrAA8jsdqnVzp53CuI/qUiOpdWur1fgFCUJyHEeoyhcgpGjXGlNcqYBZqFXyslGIAjASd2EiHXI0D4KoSAQxceJ9eD4TShlQ6a9W70KgGfHBKFRjpVPG4CCKFeAULIERCJ69WCng4oIk+nCEo8JKmAMqFUknCJ9Mo2RHlxGSOkSonqciBrAEUcU2ROjNHaI0VkfRIAxriEmvw0xiUuEWO+MSaxET4HyTKQXBxHFnGNxHmQjx6Vb7N1np4MgHcuAANzv1RgjUJEkz2LwNZijuDCTftw4qrA+HGNcTk0RVREDLPzmsyRehtn+TilU1RZSFEPKeaUmpWQ6m6Mac02ArTjHtOEgtbQS0VqjIYFQPSHk9LvU+nLCMAxFYAHYVYDAhOrIEELQF2kHkgZWetYYGh2PC2oeBxRVFEMAfiMoMo7FYpkOQ+kSnsD0vlK4Ihu5JFwvhZg94aX4hUnpDlEBvCjlgtoRgPoYjpQgD6bgxleCWRsp6By/pAxwGDE7TyWCtLLwst5bG8lArOSlsTeSkVrJi38ia4KoV/KWuirquuPs/aiouaxNguQ9C7V4E4DqsVuBNDiKMJAoAMTfDgI4PA4kQA7B2EAA=="}
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
// @twoslash-cache: {"v":1,"hash":"6f759682435215e39c397b749acb7cde986e1a1443cd5d426f26280dae2d5214","data":"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"}
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
