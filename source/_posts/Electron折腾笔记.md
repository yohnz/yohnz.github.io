title: Electron折腾笔记
date: 2016-09-09 15:30:32
tags: Electron
categories: 前端
thumbnail: http://7xn0vc.com1.z0.glb.clouddn.com/electron_icon.jpg
---


Electron折腾笔记

### 背景

部门的项目每次开发都需要手动开启三个服务：server、webpack、grunt，每个服务都要输入一些东西（端口号，项目命，项目类型，启动器名）。而且，在推送调试的时候，这三项服务非常容易被终止掉，然后又得一个个开起来，总之每天都要来上那么10几遍吧。看图：
![服务](http://7xn0vc.com1.z0.glb.clouddn.com/electron_1.png)
 <!-- more -->
### 思考

如果可以一键启动三个服务，又可以免去手动输入参数岂不又多了点偷懒的时间？

### 功能
- 一键启动、关闭项目开发过程中需要开启的三项服务
- 自动解析出服务运行所需的三个不同目录，免去一个个手动输入的麻烦
- 可以保存配置，下次免配置

### 开工
因为想做成小软件的形式，所以选用了Electron。以下是一些主要的思路和功能代码记录。
#### 工具窗口
  工具窗口部分，基本上用官网guide里的代码就足够了
```javascript
const electron = require("electron");
const {app, BrowserWindow, shell, ipcMain} = electron;
let win;
function createWdindow() {
    win = new BrowserWindow({ width: 400, height: 680,resizable:true,title: 'prensterTool',})
    win.loadURL(`file://${__dirname}/index.html`)
    win.on("close", () => {       
        win = null;
    });
}
app.on("ready", createWdindow);
// 当全部窗口关闭时退出
app.on('window-all-closed', () => {    
    // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
    // 否则绝大部分应用及其菜单栏会保持激活。
    if (process.platform !== 'darwin') {
        app.quit();
    }
});
app.on('activate', () => {
    // 在 macOS 上，当点击 dock 图标并且该应用没有打开的窗口时，
    // 绝大部分应用会重新创建一个窗口。
    if (mainWindow === null) {
        createWindow();
    }
});
```
####  自定义菜单
```javascript
const remote = require('electron').remote;
const Menu = remote.Menu;
const MenuItem = remote.MenuItem;

var template = [
  {
    label: '配置',
    submenu: [
      { label: '重新配置', accelerator: 'CmdOrCtrl+shift+alt+r', click: function () { reset(); } },
      { label: '保存配置', accelerator: 'CmdOrCtrl+shift+alt+s', click: function () { saveSetting(); } }
    ]
  }
  // ……
];
var menu = Menu.buildFromTemplate(template);
Menu.setApplicationMenu(menu);
```
####  保存配置信息
  在工作目录拖拽小工具上时，读取文件目录，本写入配置文件，供后面的功能使用。
  **读取文件路径**：
```javascript
var file = e.dataTransfer.files[0];
presenterPath = file.path;
showInfo(presenterPath);
```
**写入配置文件**
```javascript
 var data = `module.exports =` {projectName:"${projectName}",presenterId:"${presenterId}"}`;
  fs.writeFile("./resources/app/project_config.js", data, function (err) {
    if (err) {
      showInfo(err.toString());
      return;
    }
```
#### 调用服务
调用服务部分，主要用到nodejs的子进程`exec`。
```javascript
functon startServices(){
// webpack  
var processWebpack = childProcess.exec('webpack --watch', { 'cwd': `${__path}/presenters/SubjectToolSolution/${presenterId}/src` });
showLog('Webpack', processWebpack);
pids.push(processWebpack.pid);
// server
var processServices = childProcess.exec('node services', { 'cwd': __path });
showLog('Server', processServices);
pids.push(processServices.pid);
// grunt
var processGrunt = childProcess.exec('node gruntTask', { 'cwd': __dirname });
showLog('Grunt', processGrunt);
pids.push(processGrunt.pid);
}
```
#### 踩坑
- Electron关闭的时候不会关闭node的子进程，所以，要手动关闭。
  开始做的时候，服务总是偶尔开启成功，后面就不行了，很是诡异，查了一下午都无果，直到我无意中开了任务管理器，看到那一堆的node服务...

  思路：开启服务的时候把子进程的pid存起来，待后面kill.
```javascript
var platform = process.platform;
function killTask() {
  try {
    if (platform === 'win32') {
      for (let pid of pids) {
        childProcess.exec('taskkill /pid ' + pid + ' /T /F');
      }
      pids = [];
    } else {
      for (let pid of pids) {
        process.kill(processServices.pid);
      }
      pids = [];
    }
  } catch (e) {
    showInfo('pid not found');
  }
  domLog.innerHTML = "";
  showInfo("服务已停止!");
  clearInterval(timerId);
}
```

- Electron打包完之后，代码里的path变了，要手动处理下，不知道是不是姿势不对？

#### ELectron打包

关于打包的问题，官网的文档看的我真是一脸懵逼，知乎专栏的这篇文章不错，可以参考下https://zhuanlan.zhihu.com/p/20225295

- 步骤
1. 安装electron-packager：`npm install --save-dev electron-packager`
2. package.json里添加打包命令：
   ```json
    "scripts": {
    "start": "electron .",
    "package": "electron-packager ./ app --win --out presenterTool --arch=x64 --version 1.3.4 --overwrite --ignore=node_modules" 
     }
   ```

3. 执行`npm run-script package`开始打包
4. 参数说明

  `
  electron-packager <location of project> <name of project> <platform> <architecture> <electron version> <optional options>
  `

    `location of project`：应用目录;

    `name of project`：应用名称;

    `platform`：要打包的平台;

    `architecture`：x86 or x64架构;

    `electron version`：electron 版本(不是应用版本);

    `optional options`：其它选项;

- 第一次打包会下载相应的包，时间比较久
- 记得打包参数里写上`overwrite`，不然后面打包时不会重新构建。

最后，上一张初步完成之后的运行图：

![](http://7xn0vc.com1.z0.glb.clouddn.com/electron_1.gif)
