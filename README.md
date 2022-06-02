# deploy-cli-service2

前端一键自动化部署脚手架服务，支持开发、测试、生产多环境配置。配置好后一键即可自动完成部署。

### github

[https://github.com/liuzhenmei/deploy-cli-service](https://github.com/liuzhenmei/deploy-cli-service)

### npm

[https://www.npmjs.com/package/deploy-cli-service2](https://www.npmjs.com/package/deploy-cli-service2)


## 1 安装

全局安装 deploy-cli-service2

```shell
npm install deploy-cli-service2 -g
```

本地安装 deploy-cli-service2

```shell
npm install deploy-cli-service2 --save-dev
```

查看版本，表示安装成功

```javascript
deploy - cli - service2 - v
```

注：本地安装的需要在调用前加 `npx`

```shell
npx deploy-cli-service2 -v
```


### 2 使用（以下代码都以全局安装为例）

#### 2.1 查看帮助

```shell
deploy-cli-service2 -h
```

#### 2.2 初始化配置文件（在项目目录下）

```shell
deploy-cli-service2 init # 或者使用简写 deploy-cli-service2 i
```

根据提示填写内容，会在项目根目录下生成 `deploy.config.js` 文件，初始化配置只会生成 `dev` (开发环境)、`test` (测试环境)、`prod` (生产环境) 三个配置，再有其他配置可参考模板自行配置。


#### 2.3 手动创建或修改配置文件

在项目根目录下手动创建 `deploy.config.js` 文件，复制以下代码按情况修改即可。

```javascript
module.exports = {
  projectName: 'vue_samples', // 项目名称
  privateKey: '',
  passphrase: '',
  cluster: [], // 集群部署配置，要同时部署多台配置此属性如: ['dev', 'test', 'prod']
  dev: {
    // 环境对象
    name: '开发环境', // 环境名称
    script: 'npm run build', // 打包命令
    host: '192.168.0.1', // 服务器地址
    port: 22, // 服务器端口号
    username: 'root', // 服务器登录用户名
    password: '123456', // 服务器登录密码
    distPath: 'dist', // 本地打包生成目录
    webDir: '/usr/local/nginx/html', // 服务器部署路径（不可为空或'/'）
    bakDir: '/usr/local/nginx/backup', // 备份路径 (打包前备份之前部署目录 最终备份路径为 /usr/local/nginx/backup/html.zip)
    isRemoveRemoteFile: true, // 是否删除远程文件（默认true）
    isRemoveLocalFile: true // 是否删除本地文件（默认true）
  },
  test: {
    // 环境对象
    name: '测试环境', // 环境名称
    script: 'npm run build:test', // 打包命令
    host: '192.168.0.1', // 服务器地址
    port: 22, // 服务器端口号
    username: 'root', // 服务器登录用户名
    password: '123456', // 服务器登录密码
    distPath: 'dist', // 本地打包生成目录
    webDir: '/usr/local/nginx/html', // 服务器部署路径（不可为空或'/'）
    bakDir: '/usr/local/nginx/backup', // 备份路径 (打包前备份之前部署目录 最终备份路径为 /usr/local/nginx/backup/html.zip)
    isRemoveRemoteFile: true, // 是否删除远程文件（默认true）
    isRemoveLocalFile: true // 是否删除本地文件（默认true）
  },
  prod: {
    // 环境对象
    name: '生产环境', // 环境名称
    script: 'npm run build:prod', // 打包命令
    host: '192.168.0.1', // 服务器地址
    port: 22, // 服务器端口号
    username: 'root', // 服务器登录用户名
    password: '123456', // 服务器登录密码
    distPath: 'dist', // 本地打包生成目录
    webDir: '/usr/local/nginx/html', // 服务器部署路径（不可为空或'/'）
    bakDir: '/usr/local/nginx/backup', // 备份路径 (打包前备份之前部署目录 最终备份路径为 /usr/local/nginx/backup/html.zip)
    isRemoveRemoteFile: true, // 是否删除远程文件（默认true）
    isRemoveLocalFile: true // 是否删除本地文件（默认true）
  }
}
```

#### 2.4 部署 （在项目目录下）

注意：命令后面需要加 `--mode` 环境对象 （如：`--mode dev`）

```shell
deploy-cli-service2 deploy --mode dev # 或者使用 deploy-cli-service2 d --mode dev
```

输入 `Y` 确认后即可开始自动部署，看见如下提示说明部署完成

#### 2.5 集群部署 （在项目目录下）

注意：集群配置需要在 `deploy-cli-service` 中 配置 `cluster` 字段 （如：`cluster: ['dev', 'test', 'prod']`）

```shell
deploy-cli-service2 deploy # 或者使用 deploy-cli-service2 d
```

输入 `Y` 确认后即可开始自动部署，看见如下提示说明部署完成

#### 2.6 更新优化

如果不想把服务器密码保存在配置文件中，也可以在配置文件中删除 `password` 字段。在部署的时候会弹出输入密码界面。

如果不想在部署前执行打包命令，在配置文件中删除 `script` 字段即可。

如果需要部署前备份，在配置文件中配置 `bakDir` 字段，为空不会备份。ps: 服务器需要安装 zip 模块，。

#### 2.7 本地安装扩展

如果使用本地安装命令的话，可以在项目根目录下的 `package.json` 文件中 `scripts` 脚本中添加如下代码

```json
"scripts": {
  "serve": "vue-cli-service serve",
  "build": "vue-cli-service build",
  "lint": "vue-cli-service lint",
  "deploy": "deploy-cli-service2 deploy",
  "deploy:dev": "deploy-cli-service2 deploy --mode dev",
  "deploy:test": "deploy-cli-service2 deploy --mode test",
  "deploy:prod": "deploy-cli-service2 deploy --mode prod"
}
```

然后使用下面代码也可以完成部署操作

```shell
npm run deploy:dev
```

