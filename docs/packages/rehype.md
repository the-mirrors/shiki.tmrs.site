---
outline: deep
---

# @shikijs/rehype

<Badges name="@shikijs/rehype" />

适用于 [rehype](https://github.com/rehypejs/rehype) 的 Shiki 插件。

## 安装

::: code-group

```sh [npm]
npm i -D @shikijs/rehype
```

```sh [yarn]
yarn add -D @shikijs/rehype
```

```sh [pnpm]
pnpm add -D @shikijs/rehype
```

```sh [bun]
bun add -D @shikijs/rehype
```

```sh [deno]
deno add npm:@shikijs/rehype
```

:::

## 使用方法

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"bed390726b08c7e815906765ca83f04afc86b126000bffb68dd03de251b9529f","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808BLAWywlLQAJSZ9sYAyvkYBrRpRBw0AQw5IA7FQA2MMAHM0+JAEZtVGaTUwGiEDz44ho8csZhciAAxUAxvlnSXNconkBfCnR+PEIScn06ExAWNg5uXn4BNFI7NUYAMwwJKVkTABYATmVVDS1EbQBWfVkjKPNE5NSMrNt7HVd3Uk9vBQCgnBDiMgkaeiZWdi4eZlkRAAVZOFwqHLlEAoA2YvVNHT1qGuM8admF0iWJJTsHACYOjy9h3z7qYNNQ4Yix0xjJ+JnSCIAEoJAYrAwmbQAZiqIBUOzK2juB0MR1MJ0BIIsyzh1yQUPuXUePn8gVeA3eQ3C1Ei41iXAArmAMowYFBshCdPIABzbUoKaqoqJMllsy54xB5QndJ4FF6YCkEKkjWmmFwQMBSTjpRgqRCcaRgFqSTmIG7aZxwkq7SWC2p4HUqcVtRBbEBuB49V3yt5KsIq77RCZxEU6sXg3JIG43AlWhEClH20yh1ns1oON0eole7RyskKhx+z40wOG42rEwxt3w/mIWMGJMgBkXdMJrMynxQn2Kj7U0ZRX5xDHzRY4itRvKWms23R2tFmGAAkfnHFXF2Kd2dDu9fO+3sBqJljmRs0VavWsr1w7Clu4l28zeep5d3c95VfAfBqaL2ZY4IRtYbg2KcLx0ZEG3nYc/zBO8HAfdtiR3fpC33D88CPADK3kDdp0vOcb1XCUikfbNn27FD3xLT96XibErDEY9AO5fZcL2fDjlBQRhAY1t1mlRDnlfCj/TQ0wAAosFICAcA4DAAEpOE0Rd4H1YAAB0wE4LTOCuNR8DQfUpBSdQAG4NO0zgoFmQzGlMjS/EYkwoShED41ddjTCU5h4GdBxZxI7dJXIwYRKovAJKkmTMAU3T9Js4y1Ec/E8nPNzYQgqJYoYXikX4r0NmCylQv7cLJOkshoss6zOCM1IkrreRUtrKVE3nKzAV8nRYwQ/LCqLPtVRADCTRPPJcrjWsryFPBm0Il1tEzLcBJfZCQuLEqfi/WiGgS5p6rGpqZ3S68OLo2y0kyTrykWp8ST61CwtMYbx0lKFDrKDcMrwMqXHgBAco3HqyKEtaBtLI19oqVza3Ak7THSf7YK5PKnm0UlVqK9bBue008gqWNWMQB8vvRGBpCgAAxXU5r8wGlq9G5HD8ABdVxoELQdvzo7jGG1KTmE4AByAABOAeYAKzgAB6eocEFjTOe2yxzuaPmIAFwXZZgABaWr1GaeXmS24cziWNWNeHbWsFHQ3FagzjzaFy2tdtrbgE4FM2U4PxHcFz2oENjT1U1LhHRgTgAF4DQAd2kRhGWZMMoDEuTzM4AA6WaxJN0dU80jOs/t7E860zOlmzzj6MYChOHU/OtKlqXOEAfTlAHvlQBTuUAejNACvlQBIc0AVWVACo5QBuOUADIzABC3TgAAMvJgKfOEAdW1AFbrNOtNnuBVNX7Ssv1QWiHjv6daywWKC3rT2pEXf95oOAlm1i+T63vw078EuC/LrWkl2zI3/Tn6/rEtIWO8dtRwHTjwcmVMVBiUFunKWdgsAMjQOnZgAc5JyQkN5GQSBQCRFUHARgGo8BoAQH4PwQA==="}
// @noErrors: true
import rehypeShiki from '@shikijs/rehype'
import rehypeStringify from 'rehype-stringify'
import remarkParse from 'remark-parse'
import remarkRehype from 'remark-rehype'
import { unified } from 'unified'

