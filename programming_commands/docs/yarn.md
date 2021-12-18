## 安装

```shell
$ npm i -g yarn
```

## 查看版本

```shell
$ yarn -v
```

## 初始化一个工程

```shell
$ yarn init
```

## 添加一个依赖

```shell
$ yarn add webpack # yarn --save 是 yarn 默认行为
```

### 生产环境

```shell
$ yarn add webpack --dev # 简写 yarn -D，类似于 npm --save-dev
```

### 全局添加

```shell
$ yarn global add webpack
```

## 更新一个依赖

```shell
$ yarn upgrade
$ yarn upgrade webpack
$ yarn upgrade --latest # 忽略版本规则，升级到最新版本，并且更新 package.json
```

## 移除一个依赖

```shell
$ yarn remove webpack
```

## 安装 package.json 中的所有包

```shell
$ yarn 或 yarn install
$ yarn install --force # 强制下载安装
```

## 运行脚本

```shell
$ yarn run
```

```json
// package.json
{
    "scripts": {
        "dev": "node app.js",
        "start": "node app.js"
    }
}
```

```shell
$ yarn run dev 运行 dev 对应的脚本
```

## 显示某个包的信息

```shell
$ yarn info webpack
$ yarn info webpack --json # 输出 json 格式
$ yarn info webpack readme # 输出 README 部分
```

## 列出项目的所有依赖

```shell
$ yarn list
$ yarn list --depth=0 # 限制依赖的深度
$ yarn global list # 列出全局安装的模块
```

## yarn 配置文件

```shell
$ yarn config
$ yarn config set key value
$ yarn config get key
$ yarn config delete key
$ yarn config list # 显示当前配置
$ yarn config set registry https://registry.npm.taobao.org # 设置淘宝镜像
$ npm config set registry https://registry.npm.taobao.org # npm
```

## 缓存

```shell
$ yarn cache
$ sudo yarn cache list # 列出已缓存的每个包
$ sudo yarn cache dir # 返回全局缓存位置
$ sudo yarn cache clean # 清除缓存
```

