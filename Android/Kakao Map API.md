# Kakao Map API

## Kakao 개발자 사이트에서 앱 등록
- 로그인하고, 애플리케이션을 등록한다.
<img src="https://user-images.githubusercontent.com/72978589/206891546-eb498aa8-5a73-4482-9cd5-689561d7fdbd.png" width="60%" height="20%"> 

- '플랫폼' 탭으로 이동하여 내 프로젝트 `패키지명`과 `키 해시`를 등록한다.  
<img src="https://user-images.githubusercontent.com/72978589/206891662-e186384c-c043-4e1e-a798-cdf801f24344.png" width="60%" height="20%">     

### 키해시 얻는 법  
- 아래 메서드를 통해 확인할 수 있다.  
```Kotlin
private fun getHashKey() {
        var packageInfo: PackageInfo? = null
        try {
            packageInfo = packageManager.getPackageInfo(packageName, PackageManager.GET_SIGNATURES)
        } catch (e: PackageManager.NameNotFoundException) {
            e.printStackTrace()
        }
        if (packageInfo == null) Log.e("KeyHash", "KeyHash:null")
        for (signature in packageInfo!!.signatures) {
            try {
                val md: MessageDigest = MessageDigest.getInstance("SHA")
                md.update(signature.toByteArray())
                Log.d("KeyHash", Base64.encodeToString(md.digest(), Base64.DEFAULT))
            } catch (e: NoSuchAlgorithmException) {
                Log.e("KeyHash", "Unable to get MessageDigest. signature=$signature", e)
            }
        }
    }
```
### 패키지명
- 패키지명은 `AndroidManifest.xml`파일 가장 상단에서 확인할 수 있다.
<img src="https://user-images.githubusercontent.com/72978589/206891861-6b883086-cf49-4535-993c-bfb9691ef1be.png" width="60%" height="20%">    

## 라이브러리 추가
- Kakao map API 개발자 사이트에서 [SDK](https://apis.map.kakao.com/android/guide/#step1) 다운한다.
- `libs`폴더를 만들어 jar파일을 넣고, `jinLibs`폴더를 만들어 나머지 파일을 넣는다.
<img src="https://user-images.githubusercontent.com/72978589/206891984-69905441-dddc-4e7b-a989-852750c19d44.png" width="60%" height="20%">    

## Manifest 등록
- `android:usesCleartextTraffic="true`는 cleartext HTTP와 같은 cleartext 네트워크 트래픽을 사용할지 여부를 나타내는 flag로 이 플래그가 flase 로 되어 있으면, 플랫폼 구성 요소 (예 : HTTP 및 FTP 스택, DownloadManager, MediaPlayer)는 일반 텍스트 트래픽 사용에 대한 **앱의 요청을 거부**하게 됩니다. 이 flag를 설정하게 되면 모든 cleartext 트래픽은 허용처리가 됩니다.
- APP KEY 추가
```xml
<meta-data android:name="com.kakao.sdk.AppKey" android:value="xxxxxxxxxxxxxxxxxxxxx" />
```
- Permission 추가
```xml
<!-- 인터넷 권한 -->
<uses-permission android:name="android.permission.INTERNET" />
<!--    정확한 위치    -->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<!--    대략적인 위치    -->
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```


#
```
참고
- https://apis.map.kakao.com/android/guide/#step1
- https://charlie-dev.tistory.com/1
- https://w36495.tistory.com/61
- https://developside.tistory.com/85
```
