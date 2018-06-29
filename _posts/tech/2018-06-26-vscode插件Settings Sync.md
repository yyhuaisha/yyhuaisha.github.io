---
layout: post
title: vscode 插件 settings sync
category: tech
tags: 技术
keywords: Settings Sync
---

### Settings Sync

1、Settings Sync 是 vscode 中同步设置和安装插件的小工具，在扩展商店中搜索 Settings Sync 并安装.

2、登陆 Github>Your profile> settings>Developer settings>personal access tokens>generate new token，输入名称，勾选 Gist，提交.（[https://github.com/settings/tokens](https://github.com/settings/tokens)）

3、保存 Github Access Token.

4、打开 vscode，Ctrl+Shift+P 打开命令框，输入 sync，找到 update/upload settings，输入 Token，上传成功后会返回 Gist ID，保存此 Gist ID.([https://gist.github.com/youruserName](https://gist.github.com/youruserName))

5、若需在其他机器上 DownLoad 插件的话，同样，Ctrl+Shift+P 打开命令框，输入 sync，找到 Download settings，会跳转到 Github 的 Token 编辑界面，点 Edit，regenerate token，保存新生成的 token，在 vscode 命令框中输入此 Token，回车，再输入之前的 Gist ID，即可同步插件和设置.
