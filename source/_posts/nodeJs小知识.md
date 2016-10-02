title: nodeJs小知识
date: 2016-9-9 17:50:23
tags: nodeJs
categories: 前端
thumbnail: http://7xn0vc.com1.z0.glb.clouddn.com/nodeJS_icon.jpg
---

- nodeJs直接调用grunt任务
  nodeJs里直接调用grunt任务其实很简单，直接用grunt.cli来运行：
```jvascript
var grunt = require('grunt');
var path = require('path');
grunt.cli({
    gruntfile: __path + '/Gruntfile.js'
})
```
还可以直接设置默认任务：
```javascript
grunt.registerTask("default", [`project-watch:${project_config.projectName}`]);
```
- nodeJs获取上层目录

```javascript
const path = require('path');
var grunt = require("grunt");
var __path = path.resolve(__dirname, '..')
```

待续...