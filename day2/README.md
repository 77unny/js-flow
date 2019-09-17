# JS FLOW - Day2

- 함수스코프, 실행컨텍스트
- 메소드
- 콜백함수

### 스코프, 실행컨텍스트

`스코프(SCOPE)` : 변수의 유효범위

- 함수가 정의될 때 생성

`실행 컨텍스트(Eexcution Context)` : 실행되는 코드덩어리

- 함수가 실행될 때 생성

- 호이스팅, this 바인딩등의 정보가 담긴다.
  함수가 실행될 때 사용자가 함수의 대한 정보들을 불러 모은 집합체

```javascript
var a = 1;
function outer(){
	console.log(a); //1번 값 1; 선언된 전역변수 a를 불러옴
	function inner(){
		console.log(a); //2번 값 undefined
		var a = 3;
	}
  inner();
  console.log(a); //3번 값 1;
}
outer();
console.log(a); //4번 값 1;

/*
	2번에서 undefined가 나온이유
	: outer()가 실행되면서 1번값이 출력되고 inner() 실행하면서
	var a;
	console.log(a);
	var a = 3;
*/
```

```javascript
// 위 내용 풀이
var a = 1;
function outer(){
	console.log('1번출력 : ' + a);
	function inner(){
		console.log('2번출력 : ' + a); //inner()함수를 호출하면서 var a;가 정의됨
		var a = 3;
        console.log('3번출력 : ' + a);
	}
  inner();
  console.log('4번출력 : ' + a);
}
outer();
console.log('5번출력 : ' + a);
```



### 메소드

> 함수처럼 생겼으나 앞에 .이 붙어있으면 메소드라고 보면된다.

- 메소드는 this를 바인딩한다.
  **obj.메소드 여기서 .메소드 앞에있는 obj가 this가 된다**

```javascript
var obj = {
	a: 1,
  b: function bb(){
    console.log(this);
  },
  c: function(){
    console.log(this.a);
  }
};

obj.b();
obj.c();

console.dir(obj.b);
console.dir(obj.c);
```

1. 전역 실행컨텍스트 생성(GLOBAL)
2. 변수 obj 선언
3. 객체 생성, 변수 obj 주소값 
4. obj.b() 메소드 호출 -> obj.b() 실행컨텍스트 생성
5. obj.b() 에서 this에 obj를 바인딩
   **obj.메소드   // 쩜메소드 앞에 obj가 this가 된다.**
6. this출력 (obj가 출력)
7. obj.b() 종료, obj.c() 반복
8. 전역 실행컨텍스트 종료



### 콜백함수

> call / back 으로 나누어서 보면 편하다.
>
> 무언가가 이함수를 나에게 다시 호출해서 언젠가,어떻게든 돌려줄거다.
>
> **즉, 제어건을 넘겨준다. 제어건을 맡긴다.**

`setInterval(callback, milliseconds)` : setInterval 함수는 일정시간마다 콜백함수를 부른다.

```javascript
setInterval(function({
  console.log('1초마다 실행될 겁니다.')
}), 1000);

//콜백함수를 변수로 치환
var cb = function(){
  console.log('1초마다 실행될 겁니다.');
};
setInterval(cb, 1000);

```

##### 콜백함수의 특징

- 다른 함수(a)의 매개변수로 콜백함수(b)를 전달하면, a가b의 **제어권**을 갖게됨
- a에 **미리 정해진 방식**에 따라 b를 호출
- 미리 정해진 방식이란
  **this**에 무엇을 바인딩할지,
  **매개변수**에는 어떤 값들을 지정할지
  어떤 **타이밍**에 콜백을 호출할지

> 주의! 콜백은 함수이다! 메소드가 아니다.