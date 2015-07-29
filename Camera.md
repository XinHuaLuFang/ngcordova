#照相机`$cordovaCamera`

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
            destinationType: Camera.DestinationType.`DATA_URL`,
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
            destinationType: Camera.DestinationType.`FILE_URI`,
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
