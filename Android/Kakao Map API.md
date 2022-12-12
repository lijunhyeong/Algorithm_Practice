# Kakao Map API

## 📌 Kakao 개발자 사이트에서 앱 등록
- 로그인하고, 애플리케이션을 등록한다.
<img src="https://user-images.githubusercontent.com/72978589/206891546-eb498aa8-5a73-4482-9cd5-689561d7fdbd.png" width="60%" height="20%"> 

- '플랫폼' 탭으로 이동하여 내 프로젝트 `패키지명`과 `키 해시`를 등록한다.  
<img src="https://user-images.githubusercontent.com/72978589/206891662-e186384c-c043-4e1e-a798-cdf801f24344.png" width="50%" height="20%">     

### ✅ 키해시 얻는 법  
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
### ✅ 패키지명
- 패키지명은 `AndroidManifest.xml`파일 가장 상단에서 확인할 수 있다.
<img src="https://user-images.githubusercontent.com/72978589/206891861-6b883086-cf49-4535-993c-bfb9691ef1be.png" width="60%" height="20%">    

## 📌 라이브러리 추가
- Kakao map API 개발자 사이트에서 [SDK](https://apis.map.kakao.com/android/guide/#step1) 다운한다.
- `libs`폴더를 만들어 jar파일을 넣고, `jinLibs`폴더를 만들어 나머지 파일을 넣는다.
<img src="https://user-images.githubusercontent.com/72978589/206891984-69905441-dddc-4e7b-a989-852750c19d44.png" width="50%" height="20%">    

## 📌 Manifest 등록
- `android:usesCleartextTraffic="true`는 cleartext HTTP와 같은 cleartext 네트워크 트래픽을 사용할지 여부를 나타내는 flag로 이 플래그가 flase 로 되어 있으면, 플랫폼 구성 요소 (예 : HTTP 및 FTP 스택, DownloadManager, MediaPlayer)는 일반 텍스트 트래픽 사용에 대한 **앱의 요청을 거부**하게 됩니다. 이 flag를 설정하게 되면 모든 cleartext 트래픽은 허용처리가 됩니다.
### ✅ APP KEY 추가
- 카카오 개발자 페이지에서 `내 애플리케이션 선택`->`앱 설정`-> `앱- 키` -> `네이티브 앱 키`
- android:value에 네이티브 앱 키를 넣는다.
```xml
<meta-data android:name="com.kakao.sdk.AppKey" android:value="xxxxxxxxxxxxxxxxxxxxx" />
```
### ✅ Permission 추가
```xml
<!-- 인터넷 권한 -->
<uses-permission android:name="android.permission.INTERNET" />
<!--    정확한 위치    -->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<!--    대략적인 위치    -->
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```
<img src="https://user-images.githubusercontent.com/72978589/206892331-f5ef0d24-09c2-4578-86b8-98be6b78071f.png" width="60%" height="20%">    

## 📌 build.gradle:Module에 등록
```xml
//kakao map
implementation fileTree(include: ['*.jar'], dir: 'libs')
implementation files('libs/libDaumMapAndroid.jar')
```

## 📌 settings:gradle(Project Settings) 등록
```xml
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url 'https://naver.jfrog.io/artifactory/maven/' }
        
        maven{url 'https://devrepo.kakao.com/nexus/content/groups/public/' }  // 추가

    }
}
```

## 📌 activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <net.daum.mf.map.api.MapView
        android:id="@+id/mapView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintTop_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"/>
  
</androidx.constraintlayout.widget.ConstraintLayout>

```

## 📌 GPS 및 위치 권한 확인, 내위치 불러오기
```Kotlin
// 내 위치 불러오기
private fun getMyPosition(){
        if (checkLocationService()) {
            // GPS가 켜져있을 경우
            permissionCheck()
            binding.mapView.setZoomLevel(1, true)
        } else {
            // GPS가 꺼져있을 경우
            Toast.makeText(this, "GPS를 켜주세요", Toast.LENGTH_SHORT).show()
        }
}

// GPS가 켜져있는지 확인
private fun checkLocationService(): Boolean {
        val locationManager = getSystemService(Context.LOCATION_SERVICE) as LocationManager
        return locationManager.isProviderEnabled(LocationManager.GPS_PROVIDER)
}
// 위치 권한 확인
private fun permissionCheck() {
        val preference = getPreferences(MODE_PRIVATE)
        val isFirstCheck = preference.getBoolean("isFirstPermissionCheck", true)
        if (ContextCompat.checkSelfPermission(
                this,
                Manifest.permission.ACCESS_FINE_LOCATION
            ) != PackageManager.PERMISSION_GRANTED
        ) {
            // 권한이 없는 상태
            if (ActivityCompat.shouldShowRequestPermissionRationale(
                    this,
                    Manifest.permission.ACCESS_FINE_LOCATION
                )
            ) {
                // 권한 거절 (다시 한 번 물어봄)
                val builder = AlertDialog.Builder(this)
                builder.setMessage("현재 위치를 확인하시려면 위치 권한을 허용해주세요.")
                builder.setPositiveButton("확인") { _, _ ->
                    ActivityCompat.requestPermissions(
                        this,
                        arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),
                        ACCESS_FINE_LOCATION
                    )
                }
                builder.setNegativeButton("취소") { _, _ ->

                }
                builder.show()
            } else {
                if (isFirstCheck) {
                    // 최초 권한 요청
                    preference.edit().putBoolean("isFirstPermissionCheck", false).apply()
                    ActivityCompat.requestPermissions(
                        this,
                        arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),
                        ACCESS_FINE_LOCATION
                    )
                } else {
                    // 다시 묻지 않음 클릭 (앱 정보 화면으로 이동)
                    val builder = AlertDialog.Builder(this)
                    builder.setMessage("현재 위치를 확인하시려면 설정에서 위치 권한을 허용해주세요.")
                    builder.setPositiveButton("설정으로 이동") { _, _ ->
                        val intent = Intent(
                            Settings.ACTION_APPLICATION_DETAILS_SETTINGS,
                            Uri.parse("package:$packageName")
                        )
                        startActivity(intent)
                    }
                    builder.setNegativeButton("취소") { _, _ ->

                    }
                    builder.show()
                }
            }
        } else {
            // 권한이 있는 상태
            startTracking()
        }
}

// 위치추적 시작
private fun startTracking() {
        binding.mapView.currentLocationTrackingMode =
            MapView.CurrentLocationTrackingMode.TrackingModeOnWithoutHeading
}
```

## 📌 실행 화면
<img src="https://user-images.githubusercontent.com/72978589/206893006-77eaf999-c801-48a0-b119-24c46c0da5c6.jpg" width="30%" height="10%">  

#
```
참고
- Kakao Map API 가이드: https://apis.map.kakao.com/android/guide/#step
- https://charlie-dev.tistory.com/1
- https://w36495.tistory.com/61
- https://developside.tistory.com/85
```
