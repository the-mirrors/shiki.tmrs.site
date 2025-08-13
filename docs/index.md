---
layout: home

hero:
  name: "Shiki 式 "
  text: "代码语法高亮工具"
  tagline: 美观而强大的语法高亮工具
  image:
    src: /logo.svg
    alt: Shiki Logo
  actions:
    - theme: brand
      text: 快速开始
      link: /guide/
    - theme: alt
      text: 安装
      link: /guide/install/

features:
  - title: 精准且美观
    icon: 🌈
    details: 由 TextMate 驱动，与 VS Code 采用相同的引擎，并随之持续改进
  - title: 零运行时
    icon: ⏱️
    details: 通过提前运行，即使不加载任何 JavaScript 代码，也可以实现完美语法高亮
  - title: 可定制
    icon: 🧩
    details: 基于 HAST，可以进行高度客制化，支持插件和自定义变换
  - title: ESM & 通用
    icon: 🎄
    details: 开箱即用的 ESM 模块，支持完整的 Tree-shaking 优化，可在任何 JavaScript 运行时环境，包括浏览器、Node.js、Cloudflare Workers 等中轻松使用
---

<HomeDemo />
