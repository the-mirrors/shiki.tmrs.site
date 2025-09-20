# 代码装饰（Decorations）

Shiki 提供了 **装饰（Decoration） API**，允许你为代码中的特定范围包裹自定义的类名和属性。

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"17add2ca87e9a0d005a3aba43a68dda1ddc4bce2493a58927a72af2029880e0d","data":"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"}
import { codeToHtml } from 'shiki'

const code = `
const x = 10
console.log(x)
`.trim()

const html = await codeToHtml(code, {
  theme: 'vitesse-light',
  lang: 'ts',
  decorations: [ // [!code hl:8]
    {
      // 行和字符索引都从 0 开始
      start: { line: 1, character: 0 },
      end: { line: 1, character: 11 },
      properties: { class: 'highlighted-word' }
    }
  ]
})
````

效果如下（示例中通过 CSS 设置样式）：

```ts
// @decorations:[{"start":{"line":1,"character":0},"end":{"line":1,"character":11},"properties":{"class":"highlighted-word"}}]
const x = 10
console.log(x)
```

位置也可以用相对于整个代码的 **0 起始偏移量** 来表示：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"47fd17de08b4473e25bec5efb46f7c9a87fe8015e912d6600f80379a8aa88210","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808BjCMONAAnzQFsAbRdm1IBLMAHNKINgENSDRADYqvGOLT4kAFippZYmPII9ek3qNyIADFUb5Z0xjXKKAvhXTYLBYmUk16eAAU0mbScACU7MywACoQABLGgdEwAkKiYhTsEFhowixwAgDC0DBx8WFoAPK5+awAPABCAK5gUCpQADLS4s3S+lktbR0x+DDcMAB84QIACqQQ3MJwMPXp4pMAOmDC3FgQclGl5caSMnJIAIwAnMqqYupX1tR6BkzHCafK5kjPtvaOXyIG5uDw4PCEEjkHR0QzMVgcFJpNAicRnXQXRAAZmeKjUGkQ2hepH0cNKph+VhsdlIDicSBxoOonghPmh1FhQSwCxwcgwkRKsBqeQKAGUMipRuNVkN2jAoFKJpMAHTqaUCRUwABKMDEyxR0hFYAAgmAMOwAD7sUUojKdYROEIAVR2LCarTlCrGEyy6zEk3Rsnkmlx90eiAArDpXoY1RMKWALJdqQD6YgAExMzDgxDeKF+Tm5wLcnJkTCRXg9MQCTbUOC1wOYhRKEB4h4EokY0l4Sto76JpDplO0wHOTPuZk5vO+GEBIsl3nl9gAERgzFpRuFdTgytg68N24A/AJV/ujQBJGjcZAAXUt7A9MAAZuYoJIoBBGAhc0VmmxFuwe4HAeBTsGgEDsAA7rSWCcMIYj4GYCE0FAYEQAA1qocBQQ6+DKo28gAByhviVzJsS3a5kBG7bgmFhYsOdJAuOYJeJCM4cnOIDFjyZb8iua7AReV7KucaACFUT5Pis1SkLMEBwA6dTvp+34gDaQbZFJMnZKQ7D7IpRpaWBYyAYJNEsPhVBiVc6YMa2YYElGFFvLmNn9hYzn/COaYKFmLK5ux7L+IYPGlnykSnkJdSXuMyqqFAEnaQYVRyQpSksCpX54AAom0WnSQYun6elRkQE+JkwGZZ51FZUgYvIlwhncpHYtGJKuSACV0UgLbeUxzh+RO2ZsWyBZcWFi78VFFlgLF3DKguZbCPAx7sPM4V5PA96Pi+iZvlQH7ZbmG2Lit2HlZV7AwCoExgBw4FRKQMCGjAdU2cCEbOW24aXM8XadUtcjnT1iAAOyMaOvX+VOQXjaFQNLowlZwIUgi2n29VBlcCi3A5rXOQDcIowgHlIBDID9VDGaWC4N42OSubwmwnDGOwAC87DSJB0gOkcsSfHwySlFkwDbOwlUTAIADkRAOvAKwALRIVw0sUOL7C9tW7DS2gcBqxr1EgawAjIOwAD05vsMgACEKScPwYM3hrEti2AEse+jQYCOmyYux7CU+9o/sS4j50CMAUQkzL+DwYhccoYrkEHFA0vsC4/sZ+77DO2ALjhJIEy6EgoCwlhym5nrIAuC4QA=="}
import { codeToHtml } from 'shiki'

