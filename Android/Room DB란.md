# Room DB

## Room DB란?
- Room은 스마트폰 내장 DB에 데이터를 저장하기 위해 사용하는 `ORM(Object Relational Mapping)`라이브러리이다.
- Jectpack 라이브러리의 일부로 내부 저장소이며, ORM 라이브러리(DB데이터를 JAVA/Kotlin으로 변환) 이다
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

- Room은 LiveData와 RxJava를 위한 Observation으로 생성하여 동작할 수 있지만 SQLite는 그렇지 않다.
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

## 사용 예제
- Edittext 글 저장하기

### build.gradle
```Kotlin
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
    id 'kotlin-android'     // 추가
    id 'kotlin-kapt'        // 추가
}

dependencies {
    // Room DB
    def roomVersion = "2.4.3"
    implementation("androidx.room:room-runtime:$roomVersion")
    annotationProcessor("androidx.room:room-compiler:$roomVersion")
    kapt("androidx.room:room-compiler:$roomVersion")
    implementation("androidx.room:room-ktx:$roomVersion")
}
```
### DB Entity 
- Entity는 실체(객체)이다. 흔히 데이터베이스에서 개념 스키마를 뜻하며 쉽게 테이블이라고 생각하면 된다.
- data class를 정의하고 `@Entity` 어노테이션을 표시한다.
- Entity에는 하나 이상의 `기본키`를 설정해야하며, `@PrimaryKey`로 선언된 변수가 기본키이다.
- 기본키는 `복합키`로 이루어질 수 있다. 이럴 경우 어노테이션에서 설정해준다.
> @Entity(primaryKeys = arrayOf("firstName", "lastName"))
- 테이블 이름은 기본적으로 클래스 이름(예제에서는 Bookmark)가 된다. 만약 별도로 테이블 이름을 설정해주고 싶다면 어노테이션에서 설정 가능하다.
> Entity(tableName = "bookmark")
- 열로 사용할 변수 설정은 `@CcolumnInfo`로 가능하다. 기본적으로 변수 이름이 열 이름이 된다. 만약 별도로 열 이름을 설정하고 싶다면 name 속성을 주면 된다.
> @ColumnInfo(name ="category") val quoteCategory:String?

```Kotlin
@Entity
data class Bookmark(
    // PrimaryKey 를 자동적으로 생성
    @PrimaryKey(autoGenerate = true)
    val uid: Int?,
    @ColumnInfo(name ="quote")
    val quote:String?,
    @ColumnInfo(name ="quoteCategory")
    val quoteCategory:String?,
)
```

### DAO
- DAO는 `Data Access Object의 약자이다.
- DAO를 통해 쿼리문을 사용하여 데이터베이스의 데이터에 접근할 수 있다.
- Dao로 정의하기 위해선 `@Dao`라는 어노테이션이 필요하며, `인터페이스(interface)`로 작성된다.
- `@Query`를 통해 직접 SQL을 작성할 수 있다. 그 외에도 `@Insert` `@Delete` `@Update` 등으로 간편하게 구현할 수 있는 기능을 제공한다.
- 활용 방법이 많으니 자세한건 안드로이드 개발자 문서를 찾아는 걸 추천한다.
```Kotlin
@Dao
interface BookmarkDao {
    @Query("SELECT * FROM bookmark")
    fun getAll(): List<Bookmark>

    @Insert
    fun insertBookmark(bookmark: Bookmark)

    @Query("DELETE FROM bookmark")
    fun deleteAll()

    @Delete
    fun delete(bookmark: Bookmark)
}
```



#
```
참고
- https://velog.io/@limsaehyun/Android-Kotlin-Room-DB%EC%9D%98-%EC%82%AC%EC%9A%A9%EB%B2%95%EA%B3%BC-%EC%98%88%EC%A0%9C
- https://kong-droid.com/41
- https://latte-is-horse.tistory.com/155
```
