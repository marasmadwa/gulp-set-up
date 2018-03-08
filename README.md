# gulp-set-up

My gulp set up which includes scss file with basic grid set up.

## Getting Started

**What is gulp?**

Gulp is a toolkit that helps you automate painful or time-consuming tasks in your development workflow.

How to install gulp ? Check this website : (https://gulpjs.com/);

my gulp set up:

```JavaScript
const gulp = require("gulp");
const sass = require('gulp-sass');
const plumber = require('gulp-plumber');
const autoprefixer = require('gulp-autoprefixer');
const sourcemaps = require('gulp-sourcemaps');
const browserSync = require('browser-sync').create();
const gutil = require('gulp-util');
const showErrors = function (err) {
    console.log(gutil.colors.red(err.toString()));
    this.emit('end');
};

gulp.task("browseSync", function () {
    browserSync.init({
        server: ".",
        notify: true,
        //host: "192.168.0.24", //IPv4 Address Wirless LAN adapter WiFi from ipconfig
        //port: 3000,
        open: true 
    });
});

gulp.task('sass', function () {
    return gulp.src('./scss/main.scss')
        .pipe(plumber({
            errorHandler: showErrors
        }))
        .pipe(sourcemaps.init())
        .pipe(sass({
            outputStyle: "compressed"
        }))//skompiluj
        .pipe(autoprefixer({ 
            browsers: ['last 2 versions'],
            cascade: false
        }))
        .pipe(sourcemaps.write('.'))
        .pipe(gulp.dest('./css'))
        .pipe(browserSync.stream());
});

gulp.task('watch', function () {
    gulp.watch('./scss/**/*.scss', ['sass']);
    gulp.watch("*.html").on('change', browserSync.reload);
});

gulp.task('default', function () {
    gulp.start(["sass", "browseSync", "watch"]);
});```

```
 **Grid**
 
 my grid.scss file
 ```JavaScript
 $gap: 10px;//gap between columns
 $columns: 12;// number of columns
 $breakpoint: 768px; 
 
.container {
  padding-left: $gap;
  padding-right: $gap;
  .row {
    &:after {
      content: '';
      display: block;
      clear: both;
      margin-left: -$gap;
      margin-right: -$gap;
    }
    
    @for $i from 1 through $columns {
      .col-#{$i}-#{$columns} {
        box-sizing: border-box;
        padding-left: $gap;
        padding-right: $gap;
        margin-bottom: 2*$gap;
        
        @media (min-width: $breakpoint) {
          width: (100/$columns*$i)*1%;
          float: left;
        }
      }
    }
  }
}```
```
License: https://gulpjs.com/
