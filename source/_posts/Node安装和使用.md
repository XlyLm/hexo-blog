---
title: Node安装和使用
abbrlink: 4193741959
date: 2026-07-06 22:26:15
tags: 包管理
index_img: /img/包管理/node.png
cover: /img/包管理/node.png

---
# Node安装和使用

## 一、安装

### 1、直接官网下载和安装

- 官网地址：[Node.js下载 ](https://nodejs.org/zh-cn/download)，根据实际情况选择对应版本。【不推荐该方法，实际开发过程中可能涉及多个node版本】

### 2、使用nvm管理多个Node版本

- [Nvm下载地址](https://github.com/coreybutler/nvm-windows/releases)，下载`nvm-setup.zip`这个包，它可以自动配置好环境，一路安装，第一个路径是`nvm`安装路径，第二个路径是`node`的安装路径。

- [Nvm使用教程](https://github.com/coreybutler/nvm-windows)

- nvm常用指令

```javascript
nvm ls  							// 查看目前已经安装的版本
nvm install 10.5.0 		 			// 安装指定的版本的nodejs
nvm use 10.5.0 			 			// 使用指定版本的nodejs
nvm current 						// 显示当前的版本
nvm list							// 显示已下载列表
nvm list available 					// 显示可下载版本的部分列表
nvm uninstall 10.5.0				// 删除已安装的指定版本，语法与install类似
nvm alias 							// 给不同的版本号添加别名
nvm unalias					 		// 删除已定义的别名
nvm reinstall-packages <version> 	// 在当前版本node环境下，重新全局安装指定版本号的npm包
```

**注意**：如果在切换或下载时出错，可以到 cmd 中使用管理员身份进行重新命令。

### 3、使用nrm管理npm

​	安装node后，node会自带对应版本的`npm`。可以使用`npm`源管理工具`nrm`，一键切换 `npm/yarn/pnpm` 下载镜像（淘宝、官方、腾讯、华为等），解决国内`npm`下载慢、超时问题。

- [npm官方文档](https://www.npmjs.cn/)

- 安装`nrm`

```bash
npm install -g nrm

# 若提示权限报错（mac/linux）
sudo npm install -g nrm
```

- `nrm`常用指令

```bash
# 查看nrm版本
nrm -V
nrm --version
# 查看所有可用镜像源
nrm ls
# 切换镜像源
nrm use <对应的源>
# 查看当前正在使用的源
nrm current
# 测速所有源
nrm test
# 单独测试某一个源速度
nrm test <对应的源>
# 添加自定义私有 npm 源
nrm add 源名称 源地址
# 删除自定义源
nrm del 源名称
# 打开源地址网页
nrm home <对应的源>
# 同步配置 yarn/pnpm 镜像
yarn config set registry $(nrm current)
# pnpm 同理
pnpm config set registry $(nrm current)
# 查看 npm 当前 registry
npm config get registry
# 手动切换源
npm config set registry 源地址

# 切换源后下载依旧慢 清除 npm 缓存
npm cache clean --force
# 卸载 nrm
npm uninstall -g nrm
```

## 二、node常用指令

```bash
# 运行 JS 文件
node ***.js
# 直接执行一行代码
node -e "console.log('hello node')"
# 查看 Node 版本
node -v
node --version
# 查看帮助文档
node -h
node --help
# 调试模式
node --inspect ***.js
```

## 三、npm常用指令

### 版本查看

```bash
# 监听文件自动重启
npm i -g nodemon
nodemon ***.js
# 版本查看
npm -v
npm --version
```

### 1. 初始化项目（生成 package.json）

```bash
# 一步步问答创建
npm init
# 快速默认创建，跳过问答
npm init -y
```

### 2. 安装依赖

- 安装生产依赖（项目运行需要）

```bash
npm install 包名
npm i 包名
# 示例
npm i express axios
```

- 安装开发依赖（仅开发 / 打包用，不打包上线）

```bash
npm i 包名 -D
npm install 包名 --save-dev
# 示例
npm i webpack vite eslint -D
```

- 全局安装工具（cli 命令，nrm/nodemon/vite 等）

```bash
npm i -g 包名
npm install --global 包名
# 示例
npm i -g nrm nodemon pnpm
```

- 安装项目全部依赖（克隆项目后）

```bash
npm install
# 简写 npm i
npm i
```

### 3. 卸载依赖

```bash
# 卸载本地依赖
npm uninstall 包名
npm un 包名
# 卸载开发依赖
npm un 包名 -D
# 卸载全局包
npm un -g 包名
```

### 4. 更新依赖

```bash
# 更新单个包到最新版本
npm update 包名
# 更新所有包
npm update
# 全局更新
npm update -g 包名
```

### 5. 运行 package.json scripts 脚本

package.json 配置示例：

```json
"scripts": {
  "dev": "nodemon index.js",
  "build": "vite build",
  "start": "node index.js"
}
```

执行命令：

```bash
npm run dev
npm run build
npm start # start 可省略 run
npm test  # test 可省略 run
```

### 6. 查看包信息

```bash
# 查看本地安装包版本
npm list
npm ls
# 只看顶层依赖
npm ls --depth=0
# 查看全局安装的包
npm ls -g --depth=0
# 查看远程包详情、版本
npm info 包名
npm view 包名
```

### 7. 缓存清理

```bash
# 清理 npm 缓存
npm cache clean --force
# 校验缓存是否损坏
npm cache verify
```

### 8. registry 镜像源配置（搭配你之前学的 nrm）

```bash
# 查看当前镜像源
npm config get registry
# 手动切换淘宝源
npm config set registry https://registry.npmmirror.com/
# 恢复官方源
npm config set registry https://registry.npmjs.org/
```

### 9. 安装 / 删除 package-lock.json

lock 文件锁定版本，不要手动删，如需重装依赖：

```bash
rm -rf node_modules package-lock.json
npm i
```

### 10. 发布私有 / 公共包（开发 npm 包用）

```bash
# 登录 npm 账号
npm login
# 发布包
npm publish
# 撤销发布（24小时内）
npm unpublish 包名 --force
```

## 四、npx 临时运行命令（无需全局安装）

不用全局装包，临时调用工具，用完不占空间：

```bash
# 临时创建 vite 项目
npx create-vite@latest
# 临时运行 eslint 检查文件
npx eslint ./src
# 临时启动 http 静态服务
npx serve
# 临时查看包版本
npx node -v
```

## 五、使用 node 发布自己的包

​	在 `Node.js` 语言中，包和模块并没有本质的不同，包是在模块的基础上更深一步的抽象，包将某个独立的功能封装起来，用于发布、更新、依赖管理和进行版本控制。`Node.js` 根据 `CommonJS` 规范实现了包机制，开发了 `npm` 来解决包的发布和获取需求。

### 1、注册自己的 npm 账号

- 访问 `npm` 网站，点击 sign up 按钮，进入注册用户界面

- 填写账号相关的信息：Full Name、Public Email、Username、Password
- 点击 Create an Account 按钮，注册账号
- 登录邮箱，点击验证链接，进行账号的验证

### 2、登录 npm 账号

- `npm` 账号注册完成后，可以在终端中执行 `npm login` 命令 (或者 npm adduser 命令)，依次输入用户名、密码、邮箱后，即可登录成功
- 注意：在运行 `npm login` 命令之前，必须先把下包的服务器地址切换为 `npm` 的官方服务器。否则会导致发布包失败！(终端执行 `nrm use npm`)

```bash
nrm use npm
```

### 3、把包发布到 npm 上

- 将终端切换到包的根目录之后，运行 `npm publish` 命令，即可将包发布到 `npm` 上（注意：包名不能雷同，包名只能是小写字母 + 数字 + 中划线)

- 利用终端查看包名是否重名，`npm view 包名` (或者 `npm view 包名 version`)


### 4、删除已发布的包

- 运行 `npm unpublish 包名 --force` 命令，即可从 npm 删除已发布的包

**注意事项**：

- `npm unpublish` 命令只能删除 72 小时以内发布的包
- `npm unpublish` 删除的包，在 24 小时内不允许重复发布

### 5、更新包

- 修改代码，保存。
- 更新版本号。可直接在 package.json 中修改：只能改大，不能改小。
- 重新 publish


