#ngcordova
##1.如何在ionic项目中应用ngcordova？
进入到工程目录，使用bower安装ngcordova
```
    bower install ngCordova
```

将ngcordova的js文件添加到cordova.js之前，注意cordova.js不需要修改，也不能删除，否则无法实现调用手机API功能，我猜应该是编译的时候会指向正确的cordova路径，我猜的，谁知道正确的欢迎指正，fork后pull后者加我企鹅:three::four::nine::four::six::seven::two::two::one:
```
    <!-- cordova script (this will be a 404 during development) -->
    <script src="lib/ngCordova/dist/ng-cordova.min.js"></script>
    <script src="cordova.js"></script>
```
添加ngCordova依赖
```
    angular.module("app", ["ngCordova"]);
```


