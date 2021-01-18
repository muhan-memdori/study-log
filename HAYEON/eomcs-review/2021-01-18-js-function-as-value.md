

## 값으로서의 함수

함수는 객체이기 때문에 주소를 주고 받을 수 있다. 

### 아규먼트로 함수를 전달하기

함수 객체를 아규먼트로 넘길 때 보통 파라미터의 이름을 **fn** 또는 **cb(callback)**로 한다.

```js
function play(cb) {
  console.log("계산 결과 =", cb(100, 200));
}

function plus(a, b) {return a + b;}
function minus(a, b) {return a - b;}

play(plus); // 계산 결과 =300
play(minus); // 계산 결과 =-100
```

### 함수 리턴하기

함수 안에서 함수를 만들어 리턴할 수 있다.

```js
function interestCalculator(type) {
  switch (type) {
    case "보통예금":
      return function(money, month) {return money + (money * 0.0011 * month);};
    case "정기예금":
      return function(money, month) {return money + (money * 0.0014 * month);};
    default:
      return function(money, month) {return money;};
  }
}
```

`interestCalculator()` 함수가 리턴하는 것은 **내부에서 정의한 함수의 주소**이다.

```js
var fn = interestCalculator("보통예금");
console.log("100억 7달 =", fn(10000000000, 7));
```



