---
title: 将图片转化为base64并上传
tags:
  - base64
abbrlink: 29966
date: 2018-03-02 15:27:11
---
#### 通过file接口和canvas将图片转为base64，也可以通过blob格式上传
```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>upload</title>
</head>

<body>
  <img id="avatar" src="" alt="">
  <input type="file" id="file">
</body>
<script>

getImgageBase64(document.querySelector('#file'), 100, 100, 'image/jpg', 0.8, function (data) {
  avatar.src = data
})


function getImgageBase64 (eleFile, maxWidth, maxHeight, imgType, quality, cb) {
  if (!eleFile) cb && cb('')
  var maxWidth = maxWidth || 160;
  var maxHeight = maxHeight || 160;
  var quality = quality || 1;
  var imgType = imgType || 'image/jpg'
  // 压缩图片需要的一些元素和对象
  var reader = new FileReader(),
    img = new Image();

  // 选择的文件对象
  var file = null;

  // 缩放图片需要的canvas
  var canvas = document.createElement('canvas');
  var context = canvas.getContext('2d');

  // base64地址图片加载完毕后
  img.onload = function() {
    // 图片原始尺寸
    var originWidth = this.width;
    var originHeight = this.height;
    // 目标尺寸
    var targetWidth = originWidth,
      targetHeight = originHeight;
    // 图片尺寸超过400x400的限制
    if (originWidth > maxWidth || originHeight > maxHeight) {
      if (originWidth / originHeight > maxWidth / maxHeight) {
        // 更宽，按照宽度限定尺寸
        targetWidth = maxWidth;
        targetHeight = Math.round(maxWidth * (originHeight / originWidth));
      } else {
        targetHeight = maxHeight;
        targetWidth = Math.round(maxHeight * (originWidth / originHeight));
      }
    }

    // canvas对图片进行缩放
    canvas.width = targetWidth;
    canvas.height = targetHeight;
    img.width = targetWidth;
    img.height = targetHeight;
    // 清除画布
    context.clearRect(0, 0, targetWidth, targetHeight);
    // 图片压缩
    context.drawImage(img, 0, 0, targetWidth, targetHeight);
    // canvas转为blob并上传
    // canvas.toBlob(function(blob) {
    //   // 图片ajax上传
    //   var xhr = new XMLHttpRequest();
    //   // 文件上传成功
    //   xhr.onreadystatechange = function() {
    //     if (xhr.status == 200) {
    //       // xhr.responseText就是返回的数据
    //     }
    //   };
    //   // 开始上传
    //   xhr.open("POST", 'upload.php', true);
    //   xhr.send(blob);
    // }, file.type || 'image/png');
    var resBase64 = canvas.toDataURL(imgType, quality)
    cb && cb(resBase64)
  };

  // 文件base64化，以便获知图片原始尺寸
  reader.onload = function(e) {
    img.src = e.target.result;
  };
  eleFile.addEventListener('change', function(event) {
    file = event.target.files[0];
    // 选择的文件是图片
    if (file.type.indexOf("image") == 0) {
      reader.readAsDataURL(file);
    }
  });
}
</script>

</html>
```
