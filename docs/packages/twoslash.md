---
outline: deep
---

# @shikijs/twoslash

<Badges name="@shikijs/twoslash" />

适用于 [Twoslash](https://github.com/twoslashes/twoslash) 的 shiki 变换器，用于在代码块内提供行内的类型悬停显示。

[TwoSlash 注释参考](https://twoslash.netlify.app/refs/notations)

## 安装

::: code-group

```sh [npm]
npm install -D @shikijs/twoslash
```

```sh [yarn]
yarn add -D @shikijs/twoslash
```

```sh [pnpm]
pnpm add -D @shikijs/twoslash
```

```sh [bun]
bun add -D @shikijs/twoslash
```

```sh [deno]
deno add npm:@shikijs/twoslash
```

:::

这个包是 shiki 的一个**变换器插件**，这意味着对于每个支持传递 shiki 变换器的集成，你都可以使用此包。

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"566a6860d87393bdafd75b0be69dc4862d9cdc033ac1bb26c7233764af38cf7a","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808AKAQwBsBLZuASgAIAzAK5gAxmnYQwvNKWZg4/CKQC2ZACoB3CHFZd8jCFnGS4AfkS81s+YpXqtOvQEkwsWgHkjE+dwsBlfHYAa3YrOQUlVVIAHTB2ZSwlNGlrCLtSTW1dOHxKECgIEQREEAAxZjElDAFhMW9pCF4RUhhmGl5mXgDg9hTw2yiBJWkHbPxedjAaAHNZY3kAOjy4NGZSBkQARk2qVhgwabRcrYBOKlXSaZgN6lSB+yy9PI4wXC2qEXw1ippyRAAmAC+FHQ2DeBGIZDyNHoTDYnB4TRMyRE0BgaggAAk0MpWBZGKjYBYVqRJtMKLxDPM4BYAMJojGYrhoTzUgA8ACFhFA9lAADJyaaCZhXClc1y8tT4GCqAB8fAAvLLeAAFUgQZTsOAwNkksmy2LxRLrJGwRk41jLC4bADsAAZdvtDsdNg7qGsrjdCeisRbnpM3gAWD5fWRiKEA4GgnB4QgkcjnOhe5G8fAW4kyMlWtYbV3BkB7A5HJD5i6e2N+3YBpA2kPfcN/ABsUeoYNjkIT1CTcI4XD43vNuIJaIzpIOFKp3hpvHpZqxzNZU853N5AoOwtFvHFPJgUClMpg8osao1Wp1eoOBriCSSpp92Nx2fWSG2tYLTuLWzdZeueAHvsfKtXhrOsw1+F8ThbTAYxKOMoUTWESkYLB1RwdYMD4XQDgsaJqDgXCn1zQMznfItjlLD1fxKLDpn9YDEAADlAn4IyBEFWxgiF42hbskJQwwyEwPhZxgRcTD8Mk9n3VRlwlXdpMPBYjgPCwFIAJRgaYtRkNpvAAQTAaoAB8ukzA4+XYX42AAVTiSRZJ3PdpVUCkL2mWVCJfRsAGZHTIpAAFZzkom5lNUOi3hIz563AyN2Og8E4M7GEbmQ1DBIwyw7kiMgxMWHSbBy0gzH8QIQjCQr0mQABdXgTO5GB+ADKA8gKIo8AqtIojgIZSGkaUujK3osHYHAXhgJYqBWHMXwYpjSOdF9/mCy4qNufoioQIC3ldZiGyQNjo0SjseMQkAWF7RECq6h5HByAwvBMcwso29JMju/AXDcPKeFKnpOvuGJr2NZJrsB96xlawpijKH4qhqUR5gaJoWjaGAOkGno+kqwZbBGR4cgmKZNLmKdJpAabnwBO15sLRbThW8sSjBoqIaebaXx2EBorAiNA0BaqPjRPAjVvYBYl4bGboyUY9AoWJAQEdVlF4AByAABHIegAKzgAB6NBZZyVXDRvE1xakO9B1YeWwEV/hlbVrWQhNsBYlReRkjTXFeAVDoNGYSyrYA1hGAAAw9uAID2BZWAgaZGG4MOKQtyWaIsVW0DgVXbclsKYAzohLPgbUAFooDWIIc4lqXAenZAa7z7K3qN/RuApPW9d4NlS9L3hAE34wAZxMb3hO94BYJ5rwWFe4PJVFWJBQCTfY4G8PAs5AQFASAA==="}
import {
  transformerTwoslash,
} from '@shikijs/twoslash'
import {
  codeToHtml,
} from 'shiki'

const html = await codeToHtml(`console.log()`, {
  lang: 'ts',
  theme: 'vitesse-dark',
  transformers: [
    transformerTwoslash(), // <-- 这里
    // ...
  ],
})
```

默认的输出是没有样式的，你需要添加额外的 CSS 来装饰它们。

如果你想在浏览器或者 Workers 上运行 Twoslash，参考 [使用 CDN](#使用-cdn) 部分。

## 渲染器

由于 [`hast`](https://github.com/syntax-tree/hast) 的灵活性，本变换器允许你使用 AST 来自定义每个片段在输出 HTML 中的呈现方式。

我们提供了两个内建的渲染器，你也可以创建你自己的渲染器。

---

### `rendererRich`

[源代码](https://github.com/shikijs/shiki/blob/main/packages/twoslash/src/renderer-rich.ts)

::: tip 提示
自 v1.0 开始，本渲染器是默认渲染器。
:::

此渲染器提供了一个更明确的类名，前缀为 `twoslash-`，以便更好地进行作用域的限定。
此外，它还在悬停信息上进行语法的高亮显示。

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"9bafe2d835b3a768cc39f5a28647084c02c621d41bbebd6324dead19238c244e","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808AKAQwBsBLZuASgAIAzAK5gAxmnYQwvUjDCwZpAErsR+RhCzjJcAPyJei2fLLLVAeU0SwPfQBUA7hDisu+Q3LJkAOmHYBbLAhSNGkjTyUVfEoQKAgRBEQQAEEpNhpSMGZxElCPBV40fCzeLFJidnk4XgAjGDR0kpl+dloYKF4RFzgqzL94Ch97dkLeOAwwNGZaXnx2AHN8DgWQ/iCC/BhedjBVgro0ADpouEngpABOKlZZOcKkAEYAJipTubq8GTyTSOiOMFxEAAGKiqZikZhiMgXAC+FHQ2ABBGIUJe+yYbE4PAEwjEVgK4Osq1IfVIDicXTUGi01j0vFsBLgRJJZOcrgAkh5aBZqTZeABlWYAa3Y9OYhKCJJ8/kCwXxYsZErILIp0Vi8TwADEIWgghhsaJqQUIB0ZFlNsx+UL2HLxcSyAI1mhHKy4PgthMYHNwTyjlQTmCGIhHgBmK43O6Ie6Xahgt6B6gMplK50qq7bAHAkCg8GQ8hB4Ow+E4PCEEjkVH0dEcLh8NCJxWklOudSWbS00W25lN10c2Bc1vWbj6AXsYUdhV20hSgJBEJ1+VJxvk1yquIJEBasS6/W4yRGk0wM28C0j4U2ickh2kArdt3bGherJWOC+kD+s6IADs9zDYFuUUjaNXneRJ507ZNl1dX50yQZ4syKHN0iQQFC2oBES2RctqDRRJGFKDQyEwPhx0XZVXG5Z8Dk+YxSHbW93Bo3gAB9eGEWBmn+KBV3VRIAGFBBOCA/FyGiqh1XhYBECpNkIexeEPVR3V2V0IEEVh2lqETPC4v1TkDc4AFZf3/JAAA4XljECQGo8JoP+JBQ3gsFtShINUMwYtElLFFsMrXCMRrLSFFMSkB10fQGPCEKKO0Ic6XosIFGnGUQhs4KfioNV1xSY9WHSTJsk2NL7UKYp8KIaTSCqWp6ntUoYGaVp2k6LgemYPo4AGMAhhGMYJimGZ5kWIaVkdDYlONGh6Ffd9A3uQEjJAa4/wjJ4LNIOMPkS75VDsgFHOzFy8yeaEAF0QWgRFpVnXhgCCnb8Aoc9SNvXhoQEMphIAcgAAVdUd2AAKzgAB6J1IPwL6fB8MCLwgl01GAHxeHu0h9GKiJVEYPgQZB3gAB4AFpCZ8aFuGiPpJiQUB9lkOArDwNAEGhaEgA"}
import { rendererRich, transformerTwoslash } from '@shikijs/twoslash'

transformerTwoslash({
  renderer: rendererRich() // <--
})
```

这里有一些使用内置 [`style-rich.css`](https://github.com/shikijs/shiki/blob/main/packages/twoslash/style-rich.css) 的例子：

<!-- eslint-skip -->

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"43c9a2581bdd8d8c26b6a00be668c361b400ca9c2034e576e137e282e9ec30ed","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808BLMG0gMwEMBjGAAgBVoESiDhoOpBogCMABioAbGGADmafEgAsVcaRUwpIQVGGLmuRPJBd8E7qyRyAvhXTYLBYmRE16eABRYpBA4khgAlAJCAHRojGhKiLxipMwqImISUtIAnIrKahqIAKw6EvqGcQm4ZmAW0lQ2dlwOiABMLm44eIQk5Dp0hlwQYGK8aEJJAEowHCZgChgAPMYQAHwZulKabfmq6lplegZ4EyYiCuaH1rak9t6IAGyd1O49Xv3Ug6fuvDNzI0WKzWvAAvLxgLxSLN5oteMgAAq8Zi8ADWMAwEDYAgAukl+IicQBuXhOEQmLgIRAgACyHHRvA4CgUvCCITIcXgyLAAihMMBGE2WS0AA49oUkGLqOUTtT/rDBbULNobs1WtIOq5Xt1qb1vAM/NTmKxODwoucqJlJEhigB2cUHRAq3QVPCrC5XJ2NW73cjtaQvTA6zx9HzfamBYKhTCRKqJZJoVKqIXWp55EBKfZFUrS46VeJKD11G3etUPTVdDwARwArmRFV9DSBI+ywrGCzAkik0impI905mJSUjq7qXGahnPTmmncWuXA29dR8w03/ABbAyEKCRADKibSsQgAFUsKEAMIcOAwfzhLv75NUClUkBnkZ9NBwRnM8b4PhMrC2AARgYjBcLwM6+p+KIcAmSYqOMEC8DWp5kFwl4wNEvaOLIViDo60gNLmo7UMeKGkBeV5FhY04+nOfoAMz0QuwZ6p8vhDCMYxnBA0z8gsyyrBslpbI40hSnhRTOjKlRCFRkqlrOrSyMxHhkMEnxWtkbT0Q62aNNAFhtMUmhWGwjCZhwG5MGAsC0LECBUIwUB4GpAC0Rkma50iaLarnFCuhgXmAkBoIycBwIwKg8hMvAAOTjrFvDAWhNZXsioWMJ+MHQnMrkCqyUYchgmH5CQCguaQ6lyYgUoQXRNoqe8oYGoYLbRhE4wdkkHBgA2mmONpuklkRsrUB21W1bRrTFI1S7NY2hhEBIvAAHI1muwGkEka0bWQr6jImNYtBAnxPngACCPIQIBABWMAtD+HChdCQTwMoH6MrwYDrZtvBYoyvVoswUDRLw53fgAUhwS07lwqRYKF327aQWXQrwjyaK5gHxLwbAKBAT1pK5WAQMaX0/WQcAlaIIkyCZQ1PCOo07Zt1VyApvpIMpWpBh4rEBQEG7qNAkQs3tnGHcdpDRFgEhXgAkiw/jdqod5wRQUJzIwtAAPxJEjm23uTyPkhAlJ4Pt76fudsFpAhAPcjQ+jSz4HAqFSyDICAst3GumxwWDtuqPbwxgO+juITBBtkJhOIUF7PuWSIdxQNrgdLQodZJQYADuMDKLwbQA1AvD0Y8j2hXAOBcIwZlcuofCAehf3Yg3xu/SiAAGKsqJ30QADpgPLrf4JljJ6Ot73Ip+IXJMhWCXDAUAaz3n45/E+Cfa9Zm0C3cWyLQiUSHwocRbA0Il7+tBzPdjBrkyA9gODLIQA3pBB+748n5xTlkEvvCwBrvfBQsc4402FHTe0GYCiOilC6UaPsFYsDZlYOqrRbSzWsBANcC8QIjCwnTcSMCiioOwbguInEkCoHAJZDwiCYAADF8ZPREKiYGeAhZblYcDGk0Ba6MEpngQBChj4ZBOmgfg4YQAERAC8MAtC8D0MVgwKgbCbIcM3NAbhNleGpzrijIR90RHQjEZISRTYZFyIUdSNkExebaOcjYwqYQHG6P4YI6kwjRGWnEeYwwliwHDBwUoChowETQh3oolBHN6o1ScIEgy1kTTcD4KsCEg9eCdWqGrNIg8nCD0HqfUK3FeIAn4isIQIJwTAAyVk+MsUAAiMAQl8GYPYRgJAkJXhRrFQ8J5zzoRvBQPJBSWAxHHGCOKAAJZp+NYqjLFtLJRStYoanorFDWchwijMybsvZ+zeAAHpDm1IOWc45IghYcCQKAQYygIr4LHAgJwTggA="}
// @errors: 2540
interface Todo {
  title: string
}

const todo: Readonly<Todo> = {
  title: 'Delete inactive users'.toUpperCase(),
//  ^?
}

todo.title = 'Hello'

Number.parseInt('123', 10)
//      ^|

               //
               //
```

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"9b2ea5421c3bea4230c67cb0adfdbeb66e3179c76a4f9cc51a280c8d06f179c6","data":"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"}
import { createHighlighterCore } from 'shiki/core'
import { createJavaScriptRegexEngine } from 'shiki/engine/javascript'

const highlighter = await createHighlighterCore({
  engine: createJavaScriptRegexEngine()
})

// @log: 自定义日志信息
const a = 1
// @error: 自定义错误信息
const b = 1
// @warn: 自定义警告信息
const c = 1
// @annotate: 自定义注释信息
```

---

### `rendererClassic`

[源代码](https://github.com/shikijs/shiki/blob/main/packages/twoslash/src/renderer-classic.ts)

此渲染器与旧版的 [`shiki-twoslash`](https://github.com/shikijs/twoslash) 的输出一致。

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"be50298f5ea6976d501abea603d4b03abe8280f3bf0a7b10df97e47172449e7c","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808AKAQwBsBLZuASgAIAzAK5gAxmnYQwvUjDCwZpAMKsucdiMbdEvACoB3CHBVx8AJVnyyAHTDsAtlgik00i2TLLV6yiCgQRCIggOvgwvLD8zIKsLjJy7qS8bOwA5rZgKbx67Gj4vLlhTqnsYGy8AAYAAibsANbsAFZwAPRoBkZc+OW8EIJoWH0AdD5waMzOSACcVKyyKblIAIwArFRjpCkwDEFxlkrGaiI+HGC4iAAMVCL448xiZFMAvhTo2GcExA9rdNsgLBxcPhCUTiST5UjMMBwfhOOxkfSGYz4RgQLCgqEAfm0OghUJhpDhpARHRMAEl4rQAPJoiRQrS8ADK+Dq7BxkOhsOstgcThcaFxHIJ8PaSJ8fgCeAAYnc0E4MAJhGJafkILwRDJmDQkozmfVwez8YSBE58iLOrwSjQUhD0XBhlRRuNtgAmABsMzmC0Qi2m1HGm1+/INnKJZpMxxKZ0uIGut3u5EQbuerxweEIJHI33oTGSgP1eJDxKRKJpkjgWN0AsNwsRnXJsCppbp2iZLLZBaFpBs9kcznzgsJRc6Yv8gRA0rEcoVIOVsrVGq1zB1LP71cS+NNtZMFrAVpttLtI3W2wA7AAWD0ZL0+tb+rZ4IMdwdh/AR05IZ1XG4Q+NIc7J6g3jTT5M2oH4mCwUhUTITA+HbAcaxJfBqVtQZdgSCshxMcx4gUXgAB9eGECJIygEcJSCRRBFGCA7FcXCyDgFVwhgER2FgXhCD0XgYDuPIShhXgTF6VgoF4AAjMJ0JkMiHWPKYAA5L3mV9ECUv0NnvHY3AUN8zgAZi/OMaATZ0AMwVMgnTL4wOzIJ/k4Hh6L2Tw4EOTRsRfHC9m7Hk+2kjwDm8KhxTHEIwgiKIYmchIkg4NISkybJcnyUIelIYpSlYCpql1RoWjaLcuh6PoBjQe0QEdCZvXOdTZivVSVlvTTfgC/YvCOGZIyQQyY2/GUHm9MyAF0rmgd4e15XhgBihRXMOChV0LF9eEeAQoLogByXKWSaVoX02mwbEfBDQyKxhgBsXhZrIbQ2vm9RNF4Zpml4AAeABaD6bEebgfDhMYkFAH5ZDUSQHwQR5HiAA="}
import { rendererClassic, transformerTwoslash } from '@shikijs/twoslash'

transformerTwoslash({
  renderer: rendererClassic() // <--
})
```

你可能需要引用 `shiki-twoslash` 的 CSS 来装饰它。[在这里](https://github.com/shikijs/shiki/blob/main/packages/twoslash/style-classic.css) 我们也提供了来自 `shiki-twoslash` 的 CSS 拷贝，不过它可能需要进一步的优化。

### `rendererFloatingVue`

[源代码](https://github.com/shikijs/shiki/blob/main/packages/vitepress-twoslash/src/renderer-floating-vue.ts)

这个渲染器使用 [Floating Vue](https://floating-vue.starpad.dev/) 作为浮动组件（在容器外渲染），并生成 Vue 模版语法。这个渲染器不可以直接使用，而是作为 [VitePress 集成](/packages/vitepress#twoslash) 的内部渲染器。在这里列出它，对你可能创建的自己的渲染器提供一些参考。

## 选项

### 显式使用

当与 `markdown-it-shiki` 或 `rehype-shiki` 集成时，我们可能不希望 Twoslash 在每个代码块上运行。在这种情况下，我们可以将 `explicitTrigger` 设置为 `true`，仅在指定呈现 Twoslash 的代码块上显示。

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"c33fc7b243f56e1595f93ca6e9e052fed95abb58a9edc5376b448c62984626f1","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808AKAQwBsBLZuASgAIAzAK5gAxmnYQwvNKWZg4/CKQC2ZACoB3CHFZd8jCFnGS4AfkS81s+YpXqtOvQEkwsWgHkjE+dwsBlfHYAa3YrOQUlVVIAHTB2ZSwlNGlrCLtSTW1dOHxKECgIEQREEAAxZjElDAFhMW9pCF4RUhhmGl5mXgDg9hTw2yiBJWkHbPxedjAaAHNZY3kAOjy4NGZSBkQATipWGDBptFzEAEZt6jXpmA3qVIH7LL08jjBcRAAGKhF8NYqaci2AL4UdDYV4EYhkPI0ehMNicHh9GyRe6OHIGLwmcyWW7IjKjZyuOieeY8fyBEJhJHpWLxRLrRFpKKZVG5KgFIp4cqVUjVISieYNJotNowDpdcm9GT9XFDUgjB45CZTGCzNreOBLKgrNYbACsAHYdnsDkdTlRVqRLtcpVSmfick9Jq8AEyfb6yMSQ95AkE4PCEEjkc10a6MLCkQxkTB8SmMlFjYnqhZ0LAcETsNBWdjTS6kLEAIwgEF2cl4AB9eAAlFUAUVoWHLvGEsH4TqgeXZxRA1YAjoJ2C04LwAAZoe34YeC/Oi8PwPY0KBK6T4UUiaCi/OsQpBXiqVaC5hYVPVQ7sIc2uNB87TYrIZD5GD8ZiCVgMKhP1hwXAAXW/WotGwABzHEa+yHEgxy6uaFxXHgKZphmWY5pCOxOkgADMbo/J6/zOgCf4gGusB4LSSS8MADJ3HiCrjACAgRsovAAOQAAI5D0ABWcAAPRjjRTGxLEF5UcyYyMMAsS8Lw8HsOmmakNmuYWDIgiitx3G8AAPAAtNpsQAtweR7swSCgCGexwN4eBoAgAIAkAA="}
import { transformerTwoslash } from '@shikijs/twoslash'

transformerTwoslash({
  explicitTrigger: true // <--
})
```

````md
在 Markdown 中，你可以使用如下的语法来触发 Twoslash

```ts
// 这是一个普通的代码块
```

```ts twoslash
// 这是一个运行 Twoslash 的代码块
```
````

## 集成

你可以根据上面的说明自行将 Twoslash 与 Shiki 集成，也可以在这里找到与框架和工具的高级集成：

- [VitePress](/packages/vitepress#twoslash) - 在 VitePress 中启用 Twoslash 支持。
- [Nuxt](/packages/nuxt#twoslash-integration) - 在 Nuxt Content 中启用 Twoslash 支持。
- [Vocs](https://vocs.dev/docs/guides/twoslash) - 在 Vocs 中启用 Twoslash 支持。
- [Slidev](https://sli.dev/features/twoslash#twoslash-integration) - Slidev 具有内建的 Twoslash 支持。

## 用例指南

### 使用 CDN

默认情况下，[`twoslash`](https://github.com/twoslashes/twoslash/tree/main/packages/twoslash) 在 Node.js 上运行，并依赖于你的本地系统来解析 TypeScript 和导入的类型。在非 Node.js 环境中直接导入它将无法工作。

幸运的是，Twoslash 实现了一个虚拟文件系统，允许你提供自己的文件以供 TypeScript 在内存中解析。然而，在浏览器中加载这些文件仍然是一个挑战。在 [TypeScript 网站](https://github.com/microsoft/TypeScript-Website) 中，TypeScript 团队提供了一些用来从 CDN 上按需获取类型的工具，他们称之为 [自动类型获取（ATA，Automatic Type Acquisition）](https://github.com/microsoft/TypeScript-Website/tree/v2/packages/ata)。

我们简易地封装了它们，并在 [`twoslash-cdn`](https://github.com/antfu/twoslash-cdn) 中提供了易于使用的 API。 例如：

```js
// FIXME: 在生产环境中使用显式的版本替换
import { createTransformerFactory, rendererRich } from 'https://esm.sh/@shikijs/twoslash@latest/core'
import { codeToHtml } from 'https://esm.sh/shiki@latest'
import { createTwoslashFromCDN } from 'https://esm.sh/twoslash-cdn@latest'
import { createStorage } from 'https://esm.sh/unstorage@latest'
import indexedDbDriver from 'https://esm.sh/unstorage@latest/drivers/indexedb'

// ============= 初始化 =============

// 使用 IndexedDB 和 unstorage 来缓存虚拟文件系统的示例
const storage = createStorage({
  driver: indexedDbDriver({ base: 'twoslash-cdn' }),
})

const twoslash = createTwoslashFromCDN({
  storage,
  compilerOptions: {
    lib: ['esnext', 'dom'],
  },
})

const transformerTwoslash = createTransformerFactory(twoslash.runSync)({
  renderer: rendererRich(),
})

// ============= 执行 =============

const app = document.getElementById('app')

const source = `
import { ref } from 'vue'

console.log("Hi! Shiki + Twoslash on CDN :)")

const count = ref(0)
//     ^?
`.trim()

// 在渲染之前，我们需要准备好类型，以便能够同步地进行渲染
await twoslash.prepareTypes(source)

// 接着我们可以渲染代码
app.innerHTML = await codeToHtml(source, {
  lang: 'ts',
  theme: 'vitesse-dark',
  transformers: [transformerTwoslash],
})
```
