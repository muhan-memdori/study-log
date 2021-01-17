
## 함수의 파라미터와 아규먼트

자바스크립트 함수를 호출할 때 아규먼트의 개수는 **함수에 정의된 파라미터 개수와 일치하지 않아도 된다**.

- 아규먼트(argument): 함수를 호출할 때 전달하는 값
- 파라미터(parameter): 함수를 호출할 때 전달한 값을 보관하는 함수 로컬 변수

## 함수 오버로딩?

같은 이름의 변수를 여러 개 선언하는 것은 문법 오류는 아니다. 단지 기존 변수를 가리킨다. 마찬가지로 같은 이름의 함수를 여러 개 선언하는 것은 문법 오류가 아니다. 단지 기존 함수를 대체한다. 따라서 자바스크립트는 **메서드 오버로딩**을 지원하지 않는다.

## 함수 아규먼트와 내장 변수 argument

자바스크립트의 함수는 **함수를 호출할 때 전달한 값들을 보관할 배열 변수를 내장**하고 있다. 그 내장 변수의 이름은 `arguments`이다.

```js
function f1(a) { 
	console.log("a =", a);
	console.log("----------------");
	
  for (var value of arguments) {
    console.log(value);
  }
  console.log("----------------");
  for (var i in arguments) {
    console.log(arguments[i]);
  }
  console.log("----------------");
  for (var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}

//f1();
//f1(100);
f1(100, 200, 300, 400, 500);
```

- 자바스크립트는 **파라미터의 개수와 상관없이 아규먼트를 넘길 수 있기 때문에 자바의 오버로딩 문법이 자바스크립트에서는 없다.**
- 자바는 **메서드를 찾을 때 값의 타입과 개수를 이용하여 찾는다.** 그러나 자바스크립트는 **값의 타입이 없고 아규먼트의 개수도 의미가 없다.** 그래서 자바스크립트는 **같은 이름을 가진 함수를 여러 개 만들 수 없다.**

`Array()`로 만든 오리지널 배열은 `forEach()` 메서드가 있다. 그러나 `arguments`는 `Array()`로 만든 오리지널 배열이 아니다. 따라서 `forEach()` 함수가 없다. `forEach()`를 사용하려면 진짜 배열로 바꾼 후 사용해야 한다. 

```js
function f1(a) { 
	console.log("a =", a);

	/*
  arr.forEach(function(value) { // 실행 오류!
    console.log(value);
  });
  */
  
  console.log("----------------");
  // forEach()를 사용하고 싶다면 진짜 배열로 바꾼후 사용해야 한다.
  var arr = Array.from(arguments);
  arr.forEach(function(value) {
    console.log(value);
  });
  
//f1();
//f1(100);
f1(100, 200, 300, 400, 500);
```

한편 `Array()`로만든 배열에는 `reduce()` 함수가 있다. 합계 등을 계산할 때 유용하다.

```js
function f1(a) { 
 	arr.reduce(function(sum, value) {
    console.log(sum, value)
    return sum + value;
  });

//f1();
//f1(100);
f1(100, 200, 300, 400, 500);
```
