# myWallets 프론트엔드 이야기

> 출처: https://velog.io/@croco_space/mywallets-frontend-story

## 사용 기술

### 구성

- Next.js (React)
- Emotion (@emotion/react, @emotion/styled)
- TypeScript

이번 프로젝트는, Next.js 10 이 출시되고 얼마되지 않아 시작했으므로 React 17을 도입하고, next/image 컴포넌트를 사용한 첫 프로젝트이다.

상태 관리는 모두 React의 기본 Context로 하고 있으며 라이브러리를 사용하지 않은 이유는 Hook의 도입으로 Context를 직관적으로 관리할 수 있었으며 관리해야할 변수가 많지 않았기 때문이다.

> **React**
>
> 리액트는 페이스북에서 제공해주는 컴포넌트 기반 프론트엔드 라이브러리이다. 컴포넌트라고 불리는 작은 요소들을 쌓아올려 하나의 UI를 만드는 방식으로 설계된다. 컴포넌트로 쪼개져 있으므로 코드를 파악하기 용이하며, 다른 화면에서도 사용될 수 있는 재사용성을 갖고 있다. 컴포넌트의 종류는 클래스형(stateful)과 함수형(stateless)로 나뉜다. 
> 출처: https://velog.io/@stampid/React%EB%9E%80

> **NextJS**
>
> 리액트 프로젝트에서 SSR(Server Side Rendering)을 하기 위해 사용된다. SSR은 View가 어떻게 보여질지를 서버에서 늘 해석해서 보내주는 전통적인 방식의 렌더링을 말한다. 하나의 빈 페이지(Single Page)를 서버측이 클라이언트에 보내주고, 클라이언트를 데이터를 추후에 받아 빈 페이지에 적용시켜 가공하는 방식의 CSR(Client Side Redering) 렌더링 방식과 대조된다. 일반적인 리액트 싱글페이지에서는 초기 렌더링 떄 모든 컴포넌트를 내려받으므로 로딩 속도가 지연될 수 있다는 단점이 있다. Next는 이러한 문제점을 개선하여 필요에 따라 파일을 불러올 수 있게 여러개의 파일을 분리하는 코드 스플리팅을 사용했다.
> 출처: https://medium.com/@msj9121/next-js-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90-8727f76614c9
> https://velog.io/@rjs1197/NextJS-%EC%9E%85%EB%AC%B8%ED%95%98%EA%B8%B0

> **emtionJS**
>
> CSS-in-JS(JavaScript를 사용하여 선언적이고 분쟁 없이 재사용 가능한 방식으로 스타일을 해주는 CSS) 중 하나로, CSS를 JavaScript 안에서 작성하게 해준다.
> 출처: https://howdy-mj.me/css/emotion.js-intro/

### 배포 & CI

> **CI(Continuous Integration) 지속적 통합**
>
> 지속적으로 퀄리티 컨트롤을 적용하는 프로세스를 실행한다. 즉, 애플리케이션에 대한 새로운 코드 변경 사항이 정기적으로 빌드 및 테스트되어 하나의 레포지토리로 관리되는 것을 의미한다.
>
> 출처: https://minz.dev/18

- Production, Preview(beta) 모두 Vercel을 사용하고 있다.

프롣덕션에서도 Vercel을 유지하는 이유 중 하나는 ICN(서울)리전의 존재때문이다. 국내 모바일 사용자들이 타겟이니 만큼 로딩 속도가 중요하다. 하지만 Vercel도 트래픽 제한이 존재하기 때문에 혹여나 발생하는 트래픽 초과 상황을 방지하기 위해 모니터링 중이다.

### 디자인

- Figma 사용하여 기초 베이스를 잡았다. 

Figma를 선호하는 이유는 커뮤니티가 커서 여러가지 목업이나 디자인 리소스를 얻을 수 있고 다른 팀원과의 협업이 쉽고 간편하기 때문이다.

___

## 성능

