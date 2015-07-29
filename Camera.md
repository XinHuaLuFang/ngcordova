#照相机`$cordovaCamera`

[official Apache Cordova Plugin](https://github.com/apache/cordova-plugin-camera)

>此插件定义了一个全局的`navigator.camera`对象，提供了拍照和从系统相册选取图片的API。<br>
虽然该对象附加到了全局范围的导航中，但是在`deviceready`事件之前都不可用。

###CameraOptions
可选参数自定义相机设置
'''
    quality : 75,
    destinationType : Camera.DestinationType.DATA_URL,
    sourceType : Camera.PictureSourceType.CAMERA,
    allowEdit : true,
    encodingType: Camera.EncodingType.JPEG,
    targetWidth: 100,
    targetHeight: 100,
    popoverOptions: CameraPopoverOptions,
    saveToPhotoAlbum: false };
'''

###CLI安装照相机插件
```
    cordova plugin add cordova-plugin-camera
```

###Examples
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
