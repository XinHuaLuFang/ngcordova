#照相机`$cordovaCamera`

###介绍

[official Apache Cordova Plugin](https://github.com/apache/cordova-plugin-camera)

>此插件定义了一个全局的`navigator.camera`对象，提供了拍照和从系统相册选取图片的API。<br>
虽然该对象附加到了全局范围的导航中，但是在`deviceready`事件之前都不可用。

####CameraOptions
可选参数自定义相机设置
```
{
    quality : 75,
    destinationType : Camera.DestinationType.DATA_URL,
    sourceType : Camera.PictureSourceType.CAMERA,
    allowEdit : true,
    encodingType: Camera.EncodingType.JPEG,
    targetWidth: 100,
    targetHeight: 100,
    popoverOptions: CameraPopoverOptions,
    saveToPhotoAlbum: false 
};
```
* **quality**：图片质量，范围0到100，100是指全分辨率无损文件压缩，缺省值为`50`。（注意相机的分辨率信息不可用。）
* **destinationType**：设置返回值的格式。缺省值为`FILE_URI`。:octocat:FILE_URI和NATIVE_URI的区别
```
    Camera.DestinationType = {
        DATA_URL: 0,    //把图片以base64字符串返回
        FILE_URI: 1,    //返回图片文件的URI
        NATIVE_URI: 2,  //返回图片本地URI（例如，IOS为assets-library://，android为content://）
    };
```
* **sourceType**：设置图片来源，缺省值为`CAMERA`。:octocat:添加参数说明
```
    Camera.PictureSourceType = {
        PHOTOLIBRARY: 0,    //
        CAMERA: 1,          //
        SAVEDPHOTOALBUM: 2  //
    };
```
* **allowEdit**：选择图片前是否允许简单编辑图片，布尔值。:octocat:缺省值以及编辑方式
* **encodingType**：设置返回图片文件的编码格式。缺省值为`JPEG'。
```
    Camera.EncodingType = {
        JPEG: 0,
        PNG: 1
    };
```
* **targetWidth**：图片的像素宽度，需同时设置targetHeight。长宽比例保持不变。
* **targetHeight**：图片的像素高度，需同时设置targetWidth。长宽比例保持不变。
* **popoverOptions**：:octocat:未理解
* **saveToPhotoAlbum**：捕获图像完成后将图像保存到相册中，布尔值。:octocat:未给出缺省值
* :octocat:缺少三个参数，`mediaType`、`correctOrientation`、`cameraDirection`

####CameraError
错误回调函数提供了错误信息
```
    function(error) {
        //显示错误信息
    };
```
* **error**：由手机内部系统决定。
####CameraSuccess
成功回调函数提供了图像数据
```
    function(imageData) {
        //处理图片
    };
```
* **imageData**：Base64编码的图片数据或者图片的FILE URI，由`CameraOptions`中的参数`destinationType`决定。

---
###一些坑，小心掉下去了:joy:

####Android
####IOS
####Windows Phone 7 and 8
####Amazon Fire OS
####BlackBerry
####FireFox OS
####Tizen

---
###使用

####CLI安装照相机插件
```
    cordova plugin add cordova-plugin-camera
```

####Examples
返回base64编码的图片数据
```javascript
module.controller("ctrl", function($scope, $cordovaCamera) {
    $scope.photograph = function() {
        var options = {
            quality: 50,
            destinationType: Camera.DestinationType.DATA_URL,
            sourceType: Camera.PictureSourceType.CAMERA,
            allowEdit: true,
            encodingType: Camera.EncodingType.JPEG,
            targetWidth: 100,
            targetHeight: 100,
            popoverOptions: CameraPopoverOptions,
            saveToPhotoAlbum: false
        };
        $cordovaCamera.getPicture(options).then(function(imageData) {
            var image = document.getElementById('myImage');
            image.src = "data:image/jpeg;base64," + imageData;
        }, function(error) {
            //error
        });
    };
});
```
返回图片存储路径
```javascript
module.controller("ctrl", function($scope, $cordovaCamera) {
    $scope.photograph = function() {
        var options = {
            destinationType: Camera.DestinationType.FILE_URI,
            sourceType: Camera.PictureSourceType.CAMERA,
        };
        $cordovaCamera.getPicture(options).then(function(imageURI) {
            var image = document.getElementById('myImage');
            image.src = imageURI;
        }, function(err) {
            // error
        });
    };
});
```
