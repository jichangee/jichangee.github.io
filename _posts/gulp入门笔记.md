入门指南在这里[gulp入门教程](http://www.gulpjs.com.cn/docs/getting-started/)
## 开始
```
npm i -g gulp        // 全局安装gulp
npm i gulp --save    // 在项目中安装gulp
```
在项目根目录下创建一个gulpfile.js文件，写入以下代码
```javascript
const gulp = require('gulp');

const input = 'lib/',
      output= 'build/';

gulp.task('default', function () {
  return gulp.src(input + 'js/*.js')
    .pipe(gulp.dest(output + 'js'));
});
```
gulp 会执行名称为“default”的任务

然后在终端输入gulp，执行完毕之后，lib/js/下的所有js文件会被写入到build/js/目录之下

这是gulp基本的功能，如果需要其他功能要安装gulp插件。
## 插件
gulp提供了许多插件来实现各种功能
```
npm i gulp-ruby-sass gulp-babel gulp-autoprefixer gulp-minify-css gulp-uglify gulp-rename gulp-notify gulp-sourcemaps del vinyl-paths babel-preset-es2015 --save
```
执行上面这条代码，安装所需插件

gulp-autoprefixer： 可以添加css兼容前缀

gulp-notify： 用来输出log信息

vinyl-paths： 用来获取 stream 中每个文件的路径
## 配置
我们先来写一个可以转换es6代码并压缩的task
```javascript
gulp.task('scripts', function () {
  return gulp.src(input + 'js/*.js')
    .pipe(rename({
      suffix: '.min'
    }))
    .pipe(babel({
      presets: ['es2015']
    }))
    .pipe(uglify())
    .pipe(gulp.dest(output + 'js'))
    .pipe(notify({
      message: 'Scripts task complete'
    }));
});
```
第一个pipe：将要输入的文件名后缀添加上 .min

第二个pipe：转换es6代码

第三个pipe：压缩js代码

第四个pipe：将处理完毕的js代码写到输出目录

第五个pipe：输出log信息


------------


再写一个处理css文件的task
```
gulp.task('sass', function () {
  return sass(input + 'css/*.scss')
    .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
    .pipe(rename({
      suffix: '.min'
    }))
    .pipe(minifycss())
    .pipe(gulp.dest(output + 'css/'))
    .pipe(notify({
      message: 'Sass task complete'
    }));
});
```
gulp-ruby-sass：sass()需要传入转换的scss文件路径才能执行，所以不是再使用gulp.src() 来获取目标文件了

第一个pipe：添加css兼容前缀

第二个pipe：重命名

第三个pipe：压缩

第四个pipe：写出文件

第五个pipe：输出log信息

gulp-sass：可以使用gulp.src()来获取目标文件，具体如下
```
...
return gulp.src(input + 'css/*.scss')
  .pipe(sass())
...
```
处理js和css的task写好了之后，我们需要在写出文件之前先删除之前的旧文件
```
gulp.task('clean', function (cb) {
  return gulp.src(output)
    .pipe(vinylPaths(del));
});
```
第一个pipe：vinylPaths获取需要删除文件的路径，并传入del

然后修复default的task
```
gulp.task('default', ['clean'], function () {
  gulp.start('sass', 'styles', 'scripts');
});
```
第二个参数为先执行的task名称，这里要注意一点，摘自gulp中文网
>注意： 你的任务是否在这些前置依赖的任务完成之前运行了？请一定要确保你所依赖的任务列表中的任务都使用了正确的异步执行方式：使用一个 callback，或者返回一个 promise 或 stream。

###注意事项
1. 在macos中，目录名的大小写是敏感的，如果不注意可能会导致watch功能的失效。
:)