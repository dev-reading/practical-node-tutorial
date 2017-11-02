# practical-node

实用Node.js，简单粗暴，新手学习的最短曲线

## 安装环境

3m安装法

- nvm（node version manager）【需要使用npm安装，替代品是yrm（支持yarn）】
- nrm（node registry manager）【需要使用npm安装，替代品是yrm（支持yarn）】
- npm（node packages manager）【内置，替代品是n或nvs（对win也支持）】

### nvm

node版本发布非常快，而且多版本共存可能性较大，推荐使用nvm来安装node

```shell
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash

$ echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.zshrc
$ echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm' >> ~/.zshrc
$ source ~/.zshrc

$ nvm install 0.10
$ nvm install 4
$ nvm install 6
$ nvm install 8
```

### nrm

https://registry.npm.js是node官方的源（registry），服务器在国外，下载速度较慢，推荐安装nrm来切换源，国内的cnpm和taobao的源都非常快，当然，如果你想自建源也是支持的。

```shell
$ npm install --global nrm --registry=https://registry.npm.taobao.org
$ nrm use cnpm
```

### npm

nrm切换完源之后，你安装npm模块的速度会更快。

```shell
$ npm install --global yarn
```

npm基本命令

| 名称 | 描述 | 简写 |
| --- | --- | --- |
| npm install xxx | 安装xxx模块，但不记录到package.json里 | npm i xxx |
| npm install --save xxx | 安装xxx模块，并且记录到package.json里，字段对应的dependency，是产品环境必须依赖的模块 | npm i -s xxx |
| npm install --save-de xxx | 安装xxx模块，并且记录到package.json里，字段对应的dev-dependency，是开发环境必须依赖的模块，比如测试类的（mocha、chai、sinon、zombie、supertest等）都在 | npm i -D xxx |
| npm install --global xxx | 全局安装xxx模块，但不记录到package.json里，如果模块里package.json有bin配置，会自动链接，作为cli命令 | npm i -g xxx |

## 准备工作目录

我的工作目录一般是 `~/workspace/github`

## 常用软件

- 1）oh my zsh是我最习惯的shell，终端下非常好用

配合iterm2分屏 + spectacle全屏，几乎无敌

- 2）brew是mac装软件非常好的方式，和apt-get、rpm等都非常类似

安装4个必备软件

- brew install git 最流行的SCM源码版本控制软件
- brew install wget 下载、扒站神器
- brew install ack  搜索代码神器
- brew install autojump 终端下多目录跳转神器

- 3）vim

我虽然不算vim党，但也深爱着。janus是一个非常好用的vim集成开发环境。比如ctrl-p、nerdtree等插件都集成了，对我这种懒人足够了。

![Vim](images/vim.png)

## 编辑器推荐VSCode

Visual Studio Code（以下简称vsc）

- vsc是一个比较潮比较新的编辑器（跨平台Mac OS X、Windows和 Linux ）
- vsc功能和textmate、sublime、notepad++，ultraedit等比较，毫不逊色
- vsc尤其是在nodejs（调试）和typescript、go上支持尤其好
- vsc提供了自定义 Debugger Adapter 和 VSCode Debug Protocol 从而实现自己的调试器

值得一学

下载安装VScode

- 配置code命令
- 配置快捷键
- 安装vsconde-icons插件

配置code命令

![Vscode Code](images/vscode-code.png)

配置快捷键，最喜欢cmd + [1-5]，这和xcode习惯一直，非常棒

```
// 将键绑定放入此文件中以覆盖默认值
[
    { "key": "cmd+1",           "command": "workbench.view.explorer" },
    { "key": "cmd+2",           "command": "workbench.view.search" },
    { "key": "cmd+3",           "command": "workbench.view.scm" },
    { "key": "cmd+4",           "command": "workbench.view.debug" },
    { "key": "cmd+5",           "command": "workbench.view.extensions" }
]
```

安装vsconde-icons插件，对各种文件扩展都有icon显示，更直观

![Vscode Icons](images/vscode-icons.png)

## 学会VSCode调试

express调试实例

这是我们最常用的调试

通过创建express项目构建，调试来演示vsc的具体用法

### 创建express项目

使用express-generator

