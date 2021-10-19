# 개발할 때 didSet을 사용하면서 의문점

---

### 인프런 강의를 보면서 app을 만드는 데 이해가 안되는 부분이 있었다.

- 왜 didSet 부분에 초기에 지정하고 싶은 상태를 지정 해주지? Ex)

  ~~~swift
  @IBOutlet weak var nextButton: UIButton!{
          didSet{
              nextButton.isEnabled = false
          }
      }
  ~~~

  

만약 이렇게 되면 button의 text값이나 상태가 변할 때마다 didSet이 호출 되는거 아닌가? 라는 의문이 들었다. 또한 IBOutlet 변수에 didSet을 설정했을경우 처음 언제 호출 되는 지 궁금 하였다. 그래서 view controller의 life cycle이랑 IBOutlet 등등 여러가지를 찾아가면서 이유를 찾아봤다.

### 먼저 !!!!

* @IBOutlet 프로퍼티 란? 

  ~~~swift
   StoryBoard 상에 선언한 View 객체를 Interface Builder(IB) 가 알아볼 수 있게 만드는 것 (??) 
   이 말을 듣고는 이해가 잘 가진 않았다 따라서 여러 사이트를 찾아보고 내가 이해한 개념은 
   StoryBoard 상에 있는 view에 접근하기 위한 프로퍼티 라고 생각하고 view를 코드 와 연결하는 수단이라고 생각한다
   @는 컴파일러에게 어떤 속성 가지고 있다고 전하는 역할 하는 예약어 라고 한다.
   따라서 @IBOutlet이라는 말의 뜻은 인터페이스 빌더 의 view와 연결하는 프로퍼티라고 알려주는 예약어라고 볼 수 있다.
  ~~~

  그러면 왜 didSet에 초기에 지정하고 싶은 상태의 코드를 적은 것인가?

  @IBOutlet 변수가 처음 초기화 되는 시점이 언제인가 알아 보기로 했다

* ViewController LifeCycle

  ~~~swift
  ViewController lifecycle에 대해서 공부한 적이 있지만 이번에 좀 더 많이 찾아 봤다.
  이 전에는 viewdidLoad->viewWillAppear->viewWiilDisappear->viewDidDisappear 까지만 공부했지만 더 많은 단계가 존재하였다
  init->loadView->viewdidLoad->viewWillAppear->viewWiilDisappear->viewDidDisappear 라는 단계에 대해서 공부를 하였다.
  
  먼저 init에선 storyboard를 통해 view controller를 만들었을 경우 초기화 작업을 하는 메소드는 init(coder:)메소드 이고 xib로 만들었을 경우
  init(nibName:bundle:) 로 초기화 작업을 진행한다.
  
  이번에 공부 한 것중에 제일 핵심이였던 loadView 에서는 viewController.view 를 만드는 작업을 한다고 한다.
  애플 공식문서 설명에 따르면 loadView()는 직접 호출하면 안되고, View Controller가 view를 요청받았는데, view가 정의되어 있지 않다면 실행이 된다고 한다.
  viewdidLoad는 이렇게 만들어진 view 가 메모리에 로드 되면 호출되는 메소드이다 따라서 우리는 여기서 다양한 작업들을 많이 하게 된다.
  IBOutlet 이 초기화 되는 부분은 loadView 라고 한다. 아마도 storyboard상에서 모습대로 loadView에서 rootview를 생성하고 그 때 subview들이랑 IBOutlet 프로퍼티랑 연결이 되는 거 같다.
  ~~~

  자 그러면 호출되는 타이밍은 알았는데 만약

  ~~~swift
  nextButton.text = "abc"
  ~~~

  이렇게 속성값을 바까주게 되면 didSet이 호출되는 거 아닌 가 라고 생각했다.

  찾아 보니

  ###### class 즉 reference type 에서 didSet은 class 안에 속성이 변해도 reference가 변한게 아니기 때문에 didSet이 호출이 되지 않는다

  ##### 하지만 Struct일 경우에는 value type 이다. 프로퍼티의 값이 바껴도 인스턴스 자체가 바뀌는 것이기 때문에 didSet이 호출 된다.

  ###### 따라서 IBOutlet 프로퍼티에 다른 인스턴스를 넣을 일은 없기 때문에 저렇게 사용 한 것이였다. 

  ---

  

  ### 느낀점  

   공부하면 할수록 어려운 부분이 많고 공부해야할 부분 이 많은 것 같다.

  공부하면서 두려운 거는 구글에 많은 자료가 있다 보니 다양한 자료들을 보면서 내가 해석하는 내용이 맞는 건가 제대로 이해 한게 맞는건지 나 자신에게 의문이 들어서 공부하면서 확신이 생기지 않아 조금 그 부분이 어렵다.  하지만 너무 재밌다. 오래 걸리지만 여러 자료들은 찾아보고 정리해서 지식을 습득 했을 때 점점 자신감이 생기는 거 같다.

  ### 참고

  https://ahyeonlog.tistory.com/1

  https://jouureee.tistory.com/9

  https://leehonghwa.github.io/blog/loadView/

  https://velog.io/@dlskawns96/iOS-viewDidLoad-VS-loadView

  https://maivve.tistory.com/176

  https://etst.tistory.com/74

  https://baked-corn.tistory.com/32

  https://unclean.tistory.com/78