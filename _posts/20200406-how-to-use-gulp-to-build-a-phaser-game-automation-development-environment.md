---
title: 如何使用Gulp搭建Phaser游戏自动化开发环境
date: 2020-04-06
updated: 2020-04-30
tags:
  - Node.js
  - Gulp
  - Phaser
urlname: how-to-use-gulp-to-build-a-phaser-game-automation-development-environment
original: true
---
使用gulp4.0.2
<!--more-->
# 环境 
nodejs v12.16.1; npm 6.13.4; npx 6.13.4; gulp-cli 2.2.0;
# 步骤
项目中创建package.json配置文件: 
~~~
npm init
~~~
开发依赖安装gulp包: 
~~~
npm install --save-dev gulp
~~~
检查版本: 
~~~
gulp -v
~~~
创建gulpfile.js: 
~~~
function defaultTask(cb) {
    // place code for your default task here
    console.log('hello gulp');
    cb();
  }  
  exports.default = defaultTask
~~~
测试运行: 
~~~
$ gulp
[12:07:26] Using gulpfile ~\OtherProject\gulp-project\gulpfile.js
[12:07:26] Starting 'default'...
hello gulp
[12:07:26] Finished 'default' after 10 ms
~~~
可以看到已经能够成功跑起来gulp, 接下来就定义一些自动化构建任务了. 
在项目下创建文件夹: 
/dist 运行文件的发布目录
/modules 游戏的各个模块
/resource 游戏所需要的资源
项目下创建文件: 
index.html 游戏入口
main.js 游戏入口
然后安装依赖: 
~~~
npm install gulp-webserver --save-dev
npm install gulp-browserify --save-dev
~~~
然后修改index.html文件在最后引入两个脚本文件, 第一段是我本地的phaser3.22.0依赖库: 
~~~
<script src="resource/phaser.js"></script>
<script src="./dist/main.js"></script>
~~~
接下来在main.js中写phaser相关内容啦: 
~~~
var config = {
    type: Phaser.AUTO,
    width: 800,
    height: 600, 
    scene: {
        preload: preload, 
        create: create, 
        update: update
    }
};
var game = new Phaser.Game(config);
function preload () {
    console.log('preload');
}
function create () {
    console.log('create');
}
function update () {
    console.log('update');
}
~~~
最后修改我们之前新建的gulpfile.js为: 
~~~
var gulp = require('gulp');
var webserver = require('gulp-webserver');
var browserify = require("gulp-browserify");
gulp.task('webserver', async() => {
    return gulp.src('./')
        .pipe(webserver({
            livereload: false,
            directoryListing: true,
            host:"0.0.0.0",
            open: 'http://127.0.0.1:8000/index.html'
        }));
});
gulp.task("browserify",function(){
    return gulp.src("./main.js") 
        .pipe(browserify({
            insertGlobals: true
        }))
        .pipe(gulp.dest("./dist/"));
});
gulp.task('watch', function () {
    gulp.watch(['./main.js', './modules/**/*.js'],gulp.series('browserify'));
});
gulp.task('default', gulp.series("browserify", 'webserver',gulp.parallel('watch', function () {
    console.log("Server is running now");
})
));


~~~
然后直接运行: 
~~~
gulp
~~~
浏览器就会自动打开 http://127.0.0.1:8000/index.html 地址, 这就大工完成啦. 
# 解析
在最后运行 gulp 命令的时候, 它首先会去执行 gulpfile.js 下的 default 任务, 该任务会串行执行在上面定义过的 browserify, webserver 任务, 再并行执行 watch 和匿名函数. 
- browserify 任务中以main.js作为入口文件, 它会自动分析入口文件内的模块依赖, 然后打包到 dist 目录下; 
- webserver 任务则负责搭建本地服务器, livereload 属性可以设置是否开启热刷新. 
- watch 任务则是调用 gulp.watch 函数, 该函数允许匹配的文件发生改变的时候调用其他任务, 在本例子中该函数会检查main.js, 和modules文件夹下所有的.js文件是否发生变更, 如过发生变更则执行 browserify 任务. 

# 最后
gulp作为前端自动化构建工具给开发带来了不少便利~ 例子里就写了js的compile, 你也可以写html, css, coffee, scss等的compile. 

参考: 
[使用Phaser来开发我的炉石传说(一)](https://sangliang.github.io/Timing-House/2017/04/15/%E4%BD%BF%E7%94%A8Phaser%E6%9D%A5%E5%BC%80%E5%8F%91%E6%88%91%E7%9A%84%E7%82%89%E7%9F%B3%E4%BC%A0%E8%AF%B4(%E4%B8%80)/)
[译 Gulp 4 入门指南](https://github.com/baixing/FE-Blog/issues/7)
[gulp官网](https://gulpjs.com/)















