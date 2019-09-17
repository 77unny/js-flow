# JS FLOW - Day1

- 데이터 타입 : 기본형과 참조형 종류 및 차이점
- 호이스팅
- 함수선언문과 함수표현식



### 데이터 타입 : 기본형 vs 참조형 

##### 기본형 

> Number / String / Boolean / null / undefined :: 값을 그대로 할당

변수는 주소를 매칭시키고, 그주소로 이동해서 데이터를 넣는다.

```javascript
var a;
a = 10;
/*
	변수명 : a;
	변수의 주소 : 예) @1
	@1의 초기 데이터는 undefined
	이후 @1의 데이터는 10을 할당 받음
*/

var b = 'abc';
/*
	변수명 : b;
	변수의 주소 : 예) @2; (@1는 a변수가 자리잡음)
	@2의 데이터 'abc';
*/

b = false;
/*
	변수명 : b;
	변수의 주소 : 예) @2; (@2가 b 이기때문에 그대로 씀)
	@2의 데이터는 'abc' 였으나, false로 재정의됨
*/

var c = b;
/*
	변수명 : c;
	변수의 주소 : 예) @3; 
	@3의 데이터는 @2의데이터(변수명b)의 false값을 씀
*/

c = 10;
/*
	변수명 : c;
	변수의 주소 : 예) @3;
	@3의 데이터는 false였으나 10으로 재정의 됨
	@2의 데이터는 변하지 않음, 초기 설정 했을때 값만 가져와서 쓴 부분이기 때문
*/

/*
	[데이터 정리]
	a : @1 = 10;
	b : @2 = false;
	c : @3 = 10;
*/

```



##### 참조형

> Object (Array / function / RegExp) :: 값이 저장된 주소값을 할당

기본형과 비슷한 개념이지만 참조형은 키와 값이 이루어져있음

```javascript
var obj = {
	a : 1,
	b : 'b'
}
/*
	변수명 : obj;
	변수의 주소 : @1
	@1의 데이터에 @1-1 
	@1-1의 데이터에 {a:@1-2,b:@1-3}
	@1-2의 데이터에 1
	@1-3의 데이터에 'b'
*/

var obj2 = obj;
/*
	변수명 : obj2;
	변수명의 주소 : @2
	@2의 데이터는 obj와 동일하게 씀
	@2의 데이터에 @1-1 
	@1-1의 데이터에 {a:@1-2,b:@1-3}
	@1-2의 데이터에 1
	@1-3의 데이터에 'b'
*/

//obj2.a의 값을 변경
obj2.a = 10;
/*
	obj2의 @1-2의 데이터의 1이 10으로 재정의됨
	obj와 obj2의 데이터는 같은 데이터임
*/
console.log(obj.a) // 10;
console.log(obj2.a) // 10;

```



### 호이스팅

```javascript
console.log(a());
console.log(b());
console.log(c());

function a(){
	return 'a';
}

var b = function(){
	return 'b';
}

var c = function(){
	return 'c';
}

/*
	2번째 줄부터 에러가 난다. b의 함수가 없기때문에 위코드의 순서는
	
	1번 순서
	function a(){
		return 'a';
	}
	가 선언 된 이후
	
	2번 순서
	var b; 변수 정의 (변수지만 함수는 없음)
	
	3번 순서 
	var c; 변수 정의 (변수지만 함순는 없음)
	
	4번 콘솔 실행
	console.log(a());
  console.log(b()); // 변수 b는 있지만 함수는 없으므로 에러, 종료
  console.log(c()); // 변수 c는 있지만 함수는 없으므로 에러
  
  에러가 나서 이미 종료지만 굳이 실행 한다면,
  b = function(){
  	return 'b';
  }
  c = function(){
  	return 'c';
  }
*/

/*
	실행이 가능한 순서로 변경
	
  function a(){
    return 'a';
  }

  var b = function(){
    return 'b';
  }

  var c = function(){
    return 'c';
  }
  
  // 선언된 이후 콘솔 실행
  console.log(a());
  console.log(b());
  console.log(c());
*/
```



### 함수선언문 vs 함수표현식?

##### 함수 선언문

> 같은 함수가 위 아래에 있을경우 함수 호이스팅이 발생하여, 아래있는 함수로 덮어씀

```javascript
// 출력 : a + b = 값
function sum(a,b){
	return a + ' + ' + b + ' = ' + (a + b);
}
sum(1,2); // 출력 3;
/*
	중략
	:중간에 많은 함수로 인해서 누군가가 작성한 함수를 모르고 같은 함수를 썼을경우
*/

// 출력 : 값
function sum(a,b){
	return a + b;
}
sum(3,4); // 출력 7

/*
	처음 함수에서는 a + b = 값이 나와야하는데 함수호이스팅으로 인해 아래함수가
	호이스팅이되어서 출력값이 7이 나온다.
*/
```

##### 함수표현식

> 함수 호이스팅이 발생하지않음, 안전하고 예측가능한 소스가됨

```javascript
var sum = function(a,b){
	return a + ' + ' + b ' = ' + (a + b);
}
sum(1,2); //출력 a + b = 값 

var sum = function(a,b){
	return a + b;
}
sum(3,4); //출력 값

```

함수선언문과 표현식은 클로저에서 달리 나오게된다.

**결론 : 함수 선언식은 함수 호이스팅이 발생, 함수 표현식은 호이스팅이 발생하지 않는다.**