# 초보 개발자의 myWallets 백엔드 개발기

> 출처: https://velog.io/@croco_space/%EC%B4%88%EB%B3%B4-%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98-myWallets-%EB%B0%B1%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EA%B8%B0

### myWallets 서비스

Apple Wallet에 다양한 멤버십 카드를 추가할 수 있는 서비스로서, 멤버십 리스트에서 멤버십을 선택 후  패스를 추가하면 휴대폰의 Apple Wallet 앱에 등록되어 멤버십을 빠르게 사용할 수가 있다.

___

### 기술 스택

- NestJS
- TypeScript
- MySQL, TypeORM

> **NestJS?**
>
> - NodeJS 프레임워크 중 하나로, Angular에서 영감을 받았다.
> - Express를 사용하지만, Fastify와 같은 다양한 라이브러리와 호환성을 제공하므로 사용가능한 외부 플러그인을 쉽게 사용할 수가 있다.
> - 제공
>   - TypeScript
>   - 객체지향 프로그램(OPP)
>   - 기능적 프로그램(FP)
>   - FRP(Functional Reactive Programming)
>
> 출처: https://backback.tistory.com/383#:~:text=Angular%EC%97%90%EC%84%9C%20%EB%A7%8E%EC%9D%80%20%EC%98%81%EA%B0%90%EC%9D%84%20%EB%B0%9B%EC%95%98%EB%8B%A4%EA%B3%A0%20%ED%95%9C%EB%8B%A4.&text=Fastify%EC%99%80%20%EA%B0%99%EC%9D%80%20%EB%8B%A4%EC%96%91%ED%95%9C%20%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC,%EC%89%BD%EA%B2%8C%20%EC%82%AC%EC%9A%A9%20%ED%95%A0%20%EC%88%98%20%EC%9E%88%EB%8B%A4.&text=%EC%86%8D%EB%8F%84%EC%99%80%20%EC%98%A4%EB%B2%84%ED%97%A4%EB%93%9C%EB%A5%BC,js%20%EC%9B%B9%20%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%EC%9D%B4%EB%8B%A4.
>
> **TypeSecript?**
>
> - MS에서 개발한 오픈소스 프로그래밍 언어로 브라우저나 호스트, 운영체제에서 동작한다.
> - 아비스크립트는 자바스크립트의 상위 집합으로서,  ECMA의 최신 표준을 충분히 지원한다.
> -  ES7이하의 표준을 포함하며, ES5을 포함하는 집합이므로 기존의 ES5 자바스크립트 문법을 그대로 사용할 수 있다.
> - 정적 타입 언어로 컴파일 시 시간이 조금이 걸리지만 안정성을 보장한다. (자바스크립트는 동적 타입언어로, 런타임 속도는 빠르지만 타입 안정성이 보장되지 않는다.)
>
> 출처: https://medium.com/@wonjong_oh/typescript-1-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-f4b02f54009c
>
> **ORM(Object Relational Mapping)?**
>
> - 객체 - 관계 매핑을 뜻하는 단어로 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑해주는 것을 말한다. 즉, 클래스 형태의 객체와 테이블 형태의 데이터베이스 간의 불일치를 객체간의 관계를 바탕으로 SQL을 자동으로 생성하여 해결한다.
>
> - **데이터베이스 데이터 <--매핑--> Object 필드**
>
> - Persistant API라고도 할 수 있다. (ex- JPA, Hibernate 등)
>
> 출처: https://gmlwjd9405.github.io/2019/02/01/orm.html

___

### myWallets 서비스 구조에 대해

패스 생성을 위해 필요한 데이터는 다음과 같이 분류될 수 있다.

- **Membership**
  브랜드 정보: 로고, 색상, URL
- **PassTemplate**
  패스 생성을 위해 필요한 공통.정보: 패스 타입, 아이템 이미지
- **Pass**
  패스 고유 정보: 바코드, 바코드 타입, 패스 이름, 만료 기한

또한 애플 월렛에서 지원하는 패스 타입은 보딩 패스, 쿠폰, 이벤트 티켓, 매장 카드, 제네릭으로 총 5개의 타입이 존재하는데, 이에 따라 PassTemplate과 Pass는 타입에 따라 다른 필드를 가지는 객체로 구성되어야했다. 

이를 위해서는 테이블 상속 전략이 필요했는데, TypleORM에서 지원하는 상속 전략에는 다음 두가지가 있다.![image](https://media.vlpt.us/images/croco_space/post/676236f7-335f-4c6d-af31-63f80e4ecdce/image.png)

- **싱글 테이블 상속 전략**
  싱글 테이블 전략은 모든 자식 엔티티 컬럼을 한 테이블에 저장 후, 구분자를 통해 어떤 자식 테이블에 어떤 엔티티가 저장되어있는지 구분한다. 테이블이 커질 수 있지만, 쿼리 속도는 빠르다는 장점이 있다.

- **구체 테이블(멀티 테이블) 상속 전략**
  자식 엔티티를 위한 모든 속성을 가지는 테이블을 각각 생성한다. 여러 종류의 엔티티를 쿼리할때, 여러 테이블에 접근해야하므로 성능이 떨어지는 단점이 있다.

테이블이 너무 많아질 가능성에 대비해 해당 프로젝트에서는 싱글 테이블 전략을 사용하기로 했다.

```mysql
import { ChildEntity, Column } from 'typeorm';
import { PassTemplate, PassTemplateType } from './pass-template.entity';

@ChildEntity(PassTemplateType.Coupon)
export class CouponPassTemplate extends PassTemplate {
  @Column()
  itemName!: string;

  @Column()
  itemImage!: string;
}

import { ChildEntity } from 'typeorm';
import { PassTemplate, PassTemplateType } from './pass-template.entity';

@ChildEntity(PassTemplateType.StoreCard)
export class StoreCardPassTemplate extends PassTemplate {}
```

___

### 협업 방식

**GitLab Flow**에 따라 협업을 진행한다.

- 기능 단위별 브랜치를 열고 작업 후 PR을 생성한다.
- 모든 코드는 리뷰를 받고 develop 브랜치로 머지된다.
- 일정한 간격을 두고 Frontend와 함께 릴리즈하고 master(production)에 최종적으로 배포한다.

**모든 코드는 리뷰를 받아야한다**는 원칙은 새로운 언어와 프레임워크에 적응하는데 정말 많은 도움이 되었다. 

___

### 앞으로의 계획

- 각 모듈을 DDD를 도입해서 리팩토링을 하고 있다.

- 현재는 모듈 의존성이 없는 멤버십 모듈로부터 작업을 하고 있으나 앞으로는 Pass 생성을 포함한 모든 모듈을 Domain Driven으로 개발하는 것이 목표이다.

> DDD(Domain-Driven Design)?
>
> DDD에서는 어플리케이션 도메인을 표현하기 위한 오브젝트 모델을 만든다. 도메인의 복잡성을 관리하기 위해 이 모델은 도메인의 모든 관계와 로직을 담는다. 

