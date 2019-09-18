# JS FLOW - Day3

- this



### this

- 전역공간에서의 this
  `window` / `global`
- 함수내부에서의 this
  `window` / `global`
- 메소드 호출시 this
  a.b() 일때 메소드 . 바로앞에있는 것이 this
- callback에서의 this
- 생성자함수에서의 this



##### 함수내부에서의 this 

```javascript
function a(){
	console.log(this);
}
a();

//내부 함수일 때
function b(){
	function c(){
		console.log(this);
	}
	c();
}
b();
```

> 둘다 window / global 출력 :: 전역객체



##### 메소드 호출시 this

```javascript
var a = {
	b : function(){
    console.log(this);
  }
}

//메소드 호출
a.b();

/*
결과 : a객체가 this
[object Object] {
  b: function(){
    window.runnerWindow.proxyConsole.log(this);
  }
}
*/
```

```javascript
//객체 a의 b프로퍼티에 c메소드가 있는경우
var a = {
  b : {
    c : function(){
      console.log(this);
    }
  }
}

//메소드 호출
a.b.c();

/*
결과 : b객체가 this
[object Object] {
  c: function(){
      window.runnerWindow.proxyConsole.log(this);
    }
}
*/
```

> 메소드 바로앞에가 this



##### 내부함수에서의 this 우회법

```javascript
var a = 10;
var obj = {
	a : 20,
  b : function(){
    console.log(this.a); // 1번 
    
    function c(){
      console.log(this.a); // 2번
    }
    c();
  }
}
obj.b();

/*
결과
1번 : 20
2번 : 10

1번은 메소드의 바로앞에여서 obj가 this 이고,
2번은 내부함수 즉 함수자체 이기때문에 obj가아닌 var a = 10;
밖에 var a = 10; 없을경우 window / global 
*/
```

```javascript
//2번도 obj를 this로 쓰는 방법 :: 우회법
var a = 10;
var obj = {
	a : 20,
  b : function(){
    var self = this;
    console.log(this.a); // 1번 
    
    function c(){
      console.log(self.a); // 2번
    }
    c();
  }
}
obj.b();

```



##### callback에서의 this

```javascript
//명시적 this
function a(x,y,z){
	console.log(this,x,y,z);
}
var b = {
	c : 'eee'
};

a.call(b,1,2,3);

var c = a.bind(b);
c(1,2,3);

```

```javascript
//콜백함수에서의 this
var cb = function(){
	console.dir(this);
}

var obj = {
  a : 1
}

setTimeout(cb,1000); // 전역 window / global this출력
setTimeout(cb.bind(obj), 1000) // obj가 this
```

- 기본적으로 함수의 this와 같다.
- **제어권을 가진 함수가** callback의 this를 명시한 경우 그에 따른다.
- **개발자가** this를 바인딩한 채로 callback을 넘기면 그에 따른다.



##### 생성자함수에서의 this

```javascript
function Person(n,a){
	this.name = n;
	this.age = a;
}
var jueun = new Person('주은', 30);
console.log(jueun);
```

