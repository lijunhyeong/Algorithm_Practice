# 안드로이드 액티비티(Activity) 생명주기(Life Cycle)

## 🔍 생명주기(Life Cycle) 정의    
- 안드로이드 앱이 실행된 후 다른 액티비티 화면으로 전환되거나, 스마트폰 화면이 꺼지거나 혹은 앱이 종료될 때와 같이 상태 변화가 있을 때마다 화면에 보이는 액티비티의 생명 주기 메서드를 호출해서 상태 변화를 알려준다.
- `Activity` `Fragment` `Service` 총 세가지 종료의 Life Cycle이 있으며 여긴 Acitvity에 대해 작성한다.

<img src="https://user-images.githubusercontent.com/72978589/204096775-959ea8bd-927d-4094-9480-cec492e8d20d.png" width="60%" height="20%">      

|메서드 이름|액티비티 상태|설명|  
|:---|:---|:---|  
| **onCreate** | `만들어짐` | 액티비티 생성할 때 |  
| **onStart** | `화면에 나타남` | 화면에 보여지기 시작할 때 |  
| **onResume** | `현재 실행 중` `화면에 나타남` | 화면에 나타나 있고 실행중일 때 |  
| **onPause** | `화면이 가려짐` | 액티비티 화면의 일부가 다른 액티비티에 가려짐 |  
| **onStop** | `화면이 없어짐` | 다른 액티비티의 실행으로 완전히 가려짐 |  
| **onDestory** | `종료됨` | 액티비티 종료됨 |  

### 1) onCreate  
- Activity가 생성되면 가장 먼저 호출된다.
- 화면 Layout 정의, View 생성, Databinding 등은 이곳에서 구현한다.
- 생명주기 통틀어서 단 한 번만 수행되는 메소드이다.
- Acitivy 최초 실행에 해야되는 작업을 수행하기에 적합하다.

### 2) onStart  
- Activity가 화면에 표시되기 직전에 호출된다.
- 화면에 진입할 때마다 실행되어야 하는 작업을 이곳에 구현한다.

### 3) onResume  
- Activity가 화면에 보여지는 직후에 호출된다.
- 현재 Acitvity가 사용자에게 포커스인 되어있는 상태이다.

### 4) onPause  
- Activity가 화면에 보여지지 않은 직후에 호출된다.
- 현재 Activity가 사용자에게 포커스 아웃 되었있는 상태이다.
- 다른 Acitivity가 호출되기 전에 실행되기 때문에 무거운 작업을 수행하지 않도록 주의해야 한다.
- 영구적인 Data는 이곳에 저장한다.

### 5) onStop  
- Activity가 다른 Acitivy에 의해 100% 가려질 때 호출되는 메소드이다.(홈 키를 누르는 경우, 다른 액티비티로 이동이 있는 경우)
- 이 상태에서 Activity가 호출되면, `onRestart()` 메소드가 호출된다.

### 6) onDestory  
- Activity가 완전히 종료되었을 때 호출되는 메소드이다.
- 사용자: `finish()` `onBackPressed()` (기존 액티비티의 `onResume()`까지 호출된 후 `onDestroy()` 호출)
- `onStop()` `onDestory()` 메소드는 메모리 부족이 발생하면 스킵될 수 있다.

### 7) onRestart()
- `onStop()`이 호출된 이후에 다시 기존 Activity로 돌아오는 경우에 호출되는 메소드이다.
- `onRestart()`가 호출된 이후 이어서 `onStart()`가 호출된다.



## 📌 생명 주기 호출  
- 액티비티는 인스턴스 생성과 동시에 생성 관련 생명 주기 메서드가 순차적으로 호출된다.
- 액티비티를 종료하면 소멸과 관련된 생명 주기 메서드가 순차적으로 호출된다.

### ✅ 액티비티 생성  
<img src="https://user-images.githubusercontent.com/72978589/204097743-14b828f9-e9b4-4a0b-a38e-fc9f3c30ac26.png" width="20%" height="10%">  

- `onCreate()` -> 생성된 화면 구성요소를 메모리에 로드  
- `onStart()` `onResume()` -> 화면의 구성요소를 나타내고 사용자와 상호작용 시작(Resumed: 실행중)  

### ✅ 액티비티 화면에서 제거  
<img src="https://user-images.githubusercontent.com/72978589/204097849-0923986e-98dd-4ad7-990b-7d752f25c053.png" width="20%" height="10%">  

- `onPause()` `onStop()` -> 뒤로 가기, finish()를 실행할 때 동시에 실행  
- `onDestory()` -> 최종적으로 액티비티가 메모리에서 제거  


### ✅ 액티비티를 종료하지 않고 다른 액티비티 실행  
<img src="https://user-images.githubusercontent.com/72978589/204097934-18bdd691-410b-4ed5-9701-a08900f251e0.png" width="40%" height="30%">  

- `onPause()` `onStop()` -> 현재 액티비티를 종료하지 않고 새로운 액티비티가 만들어질 때(Stopped)  
- `onStart()` `onResume()` -> 두 메서드가 연속적으로 실행되고 Resumed 상태로 변경    

### ✅ 액티비티를 종료하지 않거나, 모두 가려지지 않을 때 다른 액티비티 실행  
<img src="https://user-images.githubusercontent.com/72978589/204098023-ba4c2257-eabc-414b-a51f-d527abc4d9a1.png" width="30%" height="20%">  

- `onPause()` -> 완전히 사라진 것은 아니므로 Paused 상태로 변경  
- `onResume()` -> 정지가 아니니 `onStart()`를 거치지 않고 바로 OnResume로 Resumed   



#### 생명주기 예제) https://velog.io/@its-mingyu/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-Activity-Lifecycle 
