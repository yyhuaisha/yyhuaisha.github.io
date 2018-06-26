---
layout: post
title: vscode config
category: tech
tags: 技术
keywords: vscode
---

### 记录我的 vscode 配置

一次 mac 重启之后文件夹全部被清空了一次，只剩下安装的 app，配置丢失，又是艰难的重新配置，简直是灾难。

记录下当前简单的 vscode 的配置信息，持续补充……

#### setting

```python
{
    "window.zoomLevel": 1,
    "editor.tabSize": 2,
    "editor.formatOnSave": true,
    "[javascript]": {
        "editor.formatOnSave": true
    },
    "prettier.eslintIntegration": true,
    "prettier.stylelintIntegration": true,
    "prettier.tabWidth": 2,
    "prettier.singleQuote": true,
    "prettier.semi": false,
    "prettier.bracketSpacing": true,
    "eslint.autoFixOnSave": true,
    "files.autoSave": "afterDelay",
    "files.associations": {
        "*.js": "javascriptreact"
    },
    "emmet.includeLanguages": {
        "javascript": "javascriptreact"
    },
    "emmet.syntaxProfiles": {
        "javascript": "jsx"
    },
    "emmet.triggerExpansionOnTab": true,
}
```

#### keybindings

```python
[
    {
        "key": "cmd+y",
        "command": "redo",
        "when": "textInputFocus && !editorReadonly"
    },
    {
        "key": "shift+cmd+z",
        "command": "-redo",
        "when": "textInputFocus && !editorReadonly"
    },
    {
        "key": "shift+cmd+f",
        "command": "editor.action.formatDocument",
    },
]
```
