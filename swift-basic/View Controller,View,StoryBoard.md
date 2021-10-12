# View Controller, View, StoryBoard

---

### StoryBoard

* Storyboard는 앱의 흐름을 나타내며 시각적으로 화면을 구성하는 곳이다.
* 앱의 전반적인 형태와 화면 전환, 다양한 Object(View) 를 관리해주는 곳이다.
* StroyBoard = 여러개의 Scene(View controller) + Segue(화면 전환)

### View Controller

* 이름 그대로 View 들을 관리하는 객체 이다.

* 화면과 데이터 사이의 상호 작용관리 까지 한다.(중요한 데이터의 변화에 응답으로 뷰들의 컨텐트들을 업데이트 한다.)

* View 들과 함께 사용자의 대화에 응답한다.

* **뷰 컨트롤러는** **Responder객체**이고 Responder Chain 에 연결된다. 따라서 view controller 도 **이벤트 헨들링**을 할 수있다.

* View Controller 에는 root view가 있고 root view 에 다양한 sub view 를 더해서 화면을 구성한다

* View Controller 하나가 하나의 Scene을 담당한다.

* View Controller 의 종류

  * Content View Controller 

    ~~~ 
    앱의 컨텐츠의 일부분을 관리하는 뷰 컨트롤러
    자신의 모든 뷰들을 스스로 관리한다.
    일반적인 View Controllers
    ~~~

  * Container view controller

    ~~~
    다른 뷰 컨트롤러들로 부터 정보를 모은다.
    자신의 뷰들과 자신의 자식 뷰 컨트롤러들의 root view들을 관리한다.
    직접 자식 뷰 컨트롤러의 컨텐츠를 관리하지는 않고 root view의 크기조절과 위치조절에 대해서만 관리한다.
    NavigationViewController / TabBarViewController
    ~~~

* Life Cycle

  ~~~
  viewDidLoad()
  뷰컨트롤러의 root view -> var view: UIView!프로퍼티는 지연로딩된다. view가 필요로 될 때 view가 nil이면 loadView() 메소드를 호출하여 view를 로드한다.
  loadView()직 후 에 호출되는 콜백메소드이다.
  
  viewWillAppear()
  뷰컨트롤러의 root view 가 로드된 이후에 window 의 뷰 계층으로 더해지기 직 전 호출되는 메소드이다.
  
  viewDidAppear()
  window 의 root view가 뷰 계층으로 더해진 직 후 호출되는 메소드이다.
  
  viewWillDisAppear()
  window 의 root view가 뷰 계층에서 제거되기 직 전 호출되는 메소드이다.
  
  viewDidDisAppear()
  window 의 root view가 뷰 계층에서 제거된 직 후 호출되는 메소드이다.
  
  ~~~

  

### View

* UIView 클래스의 인스턴스
* 화면에 나타나는 거의 모든 요소는 뷰의 컨텐츠이다
* 여러 개의 뷰를 계층구조로 쌓아올려 UI를 표현하게 된다.

---

### 참고

https://boidevelop.tistory.com/13

https://etst.tistory.com/79

https://o-o-wl.tistory.com/43





