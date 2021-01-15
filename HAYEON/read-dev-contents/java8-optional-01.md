[Dale Seo님 블로그](https://www.daleseo.com/)의 "[자바8 Optional 1부: 빠져나올 수 없는 null 처리의 늪](https://www.daleseo.com/java8-optional-before/)"을 읽고 공부한 내용을 정리했습니다. 감사합니다.

## 자바8 Optional 1부: 빠져나올 수 없는 null 처리의 늪

null 참조는 1965년 Tony Hoare가 "존재하지 않는 값"을 표현하는 편리한 방법으로 고안하였으나 나중에 null 참조를 만든 것을 후회한다고 토로했다. null 참조로 인해 컴파일 타임에는 잠복해있다가 런타임 때 펑펑 터지는 null pointer exception으로 자바 개발자들은 골머리를 겪어야 할 수밖에 없었다. 

null 처리가 취약한 코드에서는 NPE 발생 확률이 높다. 

```java
/* 주문을 한 회원이 살고 있는 도시를 반환한다 */
public String getCityOfMemberFromOrder(Order order) {
	return order.getMember().getAddress().getCity();
}
```

이 메서드는 NPE 위험에 노출된 상태인데, 다음과 같은 상황에서 NPE가 발생할 수 있다.

1. order 파라미터에 null값이 넘어옴
2. order.getMember()의 결과가 null 임
3. order.getMember().getAddress()의 결과가 null임
4. order.getMember().getAddress().getCity()의 결과가 null임

4번의 경우 메서드 내부에서 NPE가 발생하는 케이스는 아니지만 null을 리턴함으로써 호출부에 NPE 위험을 전파시키는 케이스이다.

### 전통적인 NPE 방어 패턴

**중첩 null 체크하기**

```java
public String getCityOfMemberFromOrder(Order order) {
	if (order != null) {
		Member member = order.getMember();
		if (member != null) {
			Address address = member.getAddress();
			if (address != null) {
				String city = address.getCity();
				if (city != null) {
					return city;
				}
			}
		}
	}
	return "Seoul"; // default
}
```

객체 탐색의 모든 단계마다 null이 반환되지 않을지 의심하면서 null 체크를 한다. 

**사방에서 return 하기**

```java
public String getCityOfMemberFromOrder(Order order) {
	if (order == null) {
		return "Seoul";
	}
	Member member = order.getMember();
	if (member == null) {
		return "Seoul";
	}
	Address address = member.getAddress();
	if (address == null) {
		return "Seoul";
	}
	String city = address.getCity();
	if (city == null) {
		return "Seoul";
	}
	return city;
}
```

두 방법 모두 초기 메서드보다 코드가 길고 지저분해졌다. 유지보수를 할 수록 비즈니스 로직은 null 체크에 가려질 것이다. 

자바 언어는 값의 부재를 나타내기 위해 null을 사용하도록 설계되었다. 하지만 null은 자바 개발자들에게 NPE 방어라는 끝나지 않는 숙제를 남겼다.

