---
layout: single
title:  "[Android] Fragment안에서 TabLayout, ViewPager 사용하기 (코틀린)"
categories: Android
published: true
---

오늘은 Fragment안에서 ViewPager와 TabLayout을 함께 사용하는 방법에 대해서 알아볼 것이다.

Fragment안에서 ViewPager와 TabLayout을 함께 사용하고 싶은데 대부분 Activity안에서 사용하는 방법밖에 안나와있어서 이렇게 글을 작성하게 되었다.

# 배경

---

현재 MainAcitivty위에 여러개의 Fragment를 bottomNavigation을 통해서 구현되어 있는 중에 특정 Fragment에서 ViewPager와 TabLayout을 사용해야 할 상황이 생겼다.

대부분의 경우 ViewPager와 TabLayout을 Activity 위에서 구현하여 여러개의 Fragment를 관리하며 사용하는 방식이겠지만 본인은 Fragment 안에서 구현해야하는 방법이 필요했다.

지금부터 그 방법에 대해서 알아볼 것이다.



## 부모 Fragment 수정

먼저 ViewPager와 TabLayout을 구현 시킬 부모 Fragment에 두 라이브러리를 구성해준다.

***fragment_home.xml***

```xml
<LinearLayout>
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <com.google.android.material.tabs.TabLayout
            android:id="@+id/tabLayout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/background"
            android:paddingBottom="8dp"
            app:tabMode="fixed"
            app:tabPaddingTop="15dp" />

        <androidx.viewpager.widget.ViewPager
            android:id="@+id/viewPager"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
            
</LinearLayout>

```


## Adapter 추가

***DetailPagerAdapter***

```kotlin
class DetailPagerAdapter(manager: FragmentManager): FragmentPagerAdapter(manager){

    var fragmentList: MutableList<Fragment> = arrayListOf()
    var titleList: MutableList<String> = arrayListOf()

    override fun getItem(position: Int): Fragment {
         return fragmentList[position]
    }

    override fun getCount(): Int {
         return fragmentList.size
    }

    override fun getPageTitle(position: Int): CharSequence? {
        return titleList[position]
    }


    fun addFragment(fragment: Fragment, title: String){
        fragmentList.add(fragment)
        titleList.add(title)
    }
}

```



## 화면에 보여줄 Fragment 생성

화면에 보여줄 임의의 Fragment를 생성해준다.


## 부모 Fragment 클래스에 바인드



***HomeFragment.kt***

```kotlin
class HomeFragment : Fragment() {

    lateinit var myFragment: View
    lateinit var viewPagers: ViewPager
    lateinit var tabLayouts: TabLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        arguments?.let {
        }
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        myFragment = inflater.inflate(R.layout.fragment_hospital, container, false)
        return myFragment
    }

    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)

		setUpViewPager()
        tabLayouts.addOnTabSelectedListener(object : TabLayout.OnTabSelectedListener {
            override fun onTabReselected(tab: TabLayout.Tab?) {
            }

            override fun onTabUnselected(tab: TabLayout.Tab?) {
            }

            override fun onTabSelected(tab: TabLayout.Tab?) {
            }
        })
    }


    private fun setUpViewPager() {
        viewPagers = viewPager
        tabLayouts = tabLayout

        var adapter = DetailPagerAdapter(requireFragmentManager())
        adapter.addFragment(UserRetroFragment(), "인기 장소")
        adapter.addFragment(UserPlaceFragment(), "최신 추천 장소")

        viewPagers!!.adapter = adapter
        tabLayouts!!.setupWithViewPager(viewPagers)
    }
}

```


위와 같이 코드를 작성하면 원하던 기능 구현 끝이다.

생각보다 굉장히 간단하다.

<br/><br/>

## 최종 결과물

![Screenshot_20230419_221538_MyRetro](https://user-images.githubusercontent.com/69960282/233086852-9547ff4f-7165-46b9-a160-bf9fa5dc5cac.jpg)