이번 프로젝트에서는 성능보다 구현을 우선으로 하여, 모든 기능을 제작한 후 성능에 대한 최적화나 리팩토링을 진행하기로 하여서 기본 기능 구현이 끝나 현재도 계속 성능 최적화를 진행하고 있다.

### Memoization

함수 재사용의 중요성을 알게 된 이후로, Your Guide to React.useCallback()이라는 글을 참고하여, 중요하거나 반복 사용되는 함수, 계산이 많은 함수들에 한하여  useCallback (혹은 use Memo)를 사용하고 있다. 

> **Memoiaztion**
>
> 메모이제이션이란 프로그래밍을 할 때 반복되는 결과를 메모리에 저장해서 다음에 같은 결과가 나올 때 빨리 실행하는 코딩 기법을 말한다.
>
> 출처: https://www.zerocho.com/category/JavaScript/post/579248728241b6f43951af19

> **useCallback / useMemo**
>
> React에서 사용되는 Hook으로 useMemo는 특정 결과값을 재사용할때 사용하는 반면, useCallback은 특정 함수를 새로 만들지 않고 재사용하고자 할 때 사용한다.
>
> 출처: https://react.vlpt.us/basic/18-useCallback.html

### 불필요한 리렌더링

Virtual DOM 개념을 이용하는 React에게는 리렌더링이라는 과정이 존재한다. 불필요한 렌더링을 방지하기 위해, 변경되는 element를 일일히 확인하거나, React Devtools에 있는 Components에서 확인하고, useEffect를 이용해 콘솔에 로그를 찍으며 렌더링이 이뤄지는 시점을 매번 확인했다.

____

## 다크모드

디자인 초반부터 다크모드를 기획했었다.

### 구현

라이트 모드를 모두 스타일링한 후에 서비스 도중에 다크 모드를 추가했으므로 효율적인 방법을 고안했다. 구현 초반에는 각 요소에 CSS properties를 지정하려했으나 범위가 너무 넓어 Context(전역 상태) + CSS 방법을 선택했다. 

> Context(전역상태)
>
> 리액트에서 제공하는 기술인 Context API를 통해서 전역상태를 관리할 수가 있다. (리액트를 전혀 몰라서 참고해도 이해할 수 없었으므로 이런게 있다고만 알고 넘어가겠습니다.)
>
> 출처: https://velog.io/@yjs3819/Context-API%EB%A1%9C-%EC%A0%84%EC%97%AD%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0

- ContextProvider 컴포넌트가 로딩될 때에 다크모드인지 감지한다.
- ContextProvider가 전역을 감싸는 div를 만든다.
- 그 후, 요소를 구성하는 syled-components에서 .theme-dark를 부모객체로 받아서 스타일링한다.

이렇게 고안한 이유는 스타일링마다 Context를 받는 과정이 번거롭고, CSS Properties를 만들기도 귀찮았기에 스타일링이 조금늘어나더라도 이런 방법을 생가한 것이다. 결과적으로는 리렌더링의 부담도 없고 잘 작동했다. 다만 다크모드 감지가 유동적이진 않으며 일부 운영체제에서 자동 색상 모드를 지원하지 못했다.

___

## 구조

이번 프로젝트는 프론트엔드에서 아키텍쳐를 처음으로 적용해보았다. MVVM(Model-View-ViewModel) 패턴을 도입했다.

> MVVM
>
> 리액트에서 사용할 수 있는 아키텍쳐 중 하나이다. MVVM은 4개의 블록으로 이뤄져있다.
>
> - View - 사용자가 상호작용할 수 있는 UI 층
> - ViewController - ViewModel에 접근하며 사용자의 입력을 다룬다.
> - ViewModel - Model에 접근하며 비즈니스로직을 다룬다.
> - Model - 애플리케이션에서 사용되는 데이터
>
> ![1*bK0LyLEHirleo1xcPDUq-w](https://miro.medium.com/max/700/1*bK0LyLEHirleo1xcPDUq-w.png)
>
> 출처: https://medium.cobeisfresh.com/level-up-your-react-architecture-with-mvvm-a471979e3f21