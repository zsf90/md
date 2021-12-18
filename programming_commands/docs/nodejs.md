Nodejs 命令笔记

##  查看 npm && node 版本

```shell
$ npm -v
6.14.9
$ node -v
v14.15.2
```

## 安装 yarn 包管理

```shell
$ npm i -g yarn
```

## npm 安装指定包

```shell
$ npm i experess
```

安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 require('express') 的方式就好，无需指定第三方包路径。

**全局安装：**

```shell
$ npm i -g experess
```

## 查看安装信息

```shell
$ npm list -g
```

### 查看某个模块的信息

```shell
$ npm list yarn	// 本地
$ npm list yarn -g // 全局
```

## 卸载模块

```shell
$ npm uninstall express
```

## 更新模块

```shell
$ npm update express
$ npm update express -g
```

## 搜索模块

```shell
$ npm search express
```

## 清空本地缓存

```shell
$ npm cache clear
```
