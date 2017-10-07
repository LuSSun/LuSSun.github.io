---
layout: post
title: "使用gulp构建自动化工作流"
date: 2017-09-07 08:55
comments: true
tags: 
    - gulp
---

- [x] 简单易用
- [x] 高效构建
- [x] 高质量的生态圈

**Gulp**就一种可以自动化完成我们开发过程中大量的重复工作，提高效率的构建工具。
- 预处理语言的编译
-  js css html 压缩混淆
-   图片体积优化

### 下载nodejs安装包


> 去nodejs官网下载软件https://nodejs.org

### 配置npm的全局路径


> Windows下的Nodejs npm路径是appdata，可能不是我们想要的路径，可以改成我们指定 的路径方便管理。 
> 在cmd下执行以下命令 
> npm config set cache “D:\nodejs\node_cache” 
> npm config set prefix “D:\nodejs\node_global” 
> 如果命令无效，可以去nodejs的安装目录中找到node_modules\npm\npmrc文件，这个文 件存放npm的userconfig配置。 
> 修改如下即可： 
> prefix = D:\nodejs\node_global 
> cache = D:\nodejs\ node_cache

### 全局安装gulp

> $ npm install --global gulp-cli

### 本地安装gulp

> $ npm install --save-dev gulp
 
### 创建配置文件
 在项目根目录下创建一个名为 gulpfile.js 的文件:
> touch gulpfile.js

### gulp使用
当配置完gulp.file后运行 gulp：

> $ gulp

### 安装相关插件

**自动加载插件（gulp-load-plugins）**

> 安装：npm install gulp-load-plugins –save-dev 
> 要使用gulp的插件，首先得用require来把插件加载进来，如果我们要使用的插件非常多，那我们的gulpfile.js文件开头可能就会是这个样子的：

```
var gulp = require('gulp'),
    //一些gulp插件,abcd这些命名只是用来举个例子
    a = require('gulp-a'), 
    b = require('gulp-b'),
    c = require('gulp-c'),
    //更多的插件...
    z = require('gulp-z');   
```
> 虽然这没什么问题，但会使我们的gulpfile.js文件变得很冗长，看上去不那么舒服gulp-load-plugins插件正是用来解决这个问题。 
> gulp-load-plugins这个插件能自动帮你加载package.json文件里的gulp插件。例如假 设你的package.json文件里的依赖是这样的:


```
"devDependencies": {
    "gulp": "^3.9.0",
    "gulp-concat": "^2.6.0",
    "gulp-cssnano": "^2.1.0",
    "gulp-htmlmin": "^1.3.0",
    "gulp-less": "^3.0.5",
    "gulp-uglify": "^1.5.1"
  }
```
然后我们可以在gulpfile.js中使用gulp-load-plugins来帮我们加载插件：


```
var gulp = require('gulp');
//加载gulp-load-plugins插件，并马上运行它
var plugins = require('gulp-load-plugins')();
```
**重命名（gulp-rename）**

> 安装：npm install gulp-rename –save-dev
> 用来重命名文件流中的文件。 
> 用gulp.dest()方法写入文件时，文件名使用的是文件流中的文件名，如果要想改变文件名，那可以在之前用gulp-rename插件来改变文件流中的文件名。


```
var gulp = require('gulp'),
    rename = require('gulp-rename'),
    uglify = require("gulp-uglify");

gulp.task('rename', function () {
    gulp.src('js/jquery.js')
    .pipe(uglify())  //压缩
    .pipe(rename('jquery.min.js')) //会将jquery.js重命名为jquery.min.js
    .pipe(gulp.dest('js'));
    //关于gulp-rename的更多强大的用法请参考https://www.npmjs.com/package/gulp-rename
});
```
**js文件压缩（gulp-uglify）**
> 安装：npm install gulp-uglify –save-dev 
> 用来压缩js文件，使用的是uglify引擎

```
var gulp = require('gulp'),
    uglify = require("gulp-uglify");

gulp.task('minify-js', function () {
    gulp.src('js/*.js') // 要压缩的js文件
    .pipe(uglify())  //使用uglify进行压缩,更多配置请参考：
    .pipe(gulp.dest('dist/js')); //压缩后的路径
});
```
 **css文件压缩（gulp-cssnano）**

> 安装：npm install gulp-cssnano –save-dev 
> 要压缩css文件时可以使用该插件


```
var gulp = require('gulp'),
    less = require('gulp-less'),
    cssnano = require('gulp-cssnano');
    
gulp.task('style', function() {
  // 这里是在执行style任务时自动执行的
  gulp.src('src/styles/*.less')
    .pipe(less())
    .pipe(cssnano())
    .pipe(gulp.dest('dist/styles'));
});

```
 **html文件压缩（gulp-htmlmin）**
