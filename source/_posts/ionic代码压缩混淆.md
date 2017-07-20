---
title: ionic代码压缩混淆
date: 2017-07-19 18:41:12
categories:
- 前端-移动-ionic
tags:
- 前端
- 移动
- ionic
- 代码压缩混淆
---

> 参考：[http://www.bijishequ.com/detail/219770?p=54-70](http://www.bijishequ.com/detail/219770?p=54-70)

ionic工程发布时，有的工程由于代码量比较大，所以需要对js代码进行合并、压缩，而有的为了安全还需要代码混淆。本文参考上面的参考链接，进行了相应的调整，采用更适合自己项目的方法来进行代码合并、压缩和混淆。

文中的ionic项目目录结构如下：

![image](http://chuantu.biz/t5/141/1500382974x3031002306.png)

其中html和js文件都在scripts目录下，并且同一功能的js和html在同一目录下

> 本文基于ionic 1.\* 以及 angular 1.\*

## 将html页面转换为angular的JS代码

这一步是将html页面代码转换成angular的js代码（保存到一个js文件中）。

### 安装[gulp-angular-templatecache](https://www.npmjs.com/package/gulp-angular-templatecache)
```
$ npm install gulp-angular-templatecache --save-dev
```

### 修改gulpfile.js文件

加入以下代码：
```
var templateCache = require('gulp-angular-templatecache');

/**
 * 把www/scripts目录下的所有html文件合并成js代码,由于直接合并时生成的路径需要带上scripts,
 * 所以不直接取www/scripts目录,而是取www目录,再忽略除了scripts的其他目录
 */
gulp.task('templatecache', function(done){
    gulp.src(['./www/**/*.html', '!./www/index.html', '!./www/lib/**/*.html', '!./www/assets/**/*.html'])
        .pipe(templateCache({standalone:true}))
        .pipe(gulp.dest('./www/scripts'))
        .on('end', done);
});
```

### 在app.js中增加templates模块依赖

```
angular.module('starter', ['ionic','starter.controllers','templates'])
```

### 测试

```
$ gulp templatecache
```

执行完毕后能在www/scripts目录下看到templates.js文件，可以打开看看是不是想要的结果。

> 这时也可以在index.html中手动加入templates.js的引用，然后通过ionic serve就可以在浏览器测试一下效果

## 将www/scripts/templates.js引用加入到index.html

首先，执行完上一步后可以手动在index.hmtl中加入templates.js的引用，然后就可以通过`ionic serve`在浏览器看到效果。但是对于强迫症的我以及对于后面项目自动化打包来说，能自动是最好的。

### 安装[gulp-inject](https://www.npmjs.com/package/gulp-inject)

```
$ npm install gulp-inject --save-dev
```

### 修改www/index.html文件

在index.html中引入项目www/scripts目录下的js的地方（index.hmtl中项目自己写的js引入、lib里面的js、ionic框架的js以及一些远程的js的引入是分开的）加入以下代码：
```
<!-- inject:js -->
<!-- endinject -->
```

### 修改gulpfile.js文件

加入以下代码：

```
var inject = require('gulp-inject');

// 添加templates.js的引用到index.html
gulp.task('inject-templates', function (done) {
    gulp.src('./www/index.html')
        .pipe(inject(gulp.src('./www/scripts/templates.js', {read: false}), {relative: true}))
        .pipe(gulp.dest('./www'))
        .on('end', done);
});
```
### 测试

```
$ gulp templatecache
```

执行完后可以运行`ionic serve`测试

## 合并js及css文件

### 安装gulp-useref

> 这里安装2.1.0版本，可以看看最新版的差别然后进行相应的修改

```
$ npm install gulp-useref@2.1.0 --save-dev
```

### 修改gulpfile.js文件

加入以下代码：
```
var useref = require('gulp-useref');

// 合并index.html中引用的需要合并的js文件
gulp.task('useref', function(done){
    var assets = useref.assets();
    gulp.src('./www/index.html')
        .pipe(assets)
        .pipe(assets.restore())
        .pipe(useref())
        .pipe(gulp.dest('./www'))
        .on('end', done);
});
```

### 修改www/index.html文件

> 注意：可能有些外部的css文件或js文件不想被处理，那么就保持原状即可。

css文件处理(只列出部分文件)：
```
<!-- build:css css/lib.css -->
<link href="lib/ion-datetime-picker/release/ion-datetime-picker.min.css" rel="stylesheet">
<link href="lib/font-awesome/css/font-awesome.min.css" rel="stylesheet">
<!-- endbuild -->
```

> 我的项目`lib/ionic/css/ionic.min.css`不属于处理范围

lib js文件处理(只列出部分文件)：
```
<!-- build:js scripts/vendor.js -->
<script src="lib/jquery/dist/jquery.min.js"></script>
<script src="lib/angulartics/dist/angulartics.min.js"></script>
<!-- endbuild -->
```

> 我的项目`lib/ionic/js/ionic.bundle.min.js`和`cordova.js`不属于处理范围

项目js 文件处理(只列出部分文件)：
```
<!-- build:js scripts/app.join.js -->
<script src="scripts/app.js"></script>
<script src="scripts/constants.js"></script>
<!-- endbuild -->
```

### 测试

```
$ gulp useref
```
执行完后会生成以下文件：
```
www/css/lib.css
www/scripts/vendor.js
www/scripts/app.join.js
```
同时index.html也会改变，引入文件的地方会变成新生成的文件的引入。

然后通过`$ ionic serve`在浏览器测试

## 进行严格依赖注入

> 这里在代码合并之后才进行严格依赖注入，有两个原因：
> - lib里面的库有的并不是很规范，所以需要先把lib合并再进行依赖注入，所以如果不需要合并lib，就可以不用先合并再进行依赖注入
> - 先合并文件，再进行依赖注入，这样只需要对合并后的一两个文件做操作，而不需要改变所有的js文件

在进行代码压缩混淆之前，我们需要启用angular的ng-strict-di，即严格依赖注入，使得工程中依赖注入不会有问题，更多关于ng-strict-di可以看[这里](https://github.com/olov/ng-annotate#highly-recommended-enable-ng-strict-di-in-your-minified-builds)。

### 安装[gulp-ng-annotate](https://www.npmjs.com/package/gulp-ng-annotate)

```
$ npm install gulp-ng-annotate --save-dev
```

### 修改gulpfile.js文件

加入以下代码：
```
var ngAnnotate = require('gulp-ng-annotate');

// 严格依赖注入
gulp.task('annotate', function () {
    return gulp.src(['./www/scripts/vendor.js', './www/scripts/app.join.js'])
        .pipe(ngAnnotate({single_quotes:true}))
        .pipe(gulp.dest('./www/scripts'));
});
```

### 测试

```
$ gulp annotate
```
执行完后www/scripts下的js文件都会进行依赖注入调整

## 压缩混淆js代码

### 安装[gulp-uglify](https://www.npmjs.com/package/gulp-uglify)

```
$ npm install gulp-uglify --save-dev
```

### 修改gulpfile.js文件

加入以下代码：

```
var uglify = require('gulp-uglify');

// js代码压缩混淆
gulp.task('js-uglify', function () {
    gulp.src(['./www/scripts/vendor.js', './www/scripts/app.join.js'])
        .pipe(uglify())
        .pipe(gulp.dest('./www/scripts'));
});
```

### 测试

```
$ gulp js-uglify
```
执行完后可以看到scripts目录下的vendor.js和app.join.js是压缩混淆后的代码，然后可以通过`$ ionic serve`在浏览器测试

## 利用hooks删除platforms里面的开发文件

### 拷贝hooks文件[020_remove_dev_files_from_platform.js](https://github.com/MrLiZ/ionic_auto_package/blob/master/020_remove_dev_files_from_platform.js)
将文件添加到./hooks/after_prepare文件夹中，将相关的文件夹目录改成自己需要的目录，然后给文件添加执行权限：
```
$ chmod 755 hooks/after_prepare/020_remove_dev_files_from_platform.js
```

### 测试
可以运行以下命令测试：
```
$ ionic build android/ios --compress
```
执行完后相关目录下应该只会有压缩过后文件

### 题外：通过gulp删除开发文件

有时候如果需要进行热更新，比如code-push，就需要对开发目录进行调整，删除开发目录下不必要的文件，这时可以用gulp，在gulpfile加入以下代码：

```
var fs = require('fs');
var path = require('path');

// 删除dev文件,热更新的时候才需要用到,先删除dev文件再进行热更新
gulp.task('rm-dev-file', function () {
    /**
     * 删除指定目录下除了排除在外的所有文件和文件夹
     * @param removePath {string}    需要删除的目录
     * @param excludePaths {Array} 需要排除的目录
     * @param excludeFiles {Array} 需要排除的文件
     */
    var deleteFolderRecursive = function(removePath, excludePaths, excludeFiles) {

        // 如果传进来的不是list,通过转换成空list忽略处理
        excludePaths = Array.isArray(excludePaths) ? excludePaths : [];
        excludeFiles = Array.isArray(excludeFiles) ? excludeFiles : [];

        if( fs.existsSync(removePath) ) {
            fs.readdirSync(removePath).forEach(function(file,index){
                var curPath = path.join(removePath, file);
                // 如果是目录
                if(fs.lstatSync(curPath).isDirectory()) {
                    // 如果不是排除在外的目录,递归调用,否则忽略
                    excludePaths.indexOf(file) === -1 && deleteFolderRecursive(curPath);
                } else {
                    // 如果是文件,并且不是排除在外的文件,删除文件,否则忽略
                    excludeFiles.indexOf(file) === -1 && fs.unlinkSync(curPath);
                }
            });
            try {
                // 删除空目录
                fs.rmdirSync(removePath);
            } catch (err) {
                // 如果不是空目录会报错,暂时忽略
            }

        }
    };

    // lib 下 ionic的文件目录
    var ionicDir = path.resolve(__dirname, "./www/lib/ionic");
    // lib
    var libDir = path.resolve(__dirname, "./www/lib");
    // dev js
    var devJSDir = path.resolve(__dirname, "./www/scripts");
    // dev css
    var devCSSDir = path.resolve(__dirname, "./www/css");
    // assets 文件目录
    var assetsDir = path.resolve(__dirname, "./www/assets");

    deleteFolderRecursive(ionicDir, ["fonts"], ["ionic.min.css", "ionic.bundle.min.js"]);
    deleteFolderRecursive(libDir, ["ionic"], []);
    deleteFolderRecursive(devJSDir, [], ["app.join.js", "vendor.js"]);
    deleteFolderRecursive(devCSSDir, [], ["app.css"]);
    deleteFolderRecursive(assetsDir, [], []);
});
```
然后运行`$ gulp rm-dev-file`测试

## 最后

- 相关的配置命令、打包命令以及打包完的git checkout等命令可以通过shell脚本来一次性处理
- 不推荐在`$ ionic serve`时实时压缩混淆代码，一方面没有必要，另一方面开发的时候不压缩混淆会更方便调试
- 不推荐在gulpfile里面将压缩混淆的命令用`$ gulp.task('compress', ['templatecache', 'inject-templates', 'useref'...]);`这种方式来执行，因为gulp执行的顺序可能不是你想要的
