# 👍MVC Design Pattern

요즘 채팅앱을 만들면서 swift 의 기본(?) 이 되는 MVC패턴에 대해서 궁금함이 생겨서 한번 공부하면서 정리해보려고 한다.

### MVC Pattern ?
![image](https://media.vlpt.us/images/zooneon/post/8c95699b-2cbe-4159-b105-ba4ea3d652bf/image.png)
출처 :https://velog.io/@zooneon/iOS-MVC-%ED%8C%A8%ED%84%B4%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90

![image](
https://mblogthumb-phinf.pstatic.net/MjAxNzAzMjVfMTM0/MDAxNDkwNDQyNDI5OTAy.MUksll6Y9SzelJjmGW6zXOlPebJKOft3OhcnmhrcmTgg.4g4FxlhwEpgxp8kGXJVLf2LHlrRJhP7NqR7LJew8tL0g.PNG.jhc9639/ModelViewControllerDiagram.png?type=w800)
출처:https://m.blog.naver.com/jhc9639/220967034588

* MVC -> Model + View + Controller 구조의 아키텍처 패턴
* 사용자가 입력을 담당하는 View를 통해 요청을 보내면 해당 요청을 Controller가 받고, Controller는 Model을 통해 데이터를 가져오고, 해당 데이터를 바탕으로 출력을 담당하는 View를 제어해서 사용자에게 전달한다. 

<br><br><br>

### Model
---
모델은 앱이 포함해야하는 데이터가 무엇인지를 정의한다.
모델은 비즈니스 로직을 처리 한 후에 컨트롤러와 뷰에게 전달 한다. 모델은 다음과 같은 규칙이 있다.
* 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 한다.
*  뷰나 컨트롤러에 대해서 어떠한 정보도 알지 말아야 한다.
*  변경이 일어나면 , 변경 통지에 대한 처리 방법을 구현해야 한다.

### View
---
사용자에게 보여지는 부분 즉 유저 인터페이스를 말한다.
뷰는 다음과 같은 규칙이 있다.
* 모델이 가지고 있는 정보를 따로 저장해서는 안된다.
* 모델이나 컨트롤러와 같은 다른 구성요소를 몰라야 한다.
* 변경이 일어나면 변경 통지에 대한 처리방법을 구현해야만 한다.

### Controller
---
모델과 뷰를 이어주는 Bridge 역활을 수행한다.
모델과 뷰는 서로를 모르는 상태에서 외부로 알리는 방법만 가지고 있다. 이때 컨트롤러에게 변경내용이 전달되게 되면 다른 구성요소에게 전달시켜주는 역활을 수행한다.
사용자가 어플리케이션을 조작하여 발생하는 이벤트를 처리하는 수행을 한다.


<br>

### 🧐MVC 패턴을 왜 사용할까?? 
---
유지보수의 편리성 때문이다!!<br>
결합도가 높은 프로그램에서 코드를 수정할 경우
다른 비즈니스 로직에 영향을 끼칠 경우가 많다.
하지만 시스템을 model,view,controller 로 나누어서 자신이 맡은 역활을 수행한 후 다른 구성요소에게 결과를 넘겨주는 식으로 코드를 구성 할 경우 코드를 수정할 때 특정 구성요소에서만 코드를 수정하면 되기 때문에 유지보수가 편리해진다.
<br>
### MVC 패턴의 단점
---
![image](https://media.vlpt.us/images/zooneon/post/36581ee1-31f6-41a9-bc35-81124ba3beba/image.png)
출처:https://velog.io/@zooneon/iOS-MVC-%ED%8C%A8%ED%84%B4%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90
<br>
ios 에서 mvc패턴은 view와 controller가 너무 밀접하게 연결되어 있다. mvc패턴자체가 컴포넌트 끼리 나눠서 결합도를 낮추기 위한 목적인데 view 와 controller가 너무 밀접하면 둘을 분리 하기 어렵다.
이런 경우 각각 테스트하기 어렵다는 단점이 있다.
ios에서는 ViewController가 view의 LifeCycle까지 관여하게 된다.
또한 delegate,datasource,네트워크 요청,db 로직등 많은 코드가 Controller안에 들어가게 된다.
따라서 Controller의 크기가 비대해지고 내부 구조가 복잡하게 된다. 이런 상황을 비유해 MVC패턴을 Massive View Controller 라고 부른다고 한다.


아키텍쳐?
위키 백과를 찾아보면 
- 시스템 구성 및 동작 원리를 나타내고 있다.
- 구성 요소 간의 관계 및 시스템 외부 환경과의 관계가 묘사된다.
> 즉 시스템이 어떻게 돌아가는 지, 동작원리에 대한 설계도 라고 이해 된다.

그러면 아키텍쳐 패턴은?

> 아키텍쳐 패턴이란 주어진 상황에서의 소프트웨어 아키텍쳐에서 일반적으로 발생하는 문제점들에 대한 일반화되고 재사용 가능한 솔루션이다. 아키텍쳐 패턴은 소프트웨어 디자인 패턴과 유사하지만 더 큰 범주에 속한다.
> 즉 전체적인 시스템에서 발생하는 문제점들을 막기위해 약속하고 정한 솔루션이다.