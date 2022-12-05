### 📌 Error
```
android.view.ViewRootImpl$CalledFromWrongThreadException:
Only the original thread that created a view hierarchy can touch its views.
```

### 📌 해석
Original Thread으로만 UI(view heirarchy: 화면 체계)를 변경시킬 수 있다.

### 📌 상황
- Firebase API를 통해 데이터를 받아서 화면에 뿌려주려고 한다.
- Firebase API는 비동기 이기 때문에 화면이 호출된 후에 데이터를 가져오기 때문에 
- 로딩 뷰를 그려주고, `Timer(타이머)`를 통해 1초마다 데이터를 넣을 배열을 확인하여 비어있지 않은 경우 로딩 뷰를 숨긴다.
- `timerTask`를 사용할 경우 `Thread`를 생성하여 timer를 작동시킨다. 이 `Thread`는 `UI`를 변경할 수 없다.
<img src="https://user-images.githubusercontent.com/72978589/205696081-39cef412-eb83-4ff8-8a31-8e5881290444.png" width="80%" height="20%">  

## 📌 해결 
- 때문에 `UI`에 대한 변경은 `main 스레드`에서 담당하게 하기 위해서, 생성된 새로운 `Thread`에서는 작업을 끝내고그 작업에 대해 `UI 변경`이 필요할 
경우 `handler`를 이용했다. 즉 `main 스레드`로 `Runnable`에 수행할 것을 담아 보내 `main`에서 처리하게 한 것이다. 이 일련에 과정은 매우 많이 쓰이기 때문에 안드로이드에서 자체적으로 구현해 놨다.
### ✅ `runOnUiThread`으로 해결
- `UI` 변경은 `main 스레드`가 담당해야 한다.  
- `runOnUiThread`는 현재 스레드가 `UI 스레드`인지 확인하고 맞으면 실행, 틀리면 핸들러를 통해 UI 스레드의 이벤트 큐로 post한다.


- 참고
```
- https://devfarming.tistory.com/3
- https://gema.tistory.com/418
- https://devforyou.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C%EC%BD%94%ED%8B%80%EB%A6%B0-%EA%B3%84%EC%82%B0%EA%B8%B0-%EB%A7%8C%EB%93%A4%EA%B8%B03-runOnUiThread-%EC%93%B0%EB%A0%88%EB%93%9C%EC%97%90%EC%84%9C-UI
```
