#照相机`$cordovaCamera`

###介绍

[official Apache Cordova Plugin](https://github.com/apache/cordova-plugin-camera)

>此插件定义了一个全局的`navigator.camera`对象，提供了拍照和从系统相册选取图片的API。<br>
虽然该对象附加到了全局范围的导航中，但是在`deviceready`事件之前都不可用。

####CameraOptions
可选参数自定义相机设置
```
{
    quality : 75,                                       //0~100
    destinationType : Camera.DestinationType.DATA_URL,  //0 || 1 || 2
    sourceType : Camera.PictureSourceType.CAMERA,       //0 || 1 || 2
    allowEdit : true,                                   //bool
    encodingType: Camera.EncodingType.JPEG,             //0 || 1
    targetWidth: 100,
    targetHeight: 100,
    popoverOptions: CameraPopoverOptions,
    saveToPhotoAlbum: false,                            //bool
    cameraDirection: Camera.Direction.BACK,             //0 || 1
    mediaType: Camera.MediaType.PICTURE,                //0 || 1 || 2
    correctOrientation: true                            //bool
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

* **cameraDirection**：照相机使用前置或后置摄像头，缺省值为`BACK`。
```
    Camera.Direction = {
        BACK: 0,    //后置摄像头
        FRONT: 1    //前置摄像头
    };
```

* **mediaType**：设置选择的媒体类型，只有当`sourceType`为`PHOTOLIBRARY`或`SAVEDPHOTOALBUM`时生效。缺省值为`PICTURE`
```
    Camera.MediaType = {
        PICTURE: 0,     //图片，通过destinationType的设置返回指定格式。
        VIDEO: 1,       //视频，总是返回FILE_URI
        ALLMEDIA: 2     //所有媒体类型
    };
```

* **correctOrientation**：照相时是否根据设备方向正确旋转图片。

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
* 当安卓试图启动相机照相时，并且系统内存过低时，cordova进程可能会被杀死。在这种情况下，当cordova进程恢复时图片将丢失。
* 无论`cameraDirection`设置为何值，都会调用后置摄像头。
* `sourceType`中，`Camera.PictureSourceType.PHOTOLIBRARY`和`Camera.PictrueSourceType.SAVEDPHOTOALBUM`都会选择相同的相册。
* Android also uses the Crop Activity for allowEdit, even though crop should work and actually pass the cropped image back to Cordova, the only one that works consistently is the one bundled with the Google Plus Photos application. Other crops may not work.:octocat:谁来翻译下这段

####IOS
* destinationType设置为Camera.destinationType.FILE_URI时，图片存储在应用的临时目录中，当应用结束时临时目录将被清除。
* 在两个回调函数的任何一个中添加js方法`alert()`将会导致错误，应该使用`setTimeout()`将alert包括起来，让IOS相册选择或弹出窗口完全关闭后，再执行alert。:octocat:setTimeout设置0延迟的原因
```
    setTimeout(function() {
        //添加自定义操作
    }, 0);
```

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

* 在给CameraOptions设置参数时，可以使用对象属性方式，也可以使用对象属性的值，使用前者方便理解，使用后者简单，但是后者不易于修改。
```
    sourceType: Camera.PictureSourceType.CAMERA
    surceType: 1
```
