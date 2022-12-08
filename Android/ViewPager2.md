# ViewPager2

## ViewPager 란?
- View를 슬라이드쇼처럼 넘길 수 있는 페이징 기법이다.
- 2019년, 구글이 viewPager2를 발표하면서 기존 방법보다 사용하기 훨씬 쉬워졌다. 리사이클러뷰(recyclerview) 사용하듯이 사용하면 된다.
- [공식 문서](https://developer.android.com/jetpack/androidx/releases/viewpager2?hl=ko)

## ViewPager2 특징
- ViewPager2는 `RTL(Right-to-Left)` `수직 방향(Vertical Orientation)` `수정 가능한 Fragment Collection` 등을 지원한다.
- Adapter에 따라 형태가 달라진다.
  - `FragmentStateAdapter`를 붙이면 기존 ViewPager 방식이다.
  - `RecyclerView.Adapter`를 붙이면 **ViewPager와 RecyclerView가 혼합된 방식**으로 만들 수 있다.
### RecyclerView.Adapter
- RecyclerView.Adapter 사용 방법과 같다.
-  RecyclerView.Adapter를 이용한 ViewPager2는 Layout이 같고 contents가 다른 화면을 만들 때 유용하다.
- [Adapter(+ViewHolder) + item_view.xml]을 ViewPager2에 붙이면 된다.
- getItemCount()에서 반환된 개수만큼 item_view를 만들어 화면에 하나씩 보여준다.
<img src="https://user-images.githubusercontent.com/72978589/206439313-0d64addb-b9d6-4e30-a2f5-f733f27b8845.png" width="70%" height="40%">    

## ViewPager2 활용
<img src="https://user-images.githubusercontent.com/72978589/206432419-a53ce350-ef5a-4adc-950e-e03682a18e93.gif" width="30%" height="30%">    
- 광고 배너, 소개 페이지 등 다방면에서 활용된다.

## 구글 디자인 정책상 권장되지 않는 방법
- 스와이프를 통해 페이지(메뉴)를 변경한는 것은 **구글 디자인 정책상 권장되지 않는 방법**이다. [참고 기사](https://www.sedaily.com/NewsVIew/1S4JMKWUI0)

## 사용 예시
### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewPager2"
        android:layout_width="match_parent"
        android:layout_height="300dp"
        android:background="#F0F0F000"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

### number_item.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <TextView
        android:id="@+id/numberTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="15sp"
        android:textColor="@color/purple_200"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

### MainActivity.kt
```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        val view = binding.root
        setContentView(view)

        // 어댑터 생성
        binding.viewPager2.adapter = ViewPagerAdapter(getNumber())
        // ViewPager의 Paging 방향은 Horizontal
        binding.viewPager2.orientation = ViewPager2.ORIENTATION_HORIZONTAL
    }
    private fun getNumber(): ArrayList<Int>{
        return arrayListOf(1,2,3)
    }
}
```

### ViewPagerAdapter.kt
```kotlin
class ViewPagerAdapter(private val number: ArrayList<Int>):RecyclerView.Adapter<ViewPagerAdapter.PagerViewHolder>() {

    inner class PagerViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        private val numberTextView: TextView = itemView.findViewById(R.id.numberTextView)

        fun bind(number: Int, position: Int) {
            numberTextView.text = "Page $number"
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): PagerViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.number_item,parent,false)
        return PagerViewHolder(view)
    }

    override fun onBindViewHolder(holder: PagerViewHolder, position: Int) {
        holder.bind(number[position], position)
    }

    override fun getItemCount(): Int = number.size
}
```
### 결과 화면
<img src="https://user-images.githubusercontent.com/72978589/206442530-5f77e5e2-7c33-4f59-80d9-fe7d00f3f2a7.gif" width="30%" height="20%">    


## 응용
<img src="https://user-images.githubusercontent.com/72978589/206445944-350e299c-a0b1-452b-b90b-5a797a8d0dba.jpg" width="30%" height="20%">    

### values/dimens.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!--  item_view 간의 양 옆 여백을 상쇄할 값   -->
    <dimen name="offsetBetweenPages">40dp</dimen>
    <!--  각 페이지의 양 옆 margin  -->
    <dimen name="pageMargin">50dp</dimen>
</resources>
```

### number_item.xml  
- number_item.xml 최상위 Layout의 양옆에 dimens에서 작성한 PageMargin 값으로 margin을 주고 contentsdls TextView의 layout_height 값을 500dp로 지정한다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/numberTextView"
        android:layout_width="300dp"
        android:layout_height="500dp"
        android:background="#F0F0F000"
        android:gravity="center"
        android:textColor="@color/purple_200"
        android:textSize="15sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>

```

### MainActivity.kt
- ViewPager2의 Adapter를 지정한 부분 아래에 Paging Animation 처리에 대한 부분을 추가한다.
- myOffset 변수와 ViewPager2의 offscreenPageLimit이 Paging Animation 부분이다.
- myOffset 값은 position * -(2 * offsetBetweenPages)인데 offsetBetweenPages는 dimens.xml에서 40dp로 지정했기 때문에 position을 -80dp만큼 움직여라로 이해하면 된다.
- 아래 그림에서 각 Item은 각 Fragment의 전체 화면 기준으로 양쪽에 pageMargin 만큼의 margin을 준 상태이며 FirstItem을 기준으로 Second, ThirdItem이 왼쪽으로 2*offsetBetweenPages 만큼 움직였다.
- 각 Fragment의 양쪽에 margin이 있으니 Item 간의 여백은 margin*2가 될 것이고 그렇기 때문에 전, 후의 item이 현재 화면에 살짝 걸치게 되려면 2*offsetBetweenPages가 pageMargin보다 커야 한다.
- offscreenPageLimit은 ViewPager2의 1.0.0-alpha04에 추가되었고 Docs에 아래와 같이 나와있다.
> `offscreenPageLimit`은 뷰 계층 구조에 보관된 `View/Fragment` 수를 엄격히 제어할 수 있습니다.
  - ViewPager에서 페이지 관리하는 것과 같은 내용이다.
  - default 값은 1이며 앞, 뒤 화면 하나씩 Preloading 한다.
<img src="https://user-images.githubusercontent.com/72978589/206447777-b9cb9d4e-2541-4537-8624-368ae1378320.png" width="50%" height="30%">    

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        val view = binding.root
        setContentView(view)

        // 어댑터 생성
        binding.viewPager2.adapter = ViewPagerAdapter(getNumber())
        // 관리하는 페이지 수. default = 1
        binding.viewPager2.offscreenPageLimit = 3
        // item_view 간의 양 옆 여백을 상쇄할 값
        val offsetBetweenPages = resources.getDimensionPixelOffset(R.dimen.offsetBetweenPages).toFloat()
        binding.viewPager2.setPageTransformer{page, position ->
            val myOffset = position * -(2*offsetBetweenPages)
            if (position < -1){
                page.translationX = -myOffset
            }else if (position <= 1){
                // Paging 시 Y축 Animation 배경색을 약간 연하게 처리
                val scaleFactor = 0.8f.coerceAtLeast(1- kotlin.math.abs(position))
                page.translationX = myOffset
                page.scaleY = scaleFactor
                page.alpha = scaleFactor
            }else{
                page.alpha = 0f
                page.translationX = myOffset
            }
        }

    }

    private fun getNumber(): ArrayList<Int>{
        return arrayListOf(1,2,3)
    }
}
```
### 결과 화면
<img src="https://user-images.githubusercontent.com/72978589/206446680-e9df0356-4cef-4160-b05d-60380a750c1a.gif" width="30%" height="20%"> 

# 
```Text
참고
- https://todaycode.tistory.com/26
- https://heaven0713.tistory.com/59
- https://furang-note.tistory.com/25
```
