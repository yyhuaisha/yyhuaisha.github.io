---
layout: post
title: 从antd-pro 迁移到 electron（2.打包篇）
category: tech
tags: 技术
keywords: antd-pro electron
---

### antd-pro 迁移到 electron —— 打包篇

上一篇讲到将 antd pro 的项目迁移到 electron 怎么开发，这几天又实践了打包的部分，继续补充完整。

#### 一、安装 electron-builder 插件

```python
# 安装依赖包并运行
npm install --save-dev electron-builder
```

也有用 electron-packager 进行打包的，两者配置方面有所不同，electron-builder 可以实现自动更新。

一般情况下，通过 packager 或者 builder 打包完毕后，exe、dll、asr 等文件总和的大小为 100M 左右。而通过 builder 制作的 nsis 安装包，一般为 32M 左右。
通过 innosetup 生成的安装包，一般为 31M 左右。总体来说，体积较大。但是您通过一系列的手段可以有效的减少它的体积，到一个可接受的范围。

#### 二、main.js 的修改

我的 antd pro 项目 默认 build 到 dist 文件夹下，所以修改 mainWindow.loadURL 配置如下

```python
const startUrl = url.format({
  pathname: path.join(__dirname, './dist/index.html'),
  protocol: 'file:',
  slashes: true,
});
mainWindow.loadURL(startUrl);
```

#### 三、electron-builder 的 package.json 配置

build 与 scripts、devDependencies 同级，scripts 根据自己实际需求添加，build 中的部分参数

- appId: 应用 id
- productName: 产品名称，这个字段是控制你的应用打开程序的名称
- asar: 是否需要将项目编写的内容压缩成一个 asar 格式的文件。理论上这个是很有必要的，如果条件允许我们可以将所有的内容压缩成一个 asar 文件的话，我们的应用就大大的减少了文件数量，这在复制，或者安装的时候，速度会快上很多。但并不是所有的内容都是可以这么做的，下边会提到。
- files: 需要的资源
- compression: 压缩的形式，electron-builder 提供了 store，normal 以及 maximun 三种形式给我们选择：其中，store 打包时间最快，但仅适用于测试；而 maximum 虽然打包速度慢，但是打包出来的体积明显要的要小。所以，我们可以根据需求去设置不同的打包形式。
- icon: 这个字段用来设置应用的桌面 icon 以及 browerWindow 实例的 icon（如果你没有将你的 browerWindow 设置为 frame: false 的话）
- electronVersion: 用来打包的 electron 版本号，一般为了安全起见，减少因为兼容性引发的 bug，我觉得最好还是设定一个项目内统一的 electron 版本比较好。
- directoried: 打包的源目录
- nsis: NSIS 的设置
- oneClick: 是否为一键安装
- allowToChangeInstallationDirectory: 是否允许用户修改安装目录，该值只有在 oneClick 为 false 的时候生效
- language: 安装语言

更多请参考：

[https://www.electron.build/configuration/configuration#configuration](https://www.electron.build/configuration/configuration#configuration)

```python
"scripts": {
  "electron-start": "electron .",
  "start": "cross-env ESLINT=none roadhog dev",
  "build": "cross-env API_ENV=production ESLINT=none roadhog build",
  "package-all": "electron-builder -mwl",
  "package-build-mac": "npm run build && electron-builder --mac",
  "package-mac": "electron-builder --mac"
},
"build": {
  "asar": false,
  "productName": "myName",
  "appId": "com.xxx.xxx",
  "extends": null,
  "files": ["dist", "node_modules/", "main.js", "package.json"],
  "mac": {
    "target": ["dmg", "zip"]
  },
  "dmg": {
    "contents": [
      {
        "x": 130,
        "y": 220
      },
      {
        "x": 410,
        "y": 220,
        "type": "link",
        "path": "/Applications"
      }
    ]
  },
  "win": {
    "target": ["nsis", "zip"]
  },
  "directories": {
    "app": "./",
    "buildResources": "./build",
    "output": "./packages"
  },
  "compression": "normal",
  "nsis": {
    "oneClick": false,
    "allowToChangeInstallationDirectory": true
  }
},
```

#### 四、其他注意事项

1.如果你的 react 项目 index 页面死活出不来，可能是你的程序入口处使用了 BroswerHistory，可以考虑换成 HashHistory。

2.假如你配置的

```python
"homepage": "./",
```

不生效, 导致 build 后读取不到静态资源文件 css,js 等，可以考虑修改 `.webpackrc` 下面的

```python
publicPath: process.env.API_ENV === 'production' ? './' : '/',
```

3.我们通常写的 JS 代码里面直接 require electron 是有问题的，可以通过如下方法解决：
(参考：[https://github.com/electron/electron/issues/7300](https://github.com/electron/electron/issues/7300))

```python
const electron = window.require("electron");
const ipcRenderer = electron.ipcRenderer;

const fs = electron.remote.require("fs");
// or
const fs = window.require('fs');
```
