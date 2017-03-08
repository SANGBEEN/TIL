#node.js 기본 모듈

#전역 객체
##process
애플리케이션 프로세스 실행 정보
  - env : 애플리케이션 실행 환경
  - arch,platform : CPU와 플랫폼 정보
  - argv : 실행 명령 파라미터
  - exit : 종료 이벤트
  - beforeExit : 종료 되기 전에 발생하는 이벤트
  - uncaugthException : 예외 처리되지 않은 예외 이벤트
```
$ node processAdd.js 3 5
//0, 1은 node, processAdd.js
process.argv[2];//3
process.argv[3];//5
```

##타이머
 - 지연 동작 : setTimeout
 - 반복 동작 : setInterval

##콘솔
  1. 로그 남기기
    - console.log()
    - console.info()
    - console.warn()
    - console.error()
    - 객체를 출력할 시에는 +가 아닌 콤마를 사용한다.
  2. 파일로 로그 남기는 커스텀 콘솔
    - var output = fs.createWriteStream('./stdout.log');
    - var errorOutput = fs.createWriteStream('./stderr.log');
    - var logger = new Console(output, errorOutput);
  3. 실행 시간 측정    - 시작 시점 설정 :  console.time(TIMER_NAME)
    - 종료 시점 : console.timeEnd(TIMER_NAME)
```
console.time('SUM');
var sum = 0;
for(var I = 1;i<10000;i++){
  sum+=i;
}
console.timeEnd('SUM');
```
##유틸리티
```
var util = require('util');
```
  - 문자열 포맷 : util.format(format,[, ...])
  - 상속 : util.inherits(constructor, superConstructor)
```
function Parent(){
}
Parent.prototype.sayHello = function(){
  console.log('hello. from parent class');
}
var obj = new Parent();
obj.sayHello();

function Child(){
}
util.ingerits(Child, Parent)
var obj2 = new Child();
obj2.sayHello();
```

##이벤트

이벤트 : 클라이언트의 접속 요청, 소켓에 데이터 도착, 파일 오픈/읽기 완료....
이벤트 처리 : 비동기 처리, 리스너 함수

이벤트 리스너 함수 등록
  - emitter.addListener(event, listener) 
  - emitter.on(event, listener) : 이벤트가 발생할 때마다 발생. 주로 사용
  - emitter.once(evnet, listener) : 이벤트가 발생했을 때 단 한번만 발생
```
process.on('exit', function(){
  console.log('occur exit event');
});
```
이벤트 리스너 함수 삭제
  - emitter.removeListener(evnet, listener)
  - emitter.removeAllListeners([event])
이벤트 발생
  - emitter.emit(event[,arg1][,arg2][,```])
  - event(이벤트 이름), arg(리스너 함수의 파라미너)
  - emit 함수 호출 결과 : true, fasle

커스텀 이벤트
```
var customEvent = new event.EventEmitter();
customEvent.on('tick', function(){
  console.log('occur custom event');
});

customEvnet.emit('tick');
```
커스텀 이벤트, 상속
```
var Person function(){};
//상속
var util = require('util');
var EventEmitter = require('events').EvnetEmitter;
util.ingerits(Person, EventEmitter);
//객체
var p = new Person();
p.on('howareyou', function(){
  console.log('Fine, thany you');
});
//이벤트 발생
p.emit('howareyou');
```
##경로 - path 모듈
경로 정규화, 경로 생성, 디렉토리/파일 이름 추출, 파일 확장자 추출

모듈추가
```
var path = require('path');
```

경로 정보
  - 전역 객체 : __filename, __dirname
  - var path = __dirname+'./image.jpg';
경로 다듬기 : path.normalize()
경로 구성 요소 얻기
```
var path = './foo/bar/baz/asdf/quux.html';
pathUtil.dirname(path) // /foo/bar/baz/asdf
pathUtil.basename(path) // quux.html
pathUtil.extname(path) // .html
```
객체
```
var info = path.parse('/home/user/dir/file.txt')
{
  root:"/",
  dir:"/home/user/dir",
  base:"file.txt",
  ext:".txt",
  name:"file"
}
//구성 요소 얻기
info.base
info.name
```
경로 만들기
  - path.seq : 연산
  - path.join : 붙이기
  - path.format 

##파일 시스템 - fs 모듈
파일 생성/읽기/쓰기/삭제, 파일 접근성/속성, 디렉토리 생성/읽기/삭제, 파일 스트림

모듈 추가
```
var fs = require('fs')
```

비동기와 동기 방식 함수 모두 제공
  - 비동기식 : callback
  - 동기식 : +Sync(readFileSync), 블록방식, 반환값 이용

파일 다루기
  - 디스크립터 : fs.read(fd, buffer, offset, length, position, callback)
  - 경로 : fs.readFile(filename[,options], callback)

파일 디스크립터
``` 
//FileDescription 얻기 : open함수
//flag r(읽기),w(쓰기),a(추가)...
fs.open(path, flags[,mode], function(err,fd){
});
//파일닫기
fs.close(fd.callback);
```
파일 읽기
  - fs.read
  - fs.readFile
  - fs.readFileSync
```
fs.readFile(file, 'UTF-8', function(err,data){
 if(err){
  console.error('File Read Error:', err);
  return;
}
console.log('Read Text File, UTF-8 Encoding');
console.log(data);
});
```
파일 접근 상태 확인
  - fs.access(path[, mode], callback)
  - fs.F_OK : 존재 확인
  - fs.R_OK, W_OK, X_OK : 읽기/쓰기/실행 여부 확인
파일에 저장
  - fs.writeFile(filename,data[,options], callback)
```
fs.writeFile('./textData.txt', 'helloworld', function(err){
  if(err){
    console.error('파일 저장 실패:‘,err);
    return;
  }
  console.log('파일 저장 성공‘);
});
```
파일에 추가 : fs.appendFile(file, data[, options], callback), 없으면 생성
파일 삭제 : fs.unlink(path, callback), 파일이 없으면 에러

파일 스트림
  - fs.createReadStream(paht[,options])
  - fs.createWriteStream(path[,options])

##버퍼
자바스크립트는 바이너리 데이터를 다루는 기능이 없으므로 Buffer 모듈을 사용함
  - 파일에서 읽기 : var fileBuffer = fs.readFilesync('image.jpg');
  - 네트워크에서 얻기 
