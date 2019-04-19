---
title: 使用canvas给图片添加水印
catalog: true
date: 2018-12-12 17:44:46
subtitle:
header-img:
tags: [javascript, html, css, canvas]
---

## 使用canvas给图片添加水印

### css部分

```css
    .clip {
        position: absolute;
        clip: rect(0 0 0 0);
    }
```

### html部分

```html
    <input type="file" id="uploadFile" class="clip" accept="image/*">
    <label class="ui-button ui-button-primary" for="uploadFile">选择图片</label>
    <img id="imgCover" src="./images/googlelogo_color_272x92dp.png" class="clip">
    <p id="imgUploadX"></p>
```

### js部分

```javascript
    var eleUploadFile = document.getElementById('uploadFile');
    var eleImgCover = document.getElementById('imgCover');
    var eleImgUploadX = document.getElementById('imgUploadX');

    eleUploadFile.addEventListener('change', function(event) {
        // dataTransfer是拖拽操作的对象
        var file = event.target.files[0] || event.target.dataTransfer.files[0];

        var reader = new FileReader();

        reader.onload = function(e) {
            var base64 = e.target.result;

            // canvas压缩图片，并且回调显示
            imgTogether(base64, function(url) {
                eleImgUploadX.innerHTML = '<img src="' + url + '"/>';
            })
        }
        reader.readAsDataURL(file);
    });

    var imgTogether = function(url, callback) {
        var canvas = document.createElement('canvas');
        var context = canvas.getContext('2d');

        // image实例对象
        var imageUpload = new Image();
        imageUpload.onload = function() {
            console.log('imageUpload', imageUpload);
            canvas.width = imageUpload.width;
            canvas.height = imageUpload.height;
            // canvas绘制图片
            context.drawImage(imageUpload, 0, 0);
            // 绘制水印
             context.drawImage(eleImgCover, 0, 0);
            // 回调
            callback(canvas.toDataURL('image/jpeg'));
        }
        imageUpload.src = url;
    }
```


