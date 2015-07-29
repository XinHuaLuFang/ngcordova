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
接下来就是具体某个插件的使用，在插件名对应的`.md文件`中查看使用方法。


#我的社区

###:+1:免费资源交流群:one::four::zero::six::two::five::four::two::four:
* 电子书分享、视频教程分享
* 啥都可以聊（除了政治），聊天、聊地、聊股票、聊人生、聊爱好...........
* 欢迎守法公民加入

###:+1:西安前端开发者分享交流会 WDShare
* [W3C Developer Share官网](http://www.wdshare.org/)
* 欢迎前端高手加入
* 群内高手
    * [大漠-w3cplus](http://www.w3cplus.com/)
    * [司徒正美-去哪儿](https://github.com/RubyLouvre)
    * [杰克](http://www.cnblogs.com/jikey)
    * [F7-付琦](http://www.imf7.com/)
    * []()
    * []()
    * []()
