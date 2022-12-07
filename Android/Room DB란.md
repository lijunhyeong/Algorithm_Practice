# Room DB

## Room DB란?
- Room은 스마트폰 내장 DB에 데이터를 저장하기 위해 사용하는 `ORM(Object Relational Mapping)`라이브러리이다.
- Jectpack 라이브러리의 일부로 내부 저장소이며, ORM 라이브러리(DB데이터를 JAVA/Kotlin으로 변환) 입니다
- Room은 SQLite의 추상레이어 위에 제공하고 있으며 SQLite의 모든 기능을 제공하면서 편한 데이터베이스의 접근을 허용한다.
- liveData나 RxJAVA와 같이 Observation 형태도 지원하므로 아키텍쳐 패턴에도 적용이 매우 쉽다.

## SharedPreferences와 차이
- SharedPreferences도 앱의 로컬에 데이터를 저장할 수 있지만, 가벼운 데이터를 저장할 목적으로 로컬 DB를 사용한다.
> 서버와 통신할 때 쓸 String형 access_token
- Room DB는 큰 사이즈의 데이터를 저장할 목적으로 로컬 DB를 사용하게 된다.
> 유저의 기본 정보 클래스

## Room 사용이 권장되는 이유
### SQLite의 단점
<img src="https://user-images.githubusercontent.com/72978589/206114703-286ba690-31c2-42c8-b7a0-9c46ce962007.png" width="80%" height="20%">    
- SQLite를 활용하며, 데이터 접근이 편리하고 유지보수가 용이하며, 마이그레이션도 간단하다.
- SQLite와 달리, 컴파일 타임에 쿼리의 적합성을 확인할 수 있다. SQLite보다 편리한 DB라고 생각하면 된다. 

## Room의 3가지 구성요소
<img src="https://user-images.githubusercontent.com/72978589/206115114-ac6679c3-f303-4ac8-89a4-288d16323474.png" width="60%" height="20%">    
### Entity
- DB에서 Table을 나타낸다.
- `@Entity`로 선언한다.

### Dao(Data Access Objects)
- 데이터에 Access 할 수 있는 Interface
- `@Dao`로 선언한다.

### Room DataBase
- abstract class로 선언하고 Room Database를 상속 받는다.
- `@Database`로 선언한다.
- DB에서 사용될 테이블의 정보(entity)가 필요하고, version 정보가 필요한다.
> @Database(entities = [UserInfo::class], version = 1)
- App이 업데이트 되어서 DB가 변경될 경우 이전 DB와 구분이 필요하므로 version에 `int`값을 넣어 관리한다.

```
참고
- https://velog.io/@limsaehyun/Android-Kotlin-Room-DB%EC%9D%98-%EC%82%AC%EC%9A%A9%EB%B2%95%EA%B3%BC-%EC%98%88%EC%A0%9C
- https://kong-droid.com/41
```