const file = await unified()
  .use(remarkParse)
  .use(remarkRehype)
  .use(rehypeShiki, {
    // 也可以是只有单个主题的 `theme` 字段
    themes: {
      light: 'vitesse-light',
      dark: 'vitesse-dark',
    }
  })
  .use(rehypeStringify)
  .process(await fs.readFile('./input.md'))
```

`@shikijs/rehype` 的默认导出使用了 来自 `getSingletonHighlighter` 的共享的 `shiki` 实例，在整个进程中存在。如果你想要完全控制 Highlighter 的生命周期，使用[细粒捆绑预设 `@shikijs/rehype/core`](#细粒捆绑预设)。

## 细粒捆绑预设

默认情况下会导入完整的 `shiki` 捆绑包。如果你使用了[细粒捆绑预设](/guide/bundles#细粒捆绑预设)，你可以从 `@shikijs/rehype/core` 中导入 `rehypeShikiFromHighlighter` 并传入你自己的 Highlighter ：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"49310968edc42da1bff9c4461853cbb816c6f0f322995d816abe11237a3bad10","data":"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"}
// @noErrors: true
import rehypeShikiFromHighlighter from '@shikijs/rehype/core'
import rehypeStringify from 'rehype-stringify'
import remarkParse from 'remark-parse'
import remarkRehype from 'remark-rehype'
import { createHighlighterCore } from 'shiki/core'

import { unified } from 'unified'

const highlighter = await createHighlighterCore({
  themes: [
    import('@shikijs/themes/vitesse-light.mjs')
  ],
  langs: [
    import('@shikijs/langs/javascript.mjs'),
  ],
  loadWasm: import('shiki/wasm')
})

const raw = await fs.readFile('./input.md')
const file = await unified()
  .use(remarkParse)
  .use(remarkRehype)
  .use(rehypeShikiFromHighlighter, highlighter, {
    // 也可以是只有单个主题的 `theme` 字段
    themes: {
      light: 'vitesse-light',
      dark: 'vitesse-dark',
    }
  })
  .use(rehypeStringify)
  .processSync(raw) // 也可以同步处理
```

## 功能

### 行高亮

