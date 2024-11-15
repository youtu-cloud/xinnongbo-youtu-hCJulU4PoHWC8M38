
你好，我是 Kagol，个人公众号：`前端开源星球`。


[Fluent Editor](https://github.com) 是一个基于 Quill 2\.0 的富文本编辑器，在 Quill 基础上扩展了丰富的模块和格式，框架无关、功能强大、开箱即用。


* 源码：[https://github.com/opentiny/fluent\-editor/](https://github.com)（欢迎 Star ⭐）
* 官网：[https://opentiny.github.io/fluent\-editor/](https://github.com)


## Quill 内置公式


Quill 内置了公式的功能，基于 `KaTeX`，使用时需要安装 katex 依赖，并导入对应的样式，具体使用示例如下：


安装 katex 依赖：



```
npm i katex

```

使用 Quill 内置 formula 公式：



```
<script setup lang="ts">
import { onMounted } from 'vue'

// 导入 katex 和对应的样式
import katex from 'katex'
import 'katex/dist/katex.min.css'

// 挂载 katex 到 window 变量上
window.katex = katex

let editor

const TOOLBAR_CONFIG = [
  [{ header: [] }],
  ['bold', 'italic', 'underline', 'link'],
  [{ list: 'ordered' }, { list: 'bullet' }],
  ['clean'],
  ['formula'], // 配置公式工具栏按钮
]

onMounted(() => {
  editor = new FluentEditor('#editor', {
    theme: 'snow',
    modules: {
      toolbar: TOOLBAR_CONFIG,
    },
  })
})
script>

<template>
  <div id="editor" />
template>

```

效果图：


![](https://img2024.cnblogs.com/blog/296720/202411/296720-20241114155630395-1854074684.png)


要插入上图中的求和公式，需要知道该公式对应的 [KaTeX](https://github.com) 语法。



```
\sum_{i=1}^{n} i^2

```

然后点击工具栏的公式按钮，并将上面的公式粘贴到公式输入框中，按回车键就可以插入公式啦。


以下是效果演示（Gif 动图）：


![](https://img2024.cnblogs.com/blog/296720/202411/296720-20241114155626432-1592555434.gif)


从上面的演示动图不难看出，Quill 内置公式主要有以下问题：


* 对于用户来说`有一定的使用成本`，用户需要记住 KaTeX 公式的语法。
* 只支持新增和删除公式，`不支持编辑公式`。


## LaTeX 可编辑公式


2024年9月27日，[kiss\-keray](https://github.com) 提交了一个 PR，为 Fluent Editor 增加 mathlive 可编辑公式能力。


过了两天 kiss\-keray 关闭了这个 PR，我觉得挺可惜的，就问了一句为什么关闭了呢？


![](https://img2024.cnblogs.com/blog/296720/202411/296720-20241114155622016-82376927.png)


过了两天 kiss\-keray 又重新提交了一个 PR [\#95](https://github.com)，经过20天左右的检视和交流，今天该 PR 已合入主干分支，并发布正式版本：[v3\.22\.0](https://github.com)🎉🎉。


感谢 [kiss\-keray](https://github.com) 为 Fluent Editor 做出的贡献，让 Fluent Editor 富文本拥有这么棒的公式编辑体验！


欢迎朋友们体验和使用👏


使用可编辑公式之前，需要先安装 `mathlive` 依赖。



```
npm i @opentiny/fluent-editor@3.22.0 mathlive

```


```
<script setup lang="ts">
import { onMounted } from 'vue'

// 导入 mathlive，并引入对应的样式
import 'mathlive'
import 'mathlive/static.css'
import 'mathlive/fonts.css'

let mathliveEditor

const TOOLBAR_CONFIG = [
  [{ header: [] }],
  ['bold', 'italic', 'underline', 'link'],
  [{ list: 'ordered' }, { list: 'bullet' }],
  ['clean'],
  ['formula'], // 配置工具栏公式按钮
]

onMounted(() => {
  mathliveEditor = new FluentEditor('#mathliveEditor', {
    theme: 'snow',
    modules: {
      toolbar: {
        container: TOOLBAR_CONFIG,
        handlers: {
          formula() {
            // 绑定点击工具栏公式按钮的事件
            const mathlive = this.quill.getModule('mathlive')
            mathlive.createDialog('e=mc^2')
          },
        },
      },
      mathlive: true, // 开启可编辑公式功能
    },
  })
})
script>

<template>
  <div id="mathliveEditor" />
template>

```

可编辑公式也支持 KaTex/LaTeX 语法，可以直接复制下面的公式，粘贴到公式输入框中。



```
\sum_{i=1}^{n} i^2

```

效果如下：


![](https://img2024.cnblogs.com/blog/296720/202411/296720-20241114155641811-1200748282.png)


同时也支持对公式进行编辑，点击公式输入框右边的小键盘图标即可呼起公式编辑键盘，里面包含非常丰富的公式，使用起来非常方便，也不需要记忆复杂的 KaTeX/LaTeX 公式语法。


以下是效果演示（Gif 动图）：


![](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/3b5a28a56aa64dd98f4c851dc6609601~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgS2Fnb2w=:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMTUwNDU5OTAyNjQ0NTE1MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1732172636&x-orig-sign=OUd0f0s6JYd%2BJHzyrpCGJ3dOx8s%3D)


欢迎通过以下链接体验更多可编辑公式的功能👏


可编辑公式体验地址：[https://opentiny.github.io/fluent\-editor/docs/formula](https://github.com):[FlowerCloud机场](https://hushicha.org)


## 往期推荐文章


* [🎉Fluent Editor：一个基于 Quill 2\.0 的富文本编辑器，功能强大、开箱即用！](https://github.com)
* [👏喜报！Fluent Editor 开源富文本迎来了第一位贡献者](https://github.com)
* [🎈Fluent Editor 富文本开源2个月的总结：增加格式刷、截屏、TypeScript 类型声明等新特性](https://github.com)


## 联系我们


GitHub：[https://github.com/opentiny/tiny\-vue](https://github.com)（欢迎 Star ⭐）


官网：[https://opentiny.design/tiny\-vue](https://github.com)


B站：[https://space.bilibili.com/15284299](https://github.com)


个人博客：[https://kagol.github.io/blogs](https://github.com)


小助手微信：opentiny\-official


公众号：OpenTiny


