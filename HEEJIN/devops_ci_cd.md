# CI/CD란 무엇인가 (Feat.DevOps 엔지니어)

## CI (Continuous Integration)

CI는 지속적인 통합이라는 의미로 어플리케이션의 새로운 코드 변경 사항이 정기적으로 빌드 및 테스트 되어 공유 레포지토리에 통합되는 것을 의미한다.

### CI가 필요한 환경

- 다수의 개발자가 형상관리 툴을 공유하여 사용하는 환경
  Git이나 SVN과 같은 형상관리 툴을 사용하고 있다면 지속적으로 서비스해야 하는 어플리케이션이나 현재 개발 중인 어플리케이션은 기능을 추가할 때마다 commit을 날려 레포지토리에 버전 업데이틀 해야 한다. 다수의 개발자가 공유 레포지토리에 commit을 할 때마다 기능별로 빌드, 테스트, 병합(merge)까지 하려면 상당히 번거로울 것이므로, 자동화된 빌드&테스트가 필요하다.
- MSA(Micro Service Architecture) 환경
  MSA는 요즘 떠오르고 있는 아키텍쳐 모델로, 기존의 어플리케이션이 모든 기능을 포함하는 하나의 거대한 서비스였다면, MSA는 작은 기능별로 서비스를 잘게 쪼개어 개발하는 형태를 말한다. 이러한 형태에 따라 MSA 환경에서는 대부분 Agile 방법론이 적용되므로, 기능 추가가 매우 빈번하게 발생하게 되는데, 자잘한 기능들의 충돌을 방지하기 위해 CI의 적용이 필요하다.

### 이러한 환경 속에서 CI의 핵심 목표는

- 버그를 신속하게 찾아 해결
- 소프트웨어의 품질 개선
- 업데이트의 검증 및 릴리즈의 시간을 단축

하는 것에 있다.

___

## CD (Continuous Delivery & Continuous Deployment)

CD는 지속적인 서비스 제공 혹은 지속적인 배포를 의미한다. 지속적인 제공(Delivery)는 공유 레포지토리로 자동으로 Release하는 것을 말하며 지속적인 배포(Deployment)는 Production 레벨까지 deply하는 것을 의미한다. 즉, CD는 개발자의 변경사항이 레포지토리를 넘어, 고객의 프로덕션(Production) 환경까지 릴리즈되는 것을 의미한다.

CI에서 예로 든 MSA와 같은 환경에서 Agile 방법론이 적용될 경우, 서비스의 사용자는 최대한 빠른 시간 내에 최신 버전의 Production을 제공받을 필요가 있다. CD는 서비스의 개발팀과 비즈니스팀(영업, CS팀 등)간의 커뮤니케이션 부족 문제를 해결해줌으로써 배포에 이르기까지의 노력을 최소한으로 단축시켜준다.

___

## DevOps 엔지니어의 역할

CI/CD의 자동화는 DevOps 엔지니어의 핵심 업무에 속한다.

![?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3WY5f%2FbtqI5zz0OUH%2FN5KhjwQ3SP9nYplyZrXuVK%2Fimg](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3WY5f%2FbtqI5zz0OUH%2FN5KhjwQ3SP9nYplyZrXuVK%2Fimg.png)

### DevOps 엔지니어는

- CI/CD를 위한 파이프라인을 구성하여 자동화한다.
- 모니터링 지표를 구성하여 개발자들의 개발 방향을 가이드한다.

이 두 가지 업무를 통해서 안정적이고 신뢰성 높은 서비스 프로덕션을 제공한다. 

DevOps 엔지니어가 사용하는 대표적인 CI/CD 툴로는 Jenkins/ Travis CI/ Bamboo 등이 있다.