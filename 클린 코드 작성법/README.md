# 클린 코드(Clean Code)란 무엇일까

## 🔍 정의  
- 원하는 로직을 빠르게 찾을 수 있는 코드, 모든 팀원이 이해하기 쉽도록 작성된 코드

## 🚀 장점
- 유지보수 시간의 단축
    - 깔끔한 코드는 코드 리뷰, 코드 파악과 디버깅 시간을 단축시킨다.
    - 만약 클린 코드를 실천하지 않고 복사, 붙여넣기와 같은 방법을 택한다면 *Technical dept가 생김
        - *Technical dept이란 기술 부채 의미, 현 시점에서 더 나은 접근 방식보다 더 쉬운 솔루션을 채택함으로써 발생되는 추가적인 재작업의 비용


## 📌 클린 코드의 주요 원칙
1. Follow Standard Conventions  
   - 코딩 표준, 아키텍처 표준 및 설계 가이드를 준수해야 한다.  
2. Keep it simple, Stupid  
   - 단순한 것이 효율적이며, 복잡함을 최소화해야 한다.  
3. Boy Scout Rule   
   - 참조되거나 수정되는 코드는 원래보다 클린해야 한다. 자신이 담당한 코드는 담당하기 이전의 코드보다 더 클린하게 만들어야한다.  
4. Root Cause Analysis 
   - 항상 근본적인 원인을 찾아야한다. 그렇지 않으면 반복될 것이다.
5. Do Not Multiple Languages in One Source File
   - 하나의 파일은 하나의 언어로 작성한다.


## 💯 Tip
1. 검색 가능한 이름을 사용하기
   - ex) 하루가 총 몇 초인지 나타낼 때
```kotlin  
// 잘못된 예
setSeconds(86400)

// 좋은 예
val SECONDS_IN_A_DAY = 86400
setSeconds(SECONDS_IN_A_DAY)
```  
2. 함수명은 반드시 동사로, 동작은 하나만
   - 이 규칙은 심플하지만 `함수가 너무 많은 역할을 하는 것은 아닌지` 알게된다. 
   - 액션 중심으로 이름을 짓다보면 구분의 필요성을 느끼게 된다.
   - **함수는 무조건 단 한가지 액션만 수행해야 한다.**
   - ex) `유저데이터`는 함수명으로 좋은 이름이 아니다. `유저 데이터 불로오기`가 좋은 이름이다.
```kotlin  
// 잘못된 예
fun userData(){
    // ...
}
val data = userData()

// 좋은 예
fun loadUserData(){
    // ...
}
val userData = loadUserData()
``` 
3. 인수(argument)
   - 함수의 인수는 3개 이하가 가장 좋다.
   - 많은 경우에는 Object로 정리해서 param 사용한다.
```kotlin  
// 잘못된 예
fun makePayment(price:Int, productId:Int, size:String, quantity:Int, userId: String){
    // process payment
}
makePayment(1000, 7, "L", 1, "준")
``` 
4. `Boolean`값을 인수로 함수에 보내는 것을 최대한 방지
   - `boolean`값은 참 혹은 거짓을 의미하는 데이터 타입이다. `boolean`값을 함수에 보낸다는 것은 그 함수 안에 `if else`가 있다는 뜻이다. 차라리 각각의 `if else`값을 다른 함수로 분리하는 것이 좋다.
   - **함수는 무조건 단 한가지 액션만 수행해야 한다.**
```kotlin  
// 잘못된 예
fun sendMessage(text, isPrivate){
    if(isPrivate) // send private message
    else // send public message
}
sendMessage("hello", false)
sendMessage("this is a secret", true)

// 좋은 예
fun sendPrivateMessage(text){
    // ...
}
fun sendPublicMessage(text){
    // ...
}
sendPrivateMessage("hello")
sendPublicMessage("this is a secret")
``` 
5. 짧은 변수명이나 (아무도 이해 못하는) 축약어를 쓰지 말 것
   - 다른 사람이 봤을 때 알아볼 수 있도록(협업에서 중요)
```kotlin  
// 잘못된 예
allUsers.forEach{ u ->
   sendEmail(u)
}

// 좋은 예
allUsers.forEach{ user ->
   sendEmail(user)
}
``` 
6. 초기 시작점부터 코드를 이쁘게 쓰려고 애쓰지 말 것
   - 일단 코드를 쓰고나서, 작동이 되는 것을 확인하고, 그 다음 클린 코드로 다듬는다.
     - 처음부터 이쁜, 클린 코드를 쓰려면 너무 어려움
