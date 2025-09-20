---
outline: deep
---

# VitePress 集成

[VitePress](https://vitepress.dev/) 在底层使用了 Shiki，所以你不需要显式地集成。

VitePress 提供了 [一些 Shiki 的自定义选项](https://github.com/vuejs/vitepress/blob/main/src/node/markdown/markdown.ts#L66-L112)，在 [VitePress 文档](https://vitepress.dev/zh/reference/site-config#markdown) 上查看更多。

## Twoslash

要在 Vitepress 中启用 [TypeScript Twoslash](/packages/twoslash)（类型悬停显示），可以使用我们提供的插件来快速开始，它借助 [Floating Vue](https://floating-vue.starpad.dev/) 在容器外显示具有样式的类型信息。

<Badges name="@shikijs/vitepress-twoslash" />

### 安装

::: code-group

```sh [npm]
npm i -D @shikijs/vitepress-twoslash
```

```sh [yarn]
yarn add -D @shikijs/vitepress-twoslash
```

```sh [pnpm]
pnpm add -D @shikijs/vitepress-twoslash
```

```sh [bun]
bun add -D @shikijs/vitepress-twoslash
```

```sh [deno]
deno add npm:@shikijs/vitepress-twoslash
```

:::

在 [`.vitepress/config.ts`](https://vitepress.dev/reference/site-config) 配置文件中：

```ts [.vitepress/config.ts]
import { transformerTwoslash } from '@shikijs/vitepress-twoslash' // [!code hl]
import { defineConfig } from 'vitepress'

export default defineConfig({
  markdown: {
    codeTransformers: [
      transformerTwoslash() // [!code hl]
    ],
    // [!code hl:2]
    // Explicitly load these languages for types hightlighting
    languages: ['js', 'jsx', 'ts', 'tsx']
  }
})
```

然后在你的 [`.vitepress/theme/index.ts`](https://vitepress.dev/guide/custom-theme) 中，安装 Vue 插件并通过 `vitepress-plugin-twoslash/styles.css` 导入 CSS。

```ts [.vitepress/theme/index.ts]
// @noErrors: true
import type { EnhanceAppContext } from 'vitepress'
import TwoslashFloatingVue from '@shikijs/vitepress-twoslash/client' // [!code hl]
import Theme from 'vitepress/theme'

import '@shikijs/vitepress-twoslash/style.css' // [!code hl]

export default {
  extends: Theme,
  enhanceApp({ app }: EnhanceAppContext) {
    app.use(TwoslashFloatingVue) // [!code hl]
  },
}
```

::: details 关于 style.css

方便起见，`vitepress-plugin-twoslash/styles.css` 包含了 `floating-vue` 和 `shiki-twoslash/style-rich.css` 中的样式，所以你只需要引入这一项。如果你使用的是自定义 `floating-vue` 样式，或者需要对样式进行更多控制，你可以将它展开成如下几项：

```ts
import '@shikijs/vitepress-twoslash/style.css'

// 等同于：
import '@shikijs/twoslash/style-rich.css'
import 'floating-vue/dist/style.css'
import '@shikijs/vitepress-twoslash/style-core.css'
```

:::

现在，你可以在你的 Markdown 文件中使用 `ts twoslash` 来启用美观的类型悬停显示。

````md
```ts twoslash
console.log('hello')
//      ^?
```
````

它会被渲染为：

```ts twoslash
// @twoslash-cache: {"v":1,"hash":"258fd8e6b019224fc5515598774b6ccef0d85eefda3a3ba0992cbaa911ba9952","data":"N4Igdg9gJgpgziAXAbVAFwJ4AcZJACwgDcYAnEAGhDRgA808wBDAW3iyYGMYACTiMHAgAbGAB0wRJqT4ChoxDwDCckbipQInBIhAAVfLwAG/QWqM8W0AK6ieWUsQCWsODyY84TlljuwARtYA5kFOYEGyZnZo+ExoPE5uXixOwtI8aBAZhhIAUkxSAMqcpE5Y8abyvGycsWCJLPaORC4wUDz+GDwA7jD+HY7dcGRwAHQSEgbVNnZ0WBCkaG5o3VlwOJxOAGZOnLI+AjBgS4gTYABUPACCPEYqUTAWnGlwbt1OMZYwMdBJ1rXuNwmVSiUbCCBBAAUAEojBRbpU1KMyI5SDCLEwwO1gQ9Rt1pGB0dk4nxMR1eNZhu1Mj1SjQMllMV0AHLQGCjABWSTQpBgrHGF2uPCC4P8TGECJBjwSgjQmO4kR2QWsvOpWW6dN4NOQRgcWngYzgaE01jQRgAupD8Gg0Fg4IgAPQOyCwLmjBZBB2abQOtI0I0AWiIACYACyjWgOphYJwOvXcV6ja0sYQAYnjBqNJrQ0PcWIkOoziazKItVptdsdzrZbo9Xq0cF9cXgaCDYYjUZjcccCbGybTRdexpR0NGPCmwtF4slDyeZP8FKpPQ+hFNCQOizCERixhdMEQiNEFisUFs7LOAH1zucAOoErfXi+KCciiBiiWH3hvjkwThoADkbhsD8UBuNIvBgDAK5kJEXhGkcaDCBgEhwBgYC1I4kCUjwwhOAA1lqhgDBAQwwVcAAKACSyyGF0vLDCw/iiPCkAyOB2QwF0lSJDQxxIRITCoeh+CYRA2G4QR7jCBKEA7jIrKwJy3K8qwYw8IUMCEbwOqQPSAhNPqrw8JRDoAPJltatr2k6e61qQnreo2fotm24aRtGsaDkmaApqmTABrpMABgIAaDgGTgQLmWwLBIVi8jK0WkCwcQRWAAoSAAorQrC+IuW4cZOb7TjiVRGKcYASEYVVchIn5ghCkL/oY0nqgswhQP+0IANwSE6PDkaUxz2jwzXgj0bVQPCNJZmJaC1VK9VQk1MAtTwACkcD/vC/6rKQ7WdT1YB9QNYQnCNK1jbt7VTWsxqzfNuIogskKQd0PAZaQqKNTehAQHa8JCMB+D5WK7SxFgOCQR10Ldb1Dr9YNSw8E9MhsK8TBBLwmLtEaXB4RkpBcFqt2wJ95V9Tw72fQsig/RAf1wADEBAyDTBg9GkNtHDlM8ySyAwFIwjmogACsiAAIwi9zPPuPExSlOUoykNYYCUWABiJPcND0DwL1sogRAsBLADMwYSwAHNC0u8/Epn+D+f5Kyrasa3AWt0PEeuwAbRvGwADAAnIgxuW9blMknuiCnWQzDCN2hmNnQv6mqliAAOxpxLAdh7LPD84L5oBhqHNkIgABsiDBsGOckgL4ry2UnuR9HpCx/HvYOknnApwI6cV2XftW0d8MyxH+st23yVhJ3gsXkag1BJXxvB2clTxMwbA8AAvDw/43qkEoAEpvmEQhgP+h11fireQkYAAimKYzIAAkwAbzAAC+ACEPAP+EZBfyMLDYeCNTrDT/k/Hg+9pI8GPv4U+Agf4QIATdTww5PqVSqmcLKOU7CUnyjuW49xSp8BePaM4VUjA1XQnIeIs1t7Cm+IUHkfIWCFGZjAQg3QYSX1ocjT6DDMZoGYSpNhHCuE8IekaSwGBiFqAYa9WCSI5GiEhLNeEI5DqxVkQtcES1RqtT2tDQ6x1EbDQMeNIxqD7pgBYDo3EejGoWI2ltXeV1jHcxOkNRQFj3HWNNNolR7IUYvRgG9D6X096/X+p4DhMRWbswhkcNonVgGmLAYoZAESaZQOiYzWJLNwgdDZiNEuUNzSoJRKvPh78GF7wPrAk+Zhz6HTsUEvEBJb7IJfm/Vgn8kGPwAUAkx8MvFnW6VAhpcCEFgAGf/UgX9KkYLAJQyg1AMY6GQMgEAwx1AgGQEIFU3BLSWUrE6UIMRrD+FGPwFg1ZXSNj3A6Jib4HQhjcr6Jw/gHR1S5NCEA5oKk7LlIsJAfsqCiHCDEJAacqAgqEXgT8azcKQTBVQWo0guA0HIIgP2H8KDoGwLgXQhASDkDhR7PAkIgbQFzO0xxaM4AYxgAAfkUEyeEowuV/TQKlcU5FMUsGGkyZA5poSKCIBAFwusADU4seDEDIOCNm/yNANjwGM5YWQjAzVNBYd4nxXoovZDwAAsrYXluV3D2WsGwIapIwDknsIJKk8IDX4A4hIHYpBpGUjaICAqDhvDSC6Iy5leZ2jiglGzKAHw+USj9ZGv4/gjQfB7hVSQ4prDwE8N4VI6RtS6kRlsSExtYQnIrFWZKYA07unsp8sA1hIzVtCsyxs1bjbdlOlsUYxtvIpiHpCQh0hlR2qRuxKNzrXj+sLSnYQoxErJTQOiCtVkqy2TGHWRyTZ/StneR2DyDo539rTHOxdcRz2thHXAGG6UVnVTgFIioYljgMJFrwhxDV/z8BVmgRQa0Orwh/ccNJoyzGKGA3+ngItUG6rmjQz9S1IOIFcZB0DoDvGyF/YoGDDI0HZkwUYM4GltJGGPZeld5Y102RrJu+t27nKBn3e5Lsx7+ypjPQsJdl6AzXqigsSwCxeBhEvalUYay5RBE2dsrw6E9lED9qMcWSm/Z+wBUC3GoLEDmwhUcII0Lg5wpHd8PAejkVhGJeCkAGLCZ/jIEgc2+LCU4DwAAR2zaQDAEnKW6Gpd8QgUA6W6IamGzGbK8wYE5dy8o8aBWEyFeysAGBRXip4JK6VkI5UKrJcqwLazHIarMXhnVd09XLkNWE41Y5zXCEtXYEdtqEJuE4POXgHBp2TXKx6ncXqnA+viImgNhCg3JU818dGmMI1SWjVAWNvKBDTkG4JTwVzU1oHTRIQW2akh5rSDIQtQbjglrLRZStTpq21rrCiptDoW0cExu2zEnbDtoB7X2/sg7h02rHWBeKk72tLlnby+dFHy1UbOfcmAdkHINh3S5ZjnZYxsZ8qe4Hl6eN8dHBQh9T6sOvp3u+qRSJHHfpfVBgDqGyfoc1RBsnOHYOlfg3VEnyHKe/up+BvHUHcPTUZ4R4jmk85keBwurjcRKOnOspD6H9YfSMb3e2FjSORfsc40lC9Yur32RvTwRKgn4oic12JiTGykBbJ2WEbgayFNKZU2pwFVBNMMG07pqF+AkDGyM/ZEzugzMQos2i6zsRbPYocx/IF/BYCIuC/oi6EBOoSDWcBJgSBQAeyOF4AQeAlggA/h/IAA="}
console.log('hello')
//      ^?
```

<br> <!-- leaving some space for the query above -->

### Vue SFC

此外，这个插件集成了 [`twoslash-vue`](https://github.com/antfu/twoslash-vue)，所以你可以使用 `vue twoslash` 高亮 Vue SFC 块：

```vue twoslash
// @twoslash-cache: {"v":1,"hash":"afc2b20dc07a9a858f9e67b3d93aeba5149a4edd4d4198ae23d46d17e9a7679e","data":"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"}
<script setup>
import { onMounted, ref } from 'vue'

// 响应式状态
const count = ref(0)
//             ^?

// 修改状态并出发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">
    Count is: {{ count }}
  </button>
</template>
```

### 文件系统缓存

为了加速构建过程，可以为生成的类型启用文件系统缓存，该缓存可在多次构建之间共享。默认情况下，缓存存储在 `.vitepress/cache/twoslash` 文件夹中，与其他 VitePress 缓存共存。

在你的 [`.vitepress/config.ts`](https://vitepress.dev/reference/site-config) 文件中：

```ts [.vitepress/config.ts]
import { transformerTwoslash } from '@shikijs/vitepress-twoslash'
import { createFileSystemTypesCache } from '@shikijs/vitepress-twoslash/cache-fs' // [!code hl]
import { defineConfig } from 'vitepress'

export default defineConfig({
  markdown: {
    codeTransformers: [
      transformerTwoslash({
        typesCache: createFileSystemTypesCache() // [!code hl]
      })
    ]
  }
})
```
