# @shikijs/monaco

<Badges name="@shikijs/monaco" />

在 [Monaco Editor](https://microsoft.github.io/monaco-editor/) 中使用 Shiki 来高亮。

Monaco 内置的 Highlighter 没有使用完整的 TextMate 语法，所以它可能不够准确。该集成使你可以在 Monaco 中使用 Shiki 的语法高亮引擎进行高亮显示，并共享 Shiki 的语法和主题。

本集成深受 [`monaco-editor-textmate`](https://github.com/zikaari/monaco-editor-textmate) 的启发。

## 安装

::: code-group

```sh [npm]
npm i -D @shikijs/monaco
```

```sh [yarn]
yarn add -D @shikijs/monaco
```

```sh [pnpm]
pnpm add -D @shikijs/monaco
```

```sh [bun]
bun add -D @shikijs/monaco
```

```sh [deno]
deno add npm:@shikijs/monaco
```

:::

```ts
import { shikiToMonaco } from '@shikijs/monaco'
import * as monaco from 'monaco-editor-core'
import { createHighlighter } from 'shiki'

// 创建一个可复用的语法 Highlighter
const highlighter = await createHighlighter({
  themes: [
    'vitesse-dark',
    'vitesse-light',
  ],
  langs: [
    'javascript',
    'typescript',
    'vue'
  ],
})

// 首先注册你需要的语言的 IDs
monaco.languages.register({ id: 'vue' })
monaco.languages.register({ id: 'typescript' })
monaco.languages.register({ id: 'javascript' })

// 注册 Shiki 主题，并为 Monaco 提供语法高亮 // [!code highlight:2]
shikiToMonaco(highlighter, monaco)

// 创建编辑器
const editor = monaco.editor.create(document.getElementById('container'), {
  value: 'const a = 1',
  language: 'javascript',
  theme: 'vitesse-dark',
})

// 正常使用
```

## modern-monaco

<Badges name="modern-monaco" />

我们强烈推荐使用 [modern-monaco](https://github.com/esm-dev/modern-monaco)，它内置了 Shiki 集成，并提供了更方便的 API 来构建 Monaco Editor。

### 安装

::: code-group

```sh [npm]
npm i -D modern-monaco
````

```sh [yarn]
yarn add -D modern-monaco
```

```sh [pnpm]
pnpm add -D modern-monaco
```

```sh [bun]
bun add -D modern-monaco
```

```sh [deno]
deno add npm:modern-monaco
```

\:::

或者在浏览器中直接通过 [esm.sh](https://esm.sh) CDN 引入，无需构建步骤：

```js
import * as monaco from 'https://esm.sh/modern-monaco'
```

### 使用方法

```html
<!-- index.html -->
<monaco-editor theme="vitesse-dark"></monaco-editor>
<script src="app.js" type="module"></script>
```

```js
// app.js
import { lazy, Workspace } from 'modern-monaco'

// 创建一个包含初始文件的工作区
const workspace = new Workspace({
  initialFiles: {
    'index.html': `<html><body>...</body></html>`,
    'main.js': `console.log('Hello, world!')`,
  },
  entryFile: 'index.html',
})

// 按需初始化编辑器
await lazy({ workspace })

// 写入文件并在编辑器中打开
workspace.fs.writeFile('util.js', 'export function add(a, b) { return a + b; }')
workspace.openTextDocument('util.js')
```

更多用法请参考 [modern-monaco](https://github.com/esm-dev/modern-monaco) 仓库。

```
