# JS FLOW - Day4

- 클로저 (closure) 



### 클로저 (closure) : 닫혀있음, 폐쇄성, 완결성

- 스코프와 밀접한관계,  스코프(유효범위) 클로저(그유휴범위로 인햔 현상,상태)
- 함수 내부에서 생성한 데이터와 그 유효범위로 인해 발생되는 특수한 현상 / 상태



##### 접근 권한 제어 / 지역변수 보호

```javascript
function a(){
  var x = 1;
  function b(){
    console.log(x);
  }
  b();
}

a();

console.log(x);
```

```
function a(){
  var x = 1;
  return function b(){
    console.log(x)
  }
}
var c = a();
c();
```