```
➜  examples git:(master) ✗ express helloworld

   create : helloworld
   create : helloworld/package.json
   create : helloworld/app.js
   create : helloworld/public
   create : helloworld/public/javascripts
   create : helloworld/public/images
   create : helloworld/public/stylesheets
   create : helloworld/public/stylesheets/style.css
   create : helloworld/routes
   create : helloworld/routes/index.js
   create : helloworld/routes/users.js
   create : helloworld/views
   create : helloworld/views/index.jade
   create : helloworld/views/layout.jade
   create : helloworld/views/error.jade
   create : helloworld/bin
   create : helloworld/bin/www

   install dependencies:
     $ cd helloworld && npm install

   run the app:
     $ DEBUG=helloworld:* npm start

➜  examples git:(master) ✗ cd helloworld 
➜  helloworld git:(master) ✗ npm install
➜  helloworld git:(master) ✗ npm start
```

测试express项目是正常的。

说明：如果是自己的项目，需要自己构建git版本控制的，faq里有具体说明。

### 修改launch.json的内容

输入command + t快速定位文件：.vscode/launch.json

修改launch.json的内容

```
{
	"version": "0.1.0",
	// List of configurations. Add new configurations or edit existing ones.
	// ONLY "node" and "mono" are supported, change "type" to switch.
	"configurations": [
		{
			// Name of configuration; appears in the launch configuration drop down menu.
			"name": "Launch helloworld",
			// Type of configuration. Possible values: "node", "mono".
			"type": "node",
			// Workspace relative or absolute path to the program.
			"program": "examples/helloworld/bin/www",
			// Automatically stop program after launch.
			"stopOnEntry": false,
			// Command line arguments passed to the program.
			"args": [],
			// Workspace relative or absolute path to the working directory of the program being debugged. Default is the current workspace.
			"cwd": ".",
			// Workspace relative or absolute path to the runtime executable to be used. Default is the runtime executable on the PATH.
			"runtimeExecutable": null,
			// Optional arguments passed to the runtime executable.
			"runtimeArgs": ["--nolazy"],
			// Environment variables passed to the program.
			"env": {
				"NODE_ENV": "development"
			},
			// Use JavaScript source maps (if they exist).
			"sourceMaps": false,
			// If JavaScript source maps are enabled, the generated code is expected in this directory.
			"outDir": null
		},
		{
			"name": "Attach",
			"type": "node",
			// TCP/IP address. Default is "localhost".
			"address": "localhost",
			// Port to attach to.
			"port": 5858,
			"sourceMaps": false
		}
	]
}
```

核心内容

```
"name": "Launch helloworld",
"type": "node",
"program": "examples/helloworld/bin/www",
```

program是要执行的express的入口。

这里的helloworld是项目，所以找到/bin/www目录即可。

### 点击调试按钮

![](images/8.png)

会弹出一个窗口，执行如下命令

```
cd '/Users/sang/workspace/github/vsc-doc'; env 'NODE_ENV=development'   'node' '--debug-brk=44412' '--nolazy' 'examples/helloworld/bin/www'
Debugger listening on port 44412
```

其实node-inspector也是这个原理的。

### 增加断点

![](images/9.png)

### 此时访问

```
curl http://127.0.0.1:3200/
```

### 进入调试界面

![](images/10.png)

和chrome的调试是一样的。

点击1）处按钮，打开控制台，配合调试，在控制台里查看对应的变量值

另外值得说明的是二级菜单里4个部分

- a）variables变量
- b）watch观察
- c）call stack 调用栈
- d）break points 断点

它和chrome的调试也是一样的，此处就不多讲了。

### VSCode的其他调试方式

- 当前文件
- 附加到进程
- 附加到远程服务器
- 前端：推荐debugger for chrome插件
- 其他语言调试

课后作业：亲手debug一次，感受一下vsc的魅力

### 更多

- [node-debug 三法三例之node debugger + node inspector](https://github.com/i5ting/node-debug-tutorial/)
    - 通过旧版本协议附加（node debug），Node.js 6.3-
    - 通过检查器协议附加（v8-inspector），版本依赖是Node.js 6.3+
- [node-inspector视频](http://i5ting.github.io/nodejs-video/node-inspector.mov)
- [node-debug视频](http://i5ting.github.io/nodejs-video/node-debug.mov)

更多vsc用法，参见 https://github.com/i5ting/vsc

