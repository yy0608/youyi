---
title: 文件ajax异步上传node服务器
tags:
  - ajax文件上传
abbrlink: 16270
date: 2018-01-09 14:47:56
---
###### ajax上传文件，本文以图片为例，node作为处理服务器
###### html文件，XMLHttpRequest 2
```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>

<body>
  <input type="file" name="file" id="file">
  <button id="upload">上传</button>
  <div id="progress">0</div>
  <img src="" width="200" id="image">
</body>

<script>
;(function () {
  'use strict'

  var xhr = new XMLHttpRequest()

  upload.addEventListener('click', uploadFile, false)
  file.addEventListener('change', previewImage, false)

  function uploadFile () {
    var formData = new FormData()
    formData.append('test-upload', file.files[0])
    xhr.onload = uploadSuccess
    xhr.upload.onprogress = setProgress

    xhr.open('post', '/upload', true)

    xhr.send(formData)
  }

  function uploadSuccess () {
    if (xhr.readyState === 4) {
      console.log(xhr.responseText)
    }
  }

  function setProgress (event) {
    if (event.lengthComputable) {
      var complete = parseInt(event.loaded / event.total * 100)
      progress.innerHTML = complete + '%'
    }
  }

  function previewImage (event) {
    var reader = new FileReader()
    reader.onload = function (event) {
      image.src = event.target.result
    }
    reader.readAsDataURL(event.target.files[0])
  }

})()
</script>

</html>
```
###### node服务端文件，express和multer中间件
```
const express = require('express')
const upload = require('multer')({ dest: 'uploads/' })

const path = require('path')
const fs = require('fs')

const port = 3000

let app = express()

app.set('port', port)

app.use(express.static('./'))

app.post('/upload', upload.single('test-upload'), function (req, res) {
  if (!req.file) {
    res.json({ ok: false })
    return
  }

  console.log('-------------------------------------------')
  console.log(req.file)
  console.log('-------------------------------------------')

  res.json({ ok: true })
})

app.listen(port, function () {
  console.log('listen on port' + port)
})

```