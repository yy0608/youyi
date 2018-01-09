---
title: node文件操作之自动化构建
date: 2018-01-06 16:17:27
tags:
  - 自动化构建
---
###### 使用node自动化生成项目
```
var fs = require('fs')
var path = require('path')

var project = {
  'name': 'test',
  'fileData': [
    {
      name: 'css',
      type: 'dir'
    },
    {
      name: 'js',
      type: 'dir'
    },
    {
      name: 'imgs',
      type: 'dir'
    },
    {
      name: 'index.html',
      type: 'file',
      content: '<!DOCTYPE html>\n<html>\n\t<head>\n\t<title>title</title>\n</head>\n<body>aaa</body>\n</html>'
    }
  ]
}

if (project.name) {
  if (fs.existsSync(path.join(__dirname, project.name))) {
    console.log('文件目录已经存在')
  } else {
    fs.mkdir(project.name, function (err) {
      if (err) {
        console.log('创建文件出错')
      } else {
        console.log('创建文件目录成功')

        var fileData = project.fileData
        if (fileData && fileData.forEach) {
          fileData.forEach(function (f) {
            f.path = project.name + '\\' + f.name
            f.content = f.content || ''

            switch (f.type) {
              case 'dir':
                fs.mkdirSync(f.path)
                break

              case 'file':
                fs.writeFileSync(f.path, f.content)
                break

              default:
                break
            }
          })
        }
      }
    })
  }
}
```
- 1.fs.existsSync，判断目录是否存在
- 2.fs.mkdir，创建目录，可以多层创建，需要上一级目录存在
- 3.fileData.forEach，判断是否数组的新方法，instanceof，Array.isArray()
- 4.fs.writeFileSync，写入文件，传路径和内容

###### 使用node执行系统命令，可以在目录生成完成之后执行，类似npm 的scripts
```
var exec = require('child_process').exec
var cliCmd = 'browser-sync start --server --files "**/*.*"' // ipconfig

exec(cliCmd, { encoding: 'utf8' }, function (err, stdout, stderr) { // 执行系统命令
  if (err) {
    console.log(err)
  } else if (stderr) {
    console.log(stderr)
  } else {
    console.log(stdout)
  }
})
```