# 2주차 튜터링 사항

## 코드 리뷰

1. 패키지 명을 구분되게 만들어서 분류할 것.
2. 프래그먼트는 싱글톤 패턴으로 만들어서 사용할 것.
	* 싱글톤 패턴 : 해당클래스의 인스턴스가 하나만 만들어지고, 그 인스턴스에 대한 전역 접근할 수 있게 하는 패턴이다.
	* 프래그먼트를 한번만 사용하는 경우에는 싱글톤 패턴을 사용한다. 다만 하나의 프래그먼트를 따로 여러곳에 사용했을 경우에는 사용하지 않는다 
			
			Public class Singleton{
				private static Singleton uniqueInstance;
				
				private Singleton(){}

				// 멀티태스킹을 위해 각 스레드에서 getInstance를 호출할 때 동기화 작업을 통해 동시실행이 불가능하도록 한다.
				public static synchronized Singleton getInstance(){
				if(uniqueinstance == null){
					uniqueInstance = new Singleton();
				}
				return uniqueInstance;
				}
			}

	* 프래그먼트의 싱글톤 패턴 예제

			public class MyFragment extends Fragment {
		
			    /**
			     * Static factory method that takes an int parameter,
			     * initializes the fragment's arguments, and returns the
			     * new fragment to the client.
			     */
			    public static MyFragment newInstance(int index) {
			        MyFragment f = new MyFragment();
			        Bundle args = new Bundle();
			        args.putInt("index", index);
			        f.setArguments(args);
			        return f;
			    }
		
			}

3. 안쓰는 코드는 삭제하여 가독성을 높이자.
4. 툴바의 사용 이유!
	1. 툴바는 View를 상속하므로 원하는 곳 어디에나 붙일 수 있다.
	2. View 이므로 애니메이션 적용이 쉽다.
	3. 스크롤에 따라 위치가 변경되도록 하는 등 많은 응용이 가능하다.

5. 1개의 클래스는 1개만 선언할 것.
6. 패키지명에 언더바를 제외하고 .(dot)을 사용하여 구분할 것.
7. OnPageScrolled와 OnPageSelected, onScrollStateChanged 의 차이
	* OnPageScrolled : 스크롤 할때마다 이벤트가 발생, 움직이면서 애니메이션을 주고 싶을 때 사용.
	* OnPageSelected : 한개의 아이템이 선택되었을 때만 이벤트가 발생한다.
	* onScrollStateChanged :뷰에서 스크롤 되었을때 발생하는 콜백메소드, 스크롤의 상태에 따라 관리할 수 있다.

8. 이미지 해상도 별로 패키지를 관리하자.
9. 비트맵이미지는 서버에서 관리하자. 클라이언트는 URL만 가지고 있자. 액티비티가 발생할때마다 메모리과부하가 일어날 가능성이 크다.
10. RecyclerView에서 ViewType을 적극적으로 사용할 것.

## 질의 문답
1. dp, sp, px의 차이점.
	* dp : 화면 밀도에 따른 픽셀 값. 비율을 가지고 하기 때문에 해상도마다 비슷한 화면 구성을 할 수 있다.
	* sp : 시스템 의존성 픽셀 값. 보통 텍스트뷰에 사용하며 시스템의 폰트 크기 설정에 따라 크기가 달라질 수 있다.
	* px : 픽셀 절대값. 사용되지 않는다.

2. Invisible, Gone의 차이점
	* Invisible : 보이지 않게만 하고 공간은 차지한다. 보이지 않더라도 뷰가 존재하기 때문에 클릭할 경우 뷰에대한 반응이 일어남
	* Gone : 완전이 보이지 않음(존재하지않는 것처럼), 공간차지도 안한다.

3. Margin, Padding의 차이점
	* Margin : 뷰의 바깥 부분의 여백
	* Padding : 뷰의 내부에서 컨텐츠와의 여백
4. 레이아웃 별 차이점
	1. LinearLayout : Horizontal과 Vertical로 나누어 연달아서 뷰의 위치를 구성한다.
	2. RelativeLayout : 자식 뷰들이 서로 상대적인 위치로 존재할 수 있으며 또한 부모와도 상대적으로 위치할 수 있다.
	3. FrameLayout : 뷰들이 절대적인 위치 혹은 Gravity를 사용하여 이동시킬 수 있다. 
	4. ConstraintLayout : Linear와 Relative의 속성 모두 사용할 수 있다.
	4. 속도 : FrameLayout > LinearLayout > RelativeLayout
		* Framelayout은 절대위치로 뷰들을 탐색하기 때문에 실행속도가 빠르며, RelativeLayout는 뷰들의 상대적위치로 뷰들을 탐색하기 때문에 실행시간이 상대적으로 늦다. 
	5. 할 수 있으면, 중첩 Layout은 피해서 코딩하라. 1층의 Linear 또는 Relative로 가능한지 생각할 것.