::: warning 警告
已废弃，在 `v0.10.0` 版本中已被默认禁用，并会在下一个次版本（minor）中移除。应该考虑使用 [`transformerNotationHighlight`](/packages/transformers#transformernotationhighlight)。
:::

除了支持 `shiki` 的功能以外，此插件还支持行的高亮。你可以以 `{<line-numbers>}` 的格式在代码块语言标注后指定你要高亮的行；以逗号分隔行号（`<line-number>`），并用大括号包裹。每一个行号可以是一个单独的数（如 `{2}` 会高亮第 2 行， `{1,4}` 会高亮第 1 行和第 4 行），或者指定一个范围（如 `{5-7}` 会高亮第 5 到第 7 行，`{1-3,5-6}` 会高亮第 1 行到第 3 行，及第 5 行到第 6 行）。 例如：

````md
```js {1,3-4}
console.log('1') // 高亮
console.log('2')
console.log('3') // 高亮
console.log('4') // 高亮
```
````

### 行内代码

您也可以使用 `inline` 选项对行内代码进行高亮。

| 选项                    | 示例             | 描述                                      |
| ----------------------- | ---------------- | ----------------------------------------- |
| `false`                 | -                | 禁用行内代码高亮（默认）                  |
| `'tailing-curly-colon'` | `let a = 1{:js}` | 使用 `{:language}` 标记在代码块中进行高亮 |

在 Rehype 插件中启用 `inline`：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"fb222cc263fa9a948aebeb15477862698264941fcf81d85841a6921e806894f2","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808BLAWywlLQAJSZ9sYAyvkYBrRpRBw0AQw5IA7FQA2MMAHM0+JAEZtVGaTUwGiEDz44ho8csZhciAAxUAxvlnSXNconkBfCnR+PEIScn06ExAWNg5uXn4BNFI7NUYAMwwJKVkTABYATmVVDS1EbQBWfVkjKPNE5NSMrNt7HVd3Uk9vBQCgnBDiMgkaeiZWdi4eZlkRAAVZOFwqHLlEAoA2YvVNHT1qGuM8admF0iWJJTsHACYOjy9h3z7qYNNQ4Yix0xjJ+JnSCIAEoJAYrAwmbQAZiqIBUOzK2juB0MR1MJ0BIIsyzh1yQUPuXUePn8gVeA3eQ3C1Ei41iXAArmAMowYFBshCdPIABzbUoKaqoqJMllsy54xB5QndJ4FF6YCkEKkjWmmFwQMBSTjpRgqRCcaRgFqSTmIG7aZxwkq7SWC2p4HUqcVtRBbEBuB49V3yt5KsIq77RCZxEU6sXg3JIG43AlWhEClH20yh1ns1oON0eole7RyskKhx+z40wOG42rEwxt3w/mIWMGJMgBkXdMJrMynxQn2Kj7U0ZRX5xDHzRY4itRvKWms23R2tFmGAAkfnHFXF2Kd2dDu9fO+3sBqJljmRs0VavWsr1w7Clu4l28zeep5d3c95VfAfBqaL2ZY4IRtYbg2KcLx0ZEG3nYc/zBO8HAfdtiR3fpC33D88CPADK3kDdp0vOcb1XCUikfbNn27FD3xLT96XibErDEY9AO5fZcL2fDjlBQRhAY1t1mlRDnlfCj/TQ0wAAosFICAcA4DAAEpODsNcYH1KQUnURiTChRwQPjV12J+MBlOdBwLX4r08nIwYRKo9CjU0/EKmI1i6wMptb2M8pYwQr0X2Q6zi37Ok/nqSxGnUZoHLrDZYxcyo3NCwRwrSTITJ0bytwE0l/MpGygtMDCTRPKFuVi0DfDcySIBceAEF47QpRI7c6ys3LAtVEBCvHOsChw8rwOvB06tgnRGp8p5tGy8lhPa0t7MwpBJ102sHwguoYGkKAADFdUIl0GvMp4bkcPwAF1XGgQtB2/OjuMYbUpOYTgAHIAAE4DugArOAAHpEuegAdZkv1ohp1JSjAHogJ7nsSgBaNSmkyQHgZo4cziWKGYeHOGsFHFHrv+X9OKxl6cf+oHCeATgUzZTg/FJ57aagFGgfVTUuEdGBOAAXgNAB3aRGEZZkwygMS5KBzhOAAOmbGAxPR0dJbAaW5aWRWf0xTiVbV+XNdu6wKE4YApelxSjOufVnpkXVUjhlwGVIJQMAdiAlA1Z7jZ+n7OEANCNADdFQA1uUASATAE8Ms3pZ92WZZls2/F12X9cSpJweaROZaqmq4DgMTpEF4XtTgGWeE2naVDE56ZZ+uwsAZNAZeYFm5LkiRmGMaQkFASJVDgRgNTwNAED8PwgA"}
// @noErrors: true
import rehypeShiki from '@shikijs/rehype'
import rehypeStringify from 'rehype-stringify'
import remarkParse from 'remark-parse'
import remarkRehype from 'remark-rehype'
import { unified } from 'unified'

const file = await unified()
  .use(remarkParse)
  .use(remarkRehype)
  .use(rehypeShiki, {
    inline: 'tailing-curly-colon', // 或其他选项
    // ...
  })
  .use(rehypeStringify)
  .process(await fs.readFile('./input.md'))
```

然后，您可以在 Markdown 中使用行内代码：

```md
此代码 `console.log("Hello World"){:js}` 将被高亮显示。
```
