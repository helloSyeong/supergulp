### SUPER GULP

[INTRODUCE]

# 0.3

yarn, npm : package manager
gulp : task runner

- 전역 NPM packagedp Gulp 설치해줘야해
  -> 콘솔에서 gulp를 쓸거거든
  -> 전역에서! -g : globally
  npm install gulp-cli -g

gulp-cli : helping call 'gulp' from console

(니코는 git init 먼저 함)

띄어쓰기로 새로운 폴더/파일 생성 가능

- lenux commander
  mkdir : create new folder
  touch (folder/)file.file : create new (folder and) file
  mv <old> <new> : change the file name <old> to <new>

src : 컴파일되기를 원하는 모든 파일들이 들어갈 폴더 (처리되기 전 소스폴더)
dest : 처리된 파일들이 들어갈 폴더
src/scss src/js src/img src/partials src/templates
js/util.js js/main.js : will be written modermn javascript
scss/style.scss scss/\_variable.scss scss/\_reset.scss
partials/header.pug partials/footer.pug
templates/layout.pug

구조짜기 Scaffolding

package.js > "scripts" 에서 gulp dev, gulp build 와 같은 명령어를 사용해서 하고 싶다면 gulp 설치 필요해. 개발자용인거니까 (-D로 npm install gulp -D), -D 깜빡했다면, package.json에서 위치를 옮겨줘도 되긴함(?)

"scripts" : {
"dev": "gulp dev",
"build": "gulp build"
}

// 근데 yarn dev 하면 gulp dev 를 yarn 이 실행시키는거 같은데
npm은 못함

# 0.4

old javascript : const gulp = require("gulp");
modern javascript : import gulp from "gulp";

# 0.5 gulp + babel

mv gulpfile.js gulpfile.babel.js : change the file name from 'gulpfile' to 'gulpfile.babel'
// babel 이용해서 최신 자바스크립트를 컴파일할거야

- babel 모듈 설치할때 최신 버전을 사용해 앞에 "@"마크 붙여서 설치

npm install @babel/{register,core,preset-env} == yarn add @babel/{register,core,preset-env}
: {중괄호와 쉼표로 여러개 한번에 설치 가능}

[PUG]

# 1.0 Build

Gulp : 작은 task들을 만들어서 그걸 하나의 명령어나 여러개의 명령어로 묶어서 사용!

- 하나의 패턴으로 모든 파일이랑 매치시켜야해! (니코는 routes라는 거 만들어서 사용해)
  gulp는 다양한 object를 가짐, 아마도 == api? : \*src, \*dest, series, parallel, watch, task, tree.... (<https://gulpjs.com/docs/en/api/concepts>)
  how to make task?? : function export 하거나 const 하면 됨
  what is the task?? : 예를 들어 scss->css변환, img 최적화, javascrtip minifying 등등
  이 작은 tasks들을 모아서 하나의 명령어나 여러개의 명령어로 묶어서 사용 예정! 이게 걸프!

src -> pipe ( -> pipe -> pipe ... ) -> dest
pipe : 뭔가의 흐름중에 하나. 코드를 복사하거나, 최소화하거나, 뭔가를 처리하는 작은 단위 흐름
series : task들의 series <- 이 시리즈에서 실행되는 task에서 할일들을 pipe를 통해 실행 하는 것

- task1 : pug files to html via gulp-pug plugin (npm install gulp-pug -D)

task는 const가 될 수도, function이 될수도 있어. 종종 뭔가를 return 해야 할 때가 있어

gulp.src : 파일들의 위치?를 찾는거.
src를 찾아야해. 하나의 패턴으로 모든 파일이랑 매치시켜야하는데. ???
그래서 그 위치들을 모아둔 routes object를 만들어두면 좋아
const routes = { ...: "..." }

tip: src/_.pug : src 폴더 안에 있는 pug파일만 선택, src/\*\*/_.pug 하위 폴더 안에 있는 pug파일들도 다 선택

# 1.1 Clean Build and Build again

npm install del
del(["지울폴더명"])

export const dev = gulp.series([clean, pug]);
: 여기서 clean은 build를 위한 준비과정, pug는 실제로 파일을 변형시키는 작업

export는 package.json 에서 사용할 것들에만 해주면 됨. (그래서 현 소스에 dev를 제외하고 export 제거됨)

[SERVER]

# 2.0

npm install gulp-webserver -D

webserver : gulp.src 찾아("웹서버로 보고싶은 폴더명")

# 2.1

watch! 지켜볼 파일들, 옵션들을 인자로 받아서 function 실행해

안에 있는 task들을 실행시키는 건 같으나
series : 순서대로
parallel : 동시에 task들 병행함

[IMAGE]

# 3.0

npm install gulp-image
image는 watch 할지 말지는 신중하게 생각해, 용량 큰것들을 많이 가지고 있다면 오래 걸려서 힘들거야