const code = `
const x = 10
console.log(x)
`.trim()
// ---cut---
const html = await codeToHtml(code, {
  theme: 'vitesse-light',
  lang: 'ts',
  decorations: [ // [!code hl:7]
    {
      start: 21,
      end: 24,
      properties: { class: 'highlighted-word' }
    }
  ]
})
```

渲染结果：

```ts
// @decorations:[{"start":21,"end":24,"properties":{"class":"highlighted-word"}}]
const x = 10
console.log(x)
```

**负数字符位置** 表示从行尾开始倒数，`-1` 表示最后一个字符：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"f445be6013dcc1515a5189fe0a44669ee748465f55a6563c00cc29cd14e82b74","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808BjCMONAAgEsaBbRdgCIxmpAIZpOLAJK9KINqNINEANioAbGGADmafEgAsVNIu0xlIbjB5z1nMLkQAGKo3yLRjGuVUBfCujYjgTEZHI09HgAFKJ2onAAlFxg3gBmnjCCwhBiEtK8ADpgnDxYORxCIuKSYDLWcgpKSACMAEwaWrr6iM1G1KbmeJU51fn1GvaOLiBuHl5hPa3+gTh4hCTkxnQWUVikEDhKGEnDuTV1PAB0jWj8APKpqXDmd6QAChBw3DVyUBCMCEQIAAyiYlOwII9nhwcuwyl88mAIal2HpMrAqojLg0wcoAMx4jo6PRIACsxgGFhutkmLVc7jE8x8SwC1CCa1Cm2o22iewOZEwSQ+CJqlzsDn4YAArjwAEZhKjUxAGdogTTE7p9MFmCzi3ATBx0mYMzzeFrNZZs1ZA9YK7mRIG7faHQXsYXfFiXWaM7ySmXyrlK0nNIldJAATgppB1TBNTJphp69LmZsQrSclsw1pCG3CPMdfJdxyymPOvEuWig9yhL3enw9YF+/0BIAAomAoMinuYIaQ4fXEcjUfh0dkzp6cYplCpI2rOiTEIT+tHBkDKwnHKrvaaFiyVsFbVyIjtCwLi+6sXq/XK7UqAOxqOcawxRmNAvUbpBbuOp8OZ9k2pyeYOiATr8kcQoDqK25MteAaTk0iAABzkk+YaILO2qrsaKZ2h+abJj6CzNEh/7ZoewEns6Z4nGOoy1OWp5KJw8AAPz8G81HMfA7AAD7sFKHYwKkkxQE2AJ4Jx4ESDxkLDpkMCaDwWgcGgEDsIwpAwOIMDYoquItE4fTquhzTTFhFhMTJCAGo4S4wame5WgeQFbCBYFFkkjDqPEcD8GwpD2NoCHKM0JGhguqEWUwPlwDZaq0ouhE7j4zQqL4AC6rjQMEzCsBwVh8CWIyIhc7AALzsMARTsOwNz8MA7BXuwLgaT+ZD8E47D+DV7CVg1TWTJ1FBtbhpD8AAtM03UUL1VksX5VUabFi0AOT4Jw2j4HYW00FAE0AO45FAq3dUUvhyMpJhIKA2xaF8LB4GgCC+L4QA="}
import { DecorationItem } from 'shiki'
// ---cut---
const item: DecorationItem = {
  start: { line: 0, character: 0 },
  end: { line: 0, character: -1 },
  properties: { class: 'highlighted-word' }
}
```

这会高亮整行：

```ts
// @decorations:[{"start":{"line":0,"character":0},"end":{"line":0,"character":-1},"properties":{"class":"highlighted-word"}}]
const x = 10
console.log(x)
```

## 在变换器（Transformer）中使用装饰

在更高级的场景中，你可以使用[代码变换（Transformer）API](./transformers.md) 来完全访问 **tokens** 和 **HAST 树**。

如果想在变换器中追加装饰，可以这样做：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"972a613554136b3fe8072a30cd90426fee58d0587de59f2e2471c74d543f4625","data":"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"}
/* eslint-disable import/no-duplicates */
import { DecorationItem } from 'shiki'

// ---cut---
import { codeToHtml, ShikiTransformer } from 'shiki'

function doSomethingWithCode(code: string): DecorationItem[] {
  return []
}
const code: string = ''

const myTransformer: ShikiTransformer = {
  name: 'my-transformer',
  preprocess(code, options) {
    // 生成装饰数据
    const decorations = doSomethingWithCode(code)

    // 确保 decorations 数组存在
    options.decorations ||= []
    // 追加装饰项
    options.decorations.push(...decorations)
  }
}

const html = await codeToHtml(code, {
  theme: 'vitesse-light',
  lang: 'ts',
  transformers: [
    myTransformer
  ]
})
```

**注意**：只能在 `preprocess` 钩子中（或之前）提供装饰。在后续钩子中对 `decorations` 数组的修改将被忽略。