> 安装：npm install gulp-htmlmin  –save-dev
> 用来压缩html文件

```
var gulp = require('gulp'),
    htmlmin = require("gulp-htmlmin");

gulp.task('minify-html', function () {
    gulp.src('html/*.html') // 要压缩的html文件
    .pipe(htmlmin({
        collapseWhitespace: true,
        removeComments: true
    })) //压缩
    .pipe(gulp.dest('dist/html'));
```
**文件合并（gulp-concat）**
> 安装：npm install gulp-concat –save-dev
> 用来把多个文件合并为一个文件,我们可以用它来合并js或css文件等，这样就能减少页面的http请求数了

```
var gulp = require('gulp'),
    concat = require("gulp-concat");

gulp.task('concat', function () {
    gulp.src('js/*.js')  //要合并的文件
    .pipe(concat('all.js'))  // 合并匹配到的js文件并命名为 "all.js"
    .pipe(gulp.dest('dist/js'));
});
```
**less的编译（gulp-less）**
> less使用gulp-less,安装：npm install gulp-less–save-dev

```
var gulp = require('gulp'),
    less = require('gulp-less');
    
gulp.task('style', function() {
  // 这里是在执行style任务时自动执行的
  gulp.src('src/styles/*.less')
    .pipe(less())
    .pipe(gulp.dest('dist/styles'));
});
```
**图片压缩（gulp-imagemin）**
> 可以使用gulp-imagemin插件来压缩jpg、png、gif等图片。 
> 安装：npm install gulp-imagemin –save-dev

```
var gulp = require('gulp');
var imagemin = require('gulp-imagemin');
var pngquant = require('imagemin-pngquant'); //png图片压缩插件

gulp.task('default', function () {
    return gulp.src('src/images/*')
        .pipe(imagemin({
            progressive: true,
            use: [pngquant()] //使用pngquant来压缩png图片
        }))
        .pipe(gulp.dest('dist'));
});
```
**自动刷新（browser-sync）**

> 安装:npm install browser-sync –save-dev 
> 当代码变化时，它可以帮我们自动刷新页面 


```
var gulp        = require('gulp');
var browserSync = require('browser-sync').create();

// 静态服务器
gulp.task('browser-sync', function() {
    browserSync.init({
        server: {
            baseDir: "./"
        }
    });
});

// 代理

gulp.task('browser-sync', function() {
    browserSync.init({
        proxy: "你的域名或IP"
    });
});
 //关于browser-sync的更多强大的用法请参考http://www.browsersync.cn/docs
```
### Gulpfile.js文件

> 以下是我的gulp文件，仅供交流。

```
//   HTML压缩 命名
var gulp = require('gulp'),
    htmlmin = require('gulp-htmlmin');

gulp.task('html',function(){
    gulp.src('./index.html')
        .pipe(htmlmin({
          collapseWhitespace: true,
          removeComments: true
        }))
        .pipe(rename('index.min.html'))
        .pipe(gulp.dest('./'))
        .pipe(browserSync.reload({
       stream:true
    }));
})
//  LESS编译 压缩
var less = require('gulp-less'),
    cssnano = require('gulp-cssnano'),
    rename = require('gulp-rename');

gulp.task('style',function(){
    gulp.src('./css/*.css')
        .pipe(cssnano())
        .pipe(rename('main.min.css'))
        .pipe(gulp.dest('css'))
        .pipe(browserSync.reload({
           stream:true
        }));
})
//  JS合并 压缩混淆
var uglify = require('gulp-uglify');

gulp.task('script',function(){
    gulp.src('./js/*.js')
        .pipe(uglify())
        .pipe(rename('main.min.js'))
        .pipe(gulp.dest('js'))
        .pipe(browserSync.reload({
           stream:true
        }));
})



gulp.task('image',function(){
    gulp.src('./img/*.*')
        .pipe(browserSync.reload({
       stream:true
    }));
})

var browserSync = require('browser-sync');

gulp.task('server',function(){
    browserSync({
    server: {
      baseDir: ['./']
    },
  }, function(err, bs) {
    console.log(bs.options.getIn(["urls", "local"]));
  });
    gulp.watch('./index.html',['html']);
    gulp.watch('./css/*.css',['style']);
    gulp.watch('./js/*.js',['script']);
    gulp.watch('./img/*.*',['image']);
})

```
### gulp的API介绍
> 使用gulp，仅需知道4个API即可：gulp.task(),gulp.src(),gulp.dest(),gulp.watch()