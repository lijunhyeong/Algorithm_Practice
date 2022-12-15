# 시간 복잡도

## 알고리즘(algorithm) 이란
- 어떠한 문제를 해결하기 위해 정해진 일련의 절차나 방법; 문제를 해결하는 방법
- 한 문제를 해결하는 방법은 한 가지만 있는 게 아니라 무수히 많을 수 있다.
- 자주 쓰이는 문제 해결 방법(알고리즘)은 패턴화: BFS, DFS, DP, 다익스트라 등등
- 각 상황에 적합한 알고리즘을 선택할 수 있어야 한다.

## 알고리즘 평가 기준
### 시간 복잡도
- 적은 시간인 알고리즘일수록 좋다. 동일한 코드로 작성되어도 실행 환경에 따라서 매번 실행시간이 달라질 수 있다. 하지만 시간의 경향성은 계산할 수 있다. 
앞으로 배워 볼 시간복자도(Big-O)을 통해서 알고리즘마다 실행시간 경향성을 살펴야한다.

### 공간 복잡도 (메모리)
- 메모리는 한정적이기 때문에 최대한 적은 용량의 메모리를 사용하는 것이 좋다. 하지만 컴퓨터의 메모리 용량이 커짐에 따라서 메모리를 크게 신경쓰지 않고 
알고리즘을 작성하는 경우도 있다. 공간 복잡도를 통해 특정 알고리즘이 얼마만큼의 메모리를 차지하게 될 지를 나타낸다.

### 구현 복잡도
- 알고리즘 구현이 너무 복잡하고 알아보기 힘들다면 다시 생각해볼 필요가 있다. 개발자는 대부분의 경우 협업을 해야하기 때문에 다른 사람들, 미래의 내가 알아 볼 수 있어야 한다.

## 시간 복잡도 Time Complexity
### Runtime(실행 시간)
- 완성된 프로그램을 실행시키면 컴퓨터(CPU)는 한 줄 한 줄 코드를 처리한다.
- 모든 코드를 처리하면 원하는 값을 얻을 수 있게 되고 프로그램이 끝난다.
- **프로그램이 시작되고 모든 코드를 실행하는데 걸리는 시간을 `runtime(실행시간)`이라고 한다.**  
 
### 시간 복잡도
- **시간 복잡도는 문제를 해결하는데 걸리는 시간과 입력값 n의 함수 관계를 말한다.**
- 코딩테스트에서는 단순히 코드 구현력을 넘어, 효율적인 알고리즘적 사고능력을 요한다.
- 시간 복잡도는 `runtime(n)`값을 토대로 `Big-O notation`으로 표현한다.

### Big-O
- input n의 크기가 커지면 작은 차수의 항들이 runtime에 미치는 영향이 미미하다. **작은 차수, 계수는 무시한다.**
> n! > 2^n > n^3 > n^2 > nlogn > n > logn > 1
<img src="https://user-images.githubusercontent.com/72978589/207917557-fc40cfae-a3fe-4a0a-bd19-c0d10e648f24.png" width="70%" height="20%">    

- O(1)
```kotlin
var a = 5
a += 1
```
- O(n)
```kotlin
for(i in 0 until n){
  println("hi")
}
```
- O(n^2)
```kotlin
for(i in 0 until n){
  for(j in 0 until n){
    println("hi")
  } 
}
```
- O(2^n)
```kotlin
두 번 재호출하는 재귀함수
fun factorial(n: Int, acc: Int): Int {
    return if (n <= 0) {
        acc
    } else {
        factorial(n-1, n*acc)
    }
}

```
- O(logn)
```kotlin
fun binarySearch(arr: IntArray, target: Int): Int {
    var low = 0
    var high = arr.lastIndex
    var mid = 0;

    while (low <= high) {
        mid = (low + high) / 2

        when {
            arr[mid] == target -> return mid
            arr[mid] > target -> high = mid - 1
            else -> low = mid + 1
        }
    }
    return -1
}
```

## 코딩 테스트를 위한 시간복잡도
### 외우면 편한 것들
- `sort()` O(nlogn)
- Hashtable 구축: O(n) // Hashtable 검색: O(1)
- Binary Search: O(logn)
- Priority queue 우선순위 큐
  - 길이가 n인 배열을 heap으로 만들 때 O(nlogn)
  - `push()` O(logn)
  - `pop()` O(logn)


## 코딩테스트에서 활용
> 시간복잡도(Big-O)에 데이터의 크기(n)를 넣어서 나온 값이 100,000,000(1억)이 넘으면 시간 제한 초과할 가능성이 있다.
- runtime을 간단히 표현한게 시간복잡도이다. 따라서 시간복잡도에 데이터의 크기 n을 넣으면 대략적인 runtime(실행시간)을 구할 수 있다.
### 제한조건  
- 100,000  
문제를 풀다보면 제한사항에 데이터의 크기를 100,000으로 제한한 문제들을 자주 보게 된다.  
&rarr O(n^2)로 풀기는 위험하다.  
&rarr O(nlogn), O(n), O(logn)의 알고리즘을 생각해야 한다.

- 비교적 작은 숫자
제한사항에 데이터의 크기를 1,000으로 제한한 문제들도 종종 나온다.
&rarr O(n^3)로 풀기는 위험하다.
&rarr O(n^2), O(nlogn), O(n), O(logn)의 알고리즘을 생각해야 한다.

- 데이터의 크기 제한이 작다면 `완전탐색` 고려
문제에서 데이터 크기 제한이 작다면, 완전탐색으로 풀어도 된다는 힌트이다.
- O(n^4)으로 4중 반복문이 나와도 풀 수 있다.
<img src="https://user-images.githubusercontent.com/72978589/207931805-ac873ea9-0d51-41b9-b51a-5581a9a0b70e.png" width="40%" height="20%">    


- 순열조합 완전탐색 문제
순열조합 문제는 완전탐색의 연장선상에 있다. 순열조합 문제는 시간복잡도가 굉장히 크기 때문에 작은 크기의 제한사항을 준다.
```
제한사항 예
numbers는 길이 1이상 7이하인 문자열입니다.
```

#  
```
참고
- https://www.nossi.dev/cote/time-complexity
- https://gyubgyub.tistory.com/56
- https://codechacha.com/ko/kotlin-examples-factorial/
- 
```
