---
title: Pack pictures to zip and download it
date: 2018-06-06 17:20:51
tags: learning
---
# Package image for "zip" in H5
### Problem Introduce
Sometimes you have many images need to download, and you will download them one by one, this`s so tired. 

### Solve IDEA
```
<!DOCTYPE html>  
<html>  
    <head>  
        <meta charset="utf-8" />  
        <title></title>  
        <style>  
            img{  
                width: 200px;  
                height: 150px;  
            }  
        </style>  
    </head>  
    <body>  
          
        <img src="http://hiracerapp.oss-cn-hangzhou.aliyuncs.com/test.jpeg" />  
        <img src="http://hiracerapp.oss-cn-hangzhou.aliyuncs.com/test.jpeg" />  
        <img src="http://hiracerapp.oss-cn-hangzhou.aliyuncs.com/test.jpeg" />  
        <img src="http://hiracerapp.oss-cn-hangzhou.aliyuncs.com/test.jpeg" />  
        <br /><br />  
        <button onclick="packageImages()">packageImages</button><span id="status"></span><br /><br />  
          
        <script type="text/javascript" src="js/jquery.min.js"></script>  
        <script type="text/javascript" src="js/jszip.min.js"></script>  
        <script type="text/javascript" src="js/FileSaver.js"></script>  
        <script type="text/javascript">  
        function packageImages(){  
            $('#status').text('处理中。。。。。');  
              
            var imgs = $('img');  
              
            var imgsSrc = [];  
            var imgBase64 = [];  
            var imageSuffix = [];//图片后缀  
            var zip = new JSZip();  
            zip.file("readme.txt", "打包说明\n");  
            var img = zip.folder("images");  
              
            for(var i=0;i<imgs.length;i++){  
                var src = imgs[i].getAttribute("src");  
                var suffix = src.substring(src.lastIndexOf("."));  
                imageSuffix.push(suffix);  
                  
                 getBase64(imgs[i].getAttribute("src"))  
                    .then(function(base64){  
                        imgBase64.push(base64.substring(22));  
                           
                          //console.log(base64);//处理成功打印在控制台  
                    },function(err){  
                          console.log(err);//打印异常信息  
                    });   
                     
            }  
            function tt(){  
                setTimeout(function(){  
                    if(imgs.length == imgBase64.length){  
                        for(var i=0;i<imgs.length;i++){  
                            img.file(i+imageSuffix[i], imgBase64[i], {base64: true});  
                        }  
                        zip.generateAsync({type:"blob"}).then(function(content) {  
                            // see FileSaver.js  
                            saveAs(content, "images.zip");  
                        });  
                        $('#status').text('处理完成。。。。。');  
                          
                    }else{  
                        //console.log('imgs.length:'+imgs.length+',imgBase64.length:'+imgBase64.length);  
                        $('#status').text('已完成：'+imgBase64.length+'/'+imgs.length);  
                        tt();  
                    }  
                },100);  
                  
            }  
            tt();  
              
        }  
  
    //传入图片路径，返回base64  
    function getBase64(img){  
        function getBase64Image(img,width,height) {//width、height调用时传入具体像素值，控制大小 ,不传则默认图像大小  
          var canvas = document.createElement("canvas");  
          canvas.width = width ? width : img.width;  
          canvas.height = height ? height : img.height;  
   
          var ctx = canvas.getContext("2d");  
          ctx.drawImage(img, 0, 0, canvas.width, canvas.height);  
          var dataURL = canvas.toDataURL();  
          return dataURL;  
        }  
        var image = new Image();  
        image.crossOrigin = 'Anonymous';  
        image.src = img;  
        var deferred=$.Deferred();  
        if(img){  
          image.onload =function (){  
            deferred.resolve(getBase64Image(image));//将base64传给done上传处理  
          }  
          return deferred.promise();//问题要让onload完成后再return sessionStorage['imgTest']  
        }  
      }  
      
</script>  
    </body>  
</html> 
```

### PITS
1. use the best new version filesave.js will be wrong.
```
https://www.jq22.com/juqery-info11353
```
2. use [canvas.toDataURL] load to  "xx Access-Control-Allow-Origin xx" error
```
This is a Cross domain question on the surface,
and I solved it by add a time identification =>
...image.setAttribute('crossOrigin','anonymous');
+ var timestamp = new Date().getTime();
+ image.src = img + '?' + timestamp;
var deferred = $.Deferred();...
```
