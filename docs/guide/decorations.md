# 代码装饰（Decorations）

Shiki 提供了 **装饰（Decoration） API**，允许你为代码中的特定范围包裹自定义的类名和属性。

```ts twoslash
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
