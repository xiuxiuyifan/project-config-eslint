
## 配置 vs code


![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e4f8d74c22974d10be6012d626d4d17f~tplv-k3u1fbpfcp-watermark.image?)



![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2e0cb3979c564bf880f956003dcab744~tplv-k3u1fbpfcp-watermark.image?)



![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b6f9a3eef7544978917b8c829d1f835~tplv-k3u1fbpfcp-watermark.image?)





![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f878f86abf0548e29b27b8c8634dc87f~tplv-k3u1fbpfcp-watermark.image?)



## husky

```shell
yarn add husky -D
```


```
  "scripts": {
    "prepare": "husky install"
  },
```

在执行commit之前要执行的命令
```
npx husky add .husky/pre-commit "yarn lint"
```

添加lint命令
```
  "scripts": {
    "lint": "eslint --ext .js,.jsx src"
  },
```

然后在git commit 的时候就会执行yarn lint 命令进行项目代码的检查。


## lint-staged

作用:只对要提交的代码进行lint，


在我们介绍了Husky、Commitlint之后，来看一个前端文件过滤的工具[Lint-staged](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Flint-staged)，代码的格式化肯定会涉及到文件系统，一般工具会首先读取文件，格式化操作之后，重新写入。对于较大型的项目，文件众多，首先遇到的就是性能问题，虽然如Eslint之类的也有文件过滤配置，但毕竟还是对于匹配文件的全量遍历，如全量的`.js`文件，基本达不到性能要求，有时还会误格式化其他同学的代码，因此我们引入Lint-staged，一个仅仅过滤出Git代码暂存区文件(被committed的文件)的工具。

```
yarn add lint-staged -D
```

+.lintstagedrc文件

```
{
  "src/**/*.{js}": "yarn lint"
}
```


![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/919b96f327104d5aa65dad0abbc478db~tplv-k3u1fbpfcp-watermark.image?)


![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4caf4d3741924fd19bef65c6e696e3ce~tplv-k3u1fbpfcp-watermark.image?)



## commitlint

添加 git hooks

```
npx husky add .husky/commit-msg "npm test"
```

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4350be4c1e494a668619c777be961db8~tplv-k3u1fbpfcp-watermark.image?)

```
yarn add commitlint -D

yarn add @commitlint/config-conventional -D

```

添加配置文件
+.commitlintrc
```
{
  extends: ["@commitlint/config-conventional"]
}
```


## 生成changelog

```
yarn add standard-version -D
```
添加脚本

```
  "scripts": {
    "release": "standard-version"
  },
```

添加配置文件.versionrc.js

```
module.exports = {
  "types": [
    { "type": "feat", "section": "新功能", hidden: false },
    { "type": "fix", "section": "修复", hidden: false },
    { "type": "chore", "section": "配置", hidden: false },
    { "type": "docs", "section": "文档", hidden: true },
    { "type": "style", "section": "样式", hidden: true },
    { "type": "refactor", "section": "重构", hidden: true },
    { "type": "perf", "section": "性能", hidden: true },
    { "type": "test", "section": "测试", hidden: true },
    { "type": "revert", "section": "回退", hidden: true },
    { "type": "build", "section": "构建", hidden: true },
    { "type": "ci", "section": "部署", hidden: true },
  ]
}
```
不同的类型怎么升级版本的问题？

feat 更新的是第二位

fix更新的是第三位


## stylelint

```
yarn add stylelint -D

yarn add stylelint-config-standard stylelint-config-recess-order -D

```

添加配置文件
+.stylelintrc

```
{
  extends: [
    "stylelint-config-standard","stylelint-config-recess-order"
  ]
}
```


![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a5efe79c780d43b68743ec5d03a2dc56~tplv-k3u1fbpfcp-watermark.image?)

vs code添加 stylelint插件


![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb5fcca6d0484d3083d29b89996d7a06~tplv-k3u1fbpfcp-watermark.image?)



然后编写一个css文件，在git commit 的时候它就会自动的去修复文件。


stylelint 默认只能处理 css 文件，对于less scas文件它是不支持的。

```
yarn add postcss-less postcss-scss -D
```

+.stylelintrc
```
{
  "extends": [
    "stylelint-config-standard","stylelint-config-recess-order"
  ],
  "overrides": [
    {
      "files": ["**/*.less"],
      "customSyntax": "postcss-less"
    },
    {
      "files": ["**/*.scss"],
      "customSyntax": "postcss-scss"
    }
  ]
}
```


修改script 脚本

```
  "scripts": {
    "prepare": "husky install",
    "lint": "yarn eslint & yarn stylelint",
    "eslint": "eslint --ext .js",
    "stylelint": "stylelint src/**/*.{css,less,scss} --fix"
  },
```


## 仓库地址

[project-config-eslint](https://github.com/xiuxiuyifan/project-config-eslint) 点击查看