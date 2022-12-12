# REST API

## RESTful API 란?
- RESTful API는 두 컴퓨터 시스템이 인터넷을 통해 정보를 안전하게 교환하기 위해 사용하는 인터페이스이다.
- 대부분의 비즈니스 애플리케이션은 다양한 태스크를 수행하기 위해 다른 내부 애플리케이션 및 서드 파티 애플리케이션과 통신해야 한다.
- 예를 들어, 월간 급여 명세서를 생성하려면 인보이스 발행을 자동화하고 내부의 근무 시간 기록 애플리케이션과 통신하기 위해 내부 계정 시스템이 데이터를 고객의 뱅킹 시스템과 공유해야 한다.
- `RESTful API`는 안전하고 신뢰할 수 있으며 효율적인 소프트웨어 통신 표준을 따르므로 이러한 정보 교환을 지원한다.
- RESTFUL이란 REST의 원리를 따르는 시스템을 의미한다.
- 

## REST란?
- REST는 API 작동 방식에 대한 조건을 부과하는 소프트웨어 아키텍처이다.
- REST는 처음에 인터넷과 같은 복잡한 네트워크에서 통신을 관리하기 위한 지침으로 만들어졌다.
- `REST(Representational State Transfer)`의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다.
<img src="https://user-images.githubusercontent.com/72978589/206942082-8a0b6ec3-1c14-4b37-ac14-fcb7440dd276.png" width="80%" height="20%">      

### 즉 REST란
- HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
  - `URI`란, 은행계좌는 계좌번호, 버스는 노선번호, 사람은 주민등록번호로 구분되듯 웹 서버의 리소스 또한 각자의 이름이 있다. (클라이언트가 요청할 때 찾아야하니까, id같은 고유한 식별값이 있어야겠지!) 이때 서버 리소스 이름(식별자)을 uniform resource identifier(통합 자원 식별자), URI라고 부른다. 
- HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
- 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.

### REST 구성 요소
- 자원(Resource) : HTTP URI
- 자원에 대한 행위(Verb) : HTTP Method
- 자원에 대한 행위의 내용(Representations) : HTTP Message Pay Load

## REST의 특징
### 온디맨드 코드
- REST 아키텍처 스타일에서 서버는 소프트웨어 프로그래밍 코드를 클라이언트에 전송하여 클라이언트 기능을 일시적으로 확장하거나 사용자 지정할 수 있다.
-  예를 들어, 웹 사이트에서 등록 양식을 작성하면 브라우저는 잘못된 전화번호와 같은 실수를 즉시 강조 표시한다. 서버에서 전송한 코드로 인해 이 작업을 수행할 수 있다.

### Stateless(무상태)
- REST 아키텍처에서 무상태는 서버가 이전의 모든 요청과 독립적으로 모든 클라이언트 요청을 완료하는 통신 방법을 나타낸다.
- 클라이언트는 임의의 순서로 리소스를 요청할 수 있으며 모든 요청은 무상태이거나 다른 요청과 분리된다.
- 이 REST API 설계 제약 조건은 서버가 매번 요청을 완전히 이해해서 이행할 수 있음을 의미한다.
  
### Cacheable(캐시 처리 가능)
- RESTful 웹 서비스는 서버 응답 시간을 개선하기 위해 클라이언트 또는 중개자에 일부 응답을 저장하는 프로세스인 캐싱을 지원한다.
- 예를 들어, 모든 페이지에 공통 머리글 및 바닥글 이미지가 있는 웹 사이트를 방문한다고 가정해 보면, 새로운 웹 사이트 페이지를 방문할 때마다 서버는 동일한 이미지를 다시 전송해야 한다. 이를 피하기 위해 클라이언트는 첫 번째 응답 후에 해당 이미지를 캐싱하거나 저장한 다음 캐시에서 직접 이미지를 사용한다.
- RESTful 웹 서비스는 캐시 가능 또는 캐시 불가능으로 정의되는 API 응답을 사용하여 캐싱을 제어한다.
  
### Layered System(계층화)
- 계층화된 시스템 아키텍처에서 클라이언트는 클라이언트와 서버 사이의 다른 승인된 중개자에게 연결할 수 있으며 여전히 서버로부터도 응답을 받는다.
- 서버는 요청을 다른 서버로 전달할 수도 있다.
- 클라이언트 요청을 이행하기 위해 함께 작동하는 보안, 애플리케이션 및 비즈니스 로직과 같은 여러 계층으로 여러 서버에서 실행되도록 RESTful 웹 서비스를 설계할 수 있다.
- 이러한 계층은 클라이언트에 보이지 않는 상태로 유지된다.
 
### Uniform Interface(인터페이스 일관성)
- 균일한 인터페이스는 모든 RESTful 웹 서비스 디자인의 기본이다.
- 이는 서버가 표준 형식으로 정보를 전송함을 나타낸다.
- 형식이 지정된 리소스를 REST에서 표현이라고 부른다.
- 이 형식은 서버 애플리케이션에 있는 리소스의 내부 표현과 다를 수 있다. 예를 들어, 서버는 데이터를 텍스트로 저장하되, HTML 표현 형식으로 전송할 수 있습니다.
- 균일한 인터페이스에는 4가지 아키텍처 제약 조건
  - 요청은 리소스를 식별해야 한다. 이를 위해 균일한 리소스 식별자를 사용한다.
  - 클라이언트는 원하는 경우 리소스를 수정하거나 삭제하기에 충분한 정보를 리소스 표현에서 가지고 있다. 서버는 리소스를 자세히 설명하는 메타데이터를 전송하여 이 조건을 충족한다.
  - 클라이언트는 표현을 추가로 처리하는 방법에 대한 정보를 수신한다. 이를 위해 서버는 클라이언트가 리소스를 적절하게 사용할 수 있는 방법에 대한 메타데이터가 포함된 명확한 메시지를 전송한다.
  - 클라이언트는 작업을 완료하는 데 필요한 다른 모든 관련 리소스에 대한 정보를 수신한다. 이를 위해 서버는 클라이언트가 더 많은 리소스를 동적으로 검색할 수 있도록 표현에 하이퍼링크를 넣어 전송한다.

## REST API 설계 예시
- URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.
```
Bad Example https://github.com/Studying  
Good Example  https://github.com/Study    
```

- 마지막에 슬래시 (/)를 포함하지 않는다.
```
Bad Example https://github.com/test/  
Good Example  https://github.com/test  
```

- 언더바 대신 하이폰을 사용한다.
```
Bad Example https://github.com/test_test  
Good Example  https://github.com/test-test  
```

- 파일확장자는 URI에 포함하지 않는다.
```
Bad Example https://github.com/photo.jpg   
Good Example  https://github.com/photo  
```

- 행위를 포함하지 않는다.
```
Bad Example https://github.com/delete-post/1  
Good Example  https://github.com/post/1  
```

#
```
참고
- https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80
- https://velog.io/@syoung125/%EA%B0%9C%EB%85%90%EA%B3%B5%EB%B6%80-URI%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-URL%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C
- https://aws.amazon.com/ko/what-is/restful-api/
```
