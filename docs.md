# 文档

本 APP 被设计为一个轻量级的 Electron APP，一方面演示一个基础 Electron APP 的构建过程，另一方面也展示了基本项目的代码组织形式。

本 APP 中 demo 展示的案例代码都是 _项目的真实代码_ 这些代码片段分别在不同的文件夹中，并以主线程和渲染线程区分开来，同时又区分到了不同的功能模块中（主要包括通信，菜单，原生 UI，多媒体，系统，窗口等）

如此做的目的当然是便于初学者快速将文档和源码对应起来。

所有的功能模块的HTML 文件以[HTML imports](http://www.html5rocks.com/en/tutorials/webcomponents/imports/)的方式注入到 `index.html` 中

如果你想添加一个新的 demo 给 Electron-demo-api 直接跳转到 [添加一个新的 demo](#add-a-section-or-demo).

## 文件结构

![APP 结构图](/assets/img/diagram.png)

#### `assets`
这个文件夹包含了 APP 所用到的静态资源：CSS，Fonts, Images，以及公共库和帮助函数。

#### `main-process`
这个文件夹包含了主进程可以调起的模块所在子文件夹模块。这个结构映射在 `renderer-process` 目录中。根目录中的 `main.js` 引用了这个文件夹中的JS 代码并执行。

#### `renderer-process`
这个文件夹包含了这个文件夹包含了主进程可以调起的模块所在子文件夹模块。这个结构映射在 `renderer-process` 目录中。根目录中的 `main.js` 引用了这个文件夹中的HTML 模板 代码并执行。
每一个视图文件会自动读取主进程和渲染进程的文件，并将这些源码添加到文档中。

#### `sections`
这个文件夹包含了案例模块的每个子文件夹，这些文件夹会引入到根目录的 `index.html` 文件中。

#### `index.html`
这是 APP 的主视图。主要包含带导航的边栏，另外使用了[HTML imports](http://www.html5rocks.com/en/tutorials/webcomponents/imports/) 来引入不同模块的实现主体。

#### `main.js`
主文件中配置了 APP 的生命周期标识，例如开始执行和结束执行的操作，这是 APP 的主入口，也是 APP 中所有文件的执行容器。`package.json`中配置了这个入口。

#### `package.json`
使用 npm 的时候，你需要有这么一个配置文件，（这么说，package.json是未来 web 的发源地，译者注）文件中包含了项目描述，作者，项目依赖，以及项目的入口文件。

#### 文档
 `CODE_OF_CONDUCT`, `README`, `docs` 以及 `CONTRIBUTING` 是这个工程的说明文件 

## UI 说明

![UI 说明](/assets/img/ui-terminology.png)

## CSS 命名规则

没啥特殊的要求，统一就好:

- 尽量不要直接配置 HTML 标签元素的样式，部分基本样式还可以。 `<p>` 或者 `<code>`
- 可以组件化的元素之间，采用父子横线命名法，以父元素为基准进行命名。
- 状态类型采用 `is-` 前缀
- 工具类型采用 `u-` 前缀

## 添加一个新 🌰 

Here are tips for covering the bases when adding a new section or demo. General tip—for some of these just copy the line or file of a similar existing item to get started!

### New Section

A whole new page with one or more demos.

#### index.html

This page contains the sidebar list of sections as well as each section template that is imported with HTML imports.

- Add demo to sidebar in the appropriate category in `index.html`
 - update `id` i.e. `id="button-dialogs"`
 - update `data-section` i.e. `data-section="dialogs"`
- Add demo template path to the import links in the `head` of `index.html`
 - i.e. `<link rel="import" href="sections/native-ui/dialogs.html">`

#### Template

This template is added to the `index.html` in the app.

- In the `sections` directory, copy an existing template `html` file from the category you're adding a section to.
- Update these tags `id`
 - i.e. `id="dialogs-section"`
- Update all the text in the `header` tag with text relevant to your new section.
 - Remove the demos and pro-tips as needed.

### Demo

Any code that you create for your demo should be added to the 'main-process' or 'renderer-process' directories depending on where it runs.


All JavaScript files within the 'main-process' directory are run when the app starts but you'll link to the file so that it is displayed within your demo (see below).

The renderer process code you add will be read and displayed within the demo and then required on the template page so that it runs in that process (see below).

- Start by copying and pasting an existing `<div class="demo">` blocks from the template page.
- Update the demo button `id`
 - i.e `<button class="demo-button" id="information-dialog">View Demo</button>`
- If demo includes a response written to the DOM, update that `id`, otherwise delete:
 - i.e. `<span class="demo-response" id="info-selection"></span>`
- Update the text describing your demo.
- If you are displaying main or renderer process sample code, include or remove that markup accordingly.
 - Sample code is read and added to the DOM by adding the path to the code in the `data-path`
   - i.e. `<pre><code data-path="renderer-process/native-ui/dialogs/information.js"></pre></code>`
 - Require your render process code in the script tag at the bottom of the template
   - i.e  `require('./renderer-process/native-ui/dialogs/information')`

#### Try it out

At this point you should be able to run the app, `npm start`, and see you section and/or demo. :tada:

