[Dale Seo님 블로그](https://www.daleseo.com/)의 "[자바8 Optional 2부: null을 대하는 새로운 방법](https://www.daleseo.com/java8-optional-after/)"을 읽고 공부한 내용을 정리했습니다. 감사합니다.

## 자바8 Optional 2부: null을 대하는 새로운 방법

null 관련 문제는

- 런타임에 NPE를 발생시킬 수 있다.
- NPE 방어를 위해 들어간 null 체크 로직 때문에 가독성과 유지보수성이 떨어진다.

는 것으로 요약할 수 있다. 

스칼라나 히스켈과 같은 함수형 언어는 *존재할 지 안 할 지 모른느 값*을 다루는 별개의 타입을 가지고 있다. 그리고 이 타입은 이 **존재할 지 안 할지 모르는 값을 제어할 수 있는 여러가지 API를 제공**하기 때문에 개발자는 API를 사용해서 간접적으로 그 값에 접근한다. Java8은 함수형 언어의 접근 방식에서 영감을 받아 `java.util.Optional<T>`라는 클래스를 도입했다. 

### Optional

`Optional`은 **존재할 수도 있지만 안 할 수도 있는 객체**, 즉 **null이 될 수 있는 일종의 래퍼 클래스**이다. 원소가 없거나 하나밖에 없는 `Collection`이나 `Stream`으로 생각해도 된다. **직접 다루기에 위험하고 까다로운 null을 담을 수 있는 특수한 그릇**이라고 생각하자. Optional로 객체를 감싸 사용하면

- NPE를 유발할 수 있는 **null을 직접 다루지 않아도 된다.**
- 수고롭게 **null 체크를 직접 하지 않아도 된다.**
- **명시적으로 해당 변수가 `null`일 수도 있다는 가능성을 표현**할 수 있다. (따라서 불필요한 방어 로직을 줄일 수 있다.)

### Optional 사용법

**변수 선언하기**

```java
Optional<Order> maybeOrder;
Optional<Member> optMember;
Optional<Address> address;
```

**제네릭을 제공**하기 때문에, **변수를 선언할 때 명기한 타입 파라미터에 따라 감쌀 수 있는 객체의 타입이 결정**된다. 

변수명은 그냥 클래스 이름을 사용하기도 하고, maybe나 opt와 같은 접두어를 붙이기도 한다.

**Optional 객체 생성하기**

Optional 클래스는 **객체 생성**을 위해 3가지 **정적 팩토리 메서드**를 제공한다.

- `Optional.empty()`: null을 담고 있는, 빈 Optional 객체를 얻어온다. 이 빈 객체는 **Optional 내부적으로 미리 생성**해놓은 **싱글턴 인스턴스**이다.
- `Optional.of(value)`: `null`이 아닌 객체를 담고 있는 Optional 객체를 생성한다. `null`이 넘어올 경우 **NPE를 던지기 때문에 유의해서 사용해야 한다.**
- `Optional.ofNullable(value)`: null인지 아닌지 확신할 수 없는 객체를 담고 있는 Optional 객체를 생성한다. empty()와 of()를 합쳐놓은 메서드라고 생각하면 된다. **null이 넘어올 경우 NPE를 던지지 않고 `Optional.empty()`와 동일하게 비어있는 Optional 객체를 얻어온다.** 해당 객체가 null인지 아닌지 자신이 없는 상황에서 이 메서드를 사용한다. 

**Optional이 담고 있는 객체 접근하기**

- `get()`: 빈 `Optional` 객체에 대해서 `NoSuchElementException`을 던진다.
- `orElse(T other)`: 빈 `Optional` 객체에 대해서 넘어 온 인자를 반환한다.
- `orElseGet(Supplier<? extends T> other)`: 빈 Optional 객체에 대해서, 넘어온 함수형 인자를 통해 생성된 객체를 반환한다. 비어 있는 경우에만 함수가 호출되기 때문에 `orElse(T other)` 대비 성능상 이점을 기대할 수 있다.
- `orElseThrow(Supplier<? extends X> exceptionSupplier)`: 비어있는 Optional 객체에 대해 넘어온 함수형 인자를 통해 생성된 예외를 던진다. 

**Optional의 잘못된 사용**

`get()` 메서드는 빈 Optional 객체를 대상으로 예외를 발생시키므로 객체 존재 여부를 boolean 타입으로 반환하는 `isPresent()` 메서드를 통해 null 체크가 필요하다.

```java
String text = getText();
Optional<String> maybeText = Optional.ofNullable(text);
int length;
if (maybeText.isPresent()) {
	length = maybeText.get().length();
} else {
	length = 0;
}
```

`Optional`을 사용하는 이유는 **null 처리를 직접 하지 않고 Optional 클래스에게 위임하기 위함**이다.  다음과 같이 한 줄의 코드로 작성할 수 있어야 한다. **기존에 조건문으로 null을 대하던 생각을 함수형 사고로 새롭게 바꿔야 한다.**

```java
int length = Optional.ofNullable(getText()).map(String::length).orElse(0);
```

