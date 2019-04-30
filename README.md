如何在 vscode 中用 JavaScript Standard Style 风格去验证 vue 文件
实际上 JavaScript Standard Style 有一个 FAQ, 说明了如何使用。

但是有一点非常重要的作者没有提到，就是 eslint-plugin-html 这个插件必须要安装 3.x.x 版本的, 现在 eslint-plugin-html, 已经升级到 4.x 版本，默认不写版本号安装的就是 4.x 版本的，所以会出现问题。

参考：详情请参考 https://www.npmjs.com/package/eslint-plugin-html
此 ESLint 插件允许 linting 和修复 HTML 文件中包含的内联脚本。

迁移到 v4
eslint-plugin-htmlv4 至少需要 ESLint v4.7。这是因为 ESLint v4.7 中发生了许多内部更改，包括支持预处理器中自动固定的新 API。如果您仍在使用旧版本的 ESLint，请考虑升级或继续使用 eslint-plugin-htmlv3。

eslint-plugin-htmlv4 中的重要特性（和重大变化）是能够选择在同一 HTML 文件中的脚本标记之间共享范围的方式。

迁移到 v3
如果您正在考虑升级到 v3，请阅读本指南。
ESLint v4 is only supported by eslint-plugin-html v3, so you can't use eslint-plugin-html v1.5.2 with it (I should add a warning about this when trying to use the plugin with an incompatible version on ESLint).
If you do not use ESLint v4, please provide more information (package.json, a gist to reproduce, ...)
// FAQ
How to lint script tag in vue or html files?

You can lint them with eslint-plugin-html, just install it first, then enable linting for those file types in settings.json with:

```json
{
  "standard.validate": ["javascript", "javascriptreact", "html"],
  "standard.options": {
    "plugins": ["html"]
  },
  "files.associations": {
    "*.vue": "html"
  }
}
```

If you want to enable autoFix for the new languages, you should enable it yourself:

```json
{
  "standard.validate": [
    "javascript",
    "javascriptreact",
    { "language": "html", "autoFix": true }
  ],
  "standard.options": {
    "plugins": ["html"]
  }
}
```

# 1、需要安装插件：

npm i -g standard
npm i -g eslint-plugin-html@3.2.2 此处使用是 3x 版本
npm i -g eslint 或者 vscode 安装 eslint

# 2 、vscode setting 设置：

```json
{
  "standard.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "html",
      "autoFix": true
    }
  ],
  "standard.options": {
    "plugin": ["html"]
  },
  "files.associations": {
    "*.vue": "html"
  },
  "standard.autoFixOnSave": true
}
```

# 3、vscode 相关插件 Prettier and eslint 格式化代码：

ESLint (如果全局安装了,vscode 可以不安装)
Prettier formatter
Vetur

# 4 格式化代码相关设置

```json
{
  "files.autoSave": "afterDelay",
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue",
    {
      "language": "html",
      "autoFix": true
    }
  ],
  "prettier.singleQuote": true,
  "prettier.semi": false,
  "editor.formatOnSave": true
}
```

# 5 .eslintrc.js 相关

项目根目录下创建 .eslintrc.js

````js
module.exports = {
  root: true,
  env: {
    node: true,
  },
  'extends': [
    // 需要官方的 eslint-plugin-vue，它支持同时检查你 .vue 文件中的模板和脚本。请确保在你的 ESLint 配置中使用了该插件自身的配置：
    'plugin:vue/essential',
      // standard 代码规范  https://github.com/standard/standard/blob/master/docs/RULES-en.md
    '@vue/standard'
  ],
  rules: {
    'no-new-func':0,
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    // 解决 iview 报错问题
    "vue/no-parsing-error": [2, { "x-invalid-end-tag": false }]
  },
  parserOptions: {
    parser: 'babel-eslint'
  }
  ```

# vscode-eslint
````