5. src 속성과 background 속성의 차이
	* src : 컨텐츠 이미지, 여러가지 속성으로도 조절이 가능하다.
	* background : 배경 이미지, 컨텐츠보다 하위에 있으며, 기능이 별로 없다.
	
6. inflate 인자 값(LayoutId, Parent, attachToRoot)
	* LayoutId : 레이아웃 리소스 아이디
	* Parent: 뷰를 팽창시킬 때, 레이아웃의 인자값을 따라 갈 부모 뷰 그룹, Relative, Linear 속성에 따라 구별할 필요가 있다. Null값인 경우에는 Default값이 사용된다.
	* attachToRoot : 단순히 뷰를 팽창한 이후에 Parent에 뷰를 붙일 것인지 말 것인지를 판별하는 값이다.
		1. flase : 뷰를 붙이지 않이지는 않지만, **부모 속성을 사용**하겠다. 
		2. ture : 뷰를 부모 뷰에 붙이고 속성까지 사용하겠다.
		3. null : 부모 뷰에 붙이지도 않고, 속성을 사용도 안하겠다.
7. Set, List, Map의 차이점
	1. List : 순서가 존재하는 동적배열로 중복을 허용한다.
	2. Set : 순서를 유지하지 않으며 중복 또한 허용하지 않는다.
	3. Map : Key,Value 페어로 데이터를 저장한다. 순서를 유지하지 않고 Value의 중복을 허용하지 않는다.
		* Map에서 키가 중복되고 Value가 다를 경우, 충돌이 발생하게 되며, LinkedList 형태로 해당 키값에 추가된다. -> Map의 성능이 저하.

8. ArrayList, Vector, LinkedList의 차이
	* Vector
		* 동기화 처리되어, ArrayList, LinkedList에 비해 멀티스레딩 상황시 자료처리에 유리하다. 
		* 한 개의 자원을 하나의 스레드가 사용하는 경우에도 동기화를 고려하여 처리하게 되어, 단일 스레드에서는 오히려 성능의 저하를 가져온다.
		* 그러므로 단일 스레드 환경에서는 ArrayList, LinkedList보다 성능이 떨어진다.
		* Vector는 자바 1.0버전 호환성 때문에 보통 사용되기 때문에, 동기화처리가 필요한 경우에는 **Collection.synchronizedCollection(Collection c) 또는 synchronizedList**을 이용하는 것이 바람직하다.
	* ArrayList 
		* 내부적으로 배열구조를 가지고 있다. 데이터의 추가 삭제를 위해 내부적으로 임시배열을 작성 후 데이터를 복사하는 방법을 사용한다. 그렇기 때문에 대량의 자료를 추가/ 삭제하는 경우에 내부적인 처리량이 늘어나서 상당한 성능저하를 가져온다.
		* 보통 많은 데이터를 한번에 가져와서 여러번 참조할 때 최상의 성능을 가져온다. 각 데이터가 Index처리되어있기 때문에.
	* LinkedList
		* 내부적으로 Index를 가지지 않으며, 다음 자료에 대한 위치만 가진다.(Pointer)
		* 내부적으로 Pointer로 연결되어있기 때문에 추가 삭제가 용이하다.
		* 데이터가 많은 경우 검색시 처음 자료로부터 순차적으로 찾아나가야 하기 때문에 느려지는 단점이 있다.



참고자료

1. [dpi, dip의 개념](http://www.kmshack.kr/2013/03/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%B1-%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80-%EC%A0%9C%EC%95%88%ED%95%98%EB%8A%94-%EB%94%94%EC%9E%90%EC%9D%B8-%EB%B0%A9%EB%B2%95%EB%A1%A0-3-dpi-dipdp/)

2. [List, Set, Map의 이해](http://hackersstudy.tistory.com/26)

3. [클릭리스너를 사용하는 3가지 방법](http://jjunji.tistory.com/57)
	- 위 내용 중, 보통 2번 형태를 많이 사용한다.

4. 버터나이프 – 많이 사용. MVVM 패턴, 데이터바인딩
	- [데이터바인딩](https://developer.android.com/topic/libraries/data-binding/index.html#build_environment)
	- [MVVM 패턴](https://news.realm.io/kr/news/eric-maxwell-mvc-mvp-and-mvvm-on-android/)

5. DI 라이브러리 – Dependency Injection 라이브러리
