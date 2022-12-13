## 📌 Error
```
android.os.NetworkOnMainThreadException
```
### ✅ NetworkOnMainThreadException 이란?
- 안드로이드 애플리케이션이 main thread 에서 네트워킹 처리를 시도할 경우 발생하는 오류이다.
- 안드로이드 HoneyComb(Android3.0, API Level 11)이상 버전부터 Main Thread에서 네트워킹 프로세스를 금지하고 있다.

## 📌 해석
- 메인 Thread 에서 발생하는 에러로 네트워크를 이용해 데이터를 받기 위해서는 별개의 Thread 가 필요하다. 그렇지 않으면 Stream 객체를 통해 데이터를 읽어 오는 과정에서 android.os.NetworkOnMainThreadException 에러가 발생한다.

## 📌 상황
- Naver AI Service CLOVA OCR를 통해 카메라로 사진을 찍으면 사진 속 글자를 추출하는 기능을 구현하고 있었다.
- CLOVA OCR 가이드 문서대로 이미지를 API에 넘기면 `android.os.NetworkOnMainThreadException` Error가 발생했다.

## 📌 해결
- 사용자가 사용하는 User interface(버튼을 누르는 등의 동작)와 별개의 프로세스(thread)에서 network를 사용해야한다. 즉, Thread를 따로 만들어 Network(Http등의 동작) API를 쓰도록하면 해결된다.
```Kotlin
Thread {
    companyName = ocrGeneralAPI(uri)
}.start()
```

#
```
참고
- https://jamesdreaming.tistory.com/35
- https://pickersoft.net/entry/androidosNetworkOnMainThreadException-%EA%B0%80%EC%9E%A5-%EC%89%BD%EA%B2%8C-%ED%95%B4%EA%B2%B0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95
```
