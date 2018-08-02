---
layout: post
title: 从antd-pro 迁移到 electron（1.开发篇）
category: tech
tags: 技术
keywords: antd-pro electron
---

### antd-pro 迁移到 electron —— 开发篇

最近准备将 antd pro 的项目迁移到 electron，找了两天资料终于有点头绪。

#### 一、首先你要有一个完整的 antd pro 的项目，然后进入项目目录安装依赖

```python
# 安装依赖包并运行
npm install -save electron
```

#### 二、配置 main.js

项目根目录下面新建 main.js 文件

```python
// 引入electron并创建一个Browserwindow
const { app, BrowserWindow } = require('electron');
const path = require('path');
const url = require('url');

// 保持window对象的全局引用,避免JavaScript对象被垃圾回收时,窗口被自动关闭.
let mainWindow;

function createWindow() {
  // 创建浏览器窗口,宽高自定义具体大小你开心就好
  mainWindow = new BrowserWindow({ width: 800, height: 600 });

  // 加载应用-----  electron-quick-start中默认的加载入口
  // mainWindow.loadURL(
  //   url.format({
  //     pathname: path.join(__dirname, 'index.html'),
  //     protocol: 'file:',
  //     slashes: true,
  //   })
  // );

  // 加载应用----适用于 react 项目
  mainWindow.loadURL('http://localhost:8000/');

  // 打开开发者工具，默认不打开
  // mainWindow.webContents.openDevTools();

  // 关闭window时触发下列事件.
  mainWindow.on('closed', () => {
    mainWindow = null;
  });
}

// 当 Electron 完成初始化并准备创建浏览器窗口时调用此方法
app.on('ready', createWindow);

// 所有窗口关闭时退出应用.
app.on('window-all-closed', () => {
  // macOS中除非用户按下 `Cmd + Q` 显式退出,否则应用与菜单栏始终处于活动状态.
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  // macOS中点击Dock图标时没有已打开的其余应用窗口时,则通常在应用中重建一个窗口
  if (mainWindow === null) {
    createWindow();
  }
});

// 你可以在这个脚本中续写或者使用require引入独立的js文件.
```

要注意“加载应用”这里你的端口号要一致。

#### 三、配置你的 package.json ，添加以下项

```python
{
  "main": "main.js", // 配置启动文件
  "dependencies": {
    "electron": "^2.0.5"
  },
  "scripts": {
    "electron-start": "electron ." // 配置electron的start，区别于web端的start
  }
}
```

注意根据你自己原本的项目的基础上来配置。

#### 四、还有重要的一点需要修改 webpack

在 antd pro 我修改的是 .babelrc.js，添加 target: 'electron-renderer'。

以下是我的 .babelrc.js 配置

```python
module.exports = {
  target: 'electron-renderer',
  plugins: [
    [
      'babel-plugin-module-resolver',
      {
        alias: {
          components: './src/components',
        },
      },
    ],
  ],
};
```

注意根据你自己原本的项目的基础上来配置。

#### 五、启动

```python
# 启动react项目
npm start

# 启动electron
npm run electron-start
```
