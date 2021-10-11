# Weak,Strong

---

### ARC란?

+ Automatic Reference Counting

* 컴파일 시 코드를 분석하여 자동으로 <u>**(retain,release)**</u> 코드를 생성해주는 것
  - retain,reslease 코드? 
    * retain : retain count(= reference count) 증가를 통해 현재 Scope 에서 <u>객체가 유지</u>되는 것을 보장
    * release : retain count(= reference count) 감소 시킴. retain 후에 필요 없을 때 release함
* ARC는 reference Count를 통해 메모리를 관리한다.
* ARC는 Heap 과 관련이 있는데 Heap은 참조형 자료들이 머무는 공간이자, 개발자가 동적으로 할당 하는 메모리 공간
* 따라서 관리를 하기 위해서 Heap 영역에 참조형 자료들이 얼마나 참조 되고 있는 지 카운팅을 하고 카운트에 따라 메모리를 할당 및 제거를 통해 관리를 하면 된다.
* 이러한 관리를 해주는 것이 바로 ARC 이다.
* ARC의 메커니즘

~~~
ARC의 메커니즘은 swift runtime library 에 구현되어 있다.
swift runtime은 동적 할당 되는 모든 object를 HeapObject 라는 struct로 표현한다.HeapObject는 객체를 구성하는 모든 데이터 reference count와 type meta data를 포함하고 있다.

ARC가 complie time에 실행 되는데 어떻게 동적으로 실행되는 것들의 reference count를 세고 메모리 관리를 할수 있는 가?
1.complie time에 코드 분석을 통해 적절한 위치에 retain,release 코드를 삽입
2.삽입된 코드는 run time에 실행
3.retain,release 코드를 통해 reference Count를 증감 시키다가 count가 0이 되면 deinit를 통해 해제시킴
4.referenceCount 는 동적 할당된 object를 표현하는 HeapObject struct에서 접근 가능
~~~

### Weak,Strong

* Weak (약한 참조)

  * 포인터 개념이다(해당 인스턴스의 소유권을 가지지 않고 주소값만 가지고 있다)
  * 자신이 참조하는 인스턴스의 reference Count 증가 x,release 발생 x
  * 자신이 참조는 하지만 메모리를 헤제시킬 수 있는 권한은 다른 클래스에 있다.
  * 메모리가 해제 되면 자동으로 ARC가 nil로 초기화를 해준다.
  * weak속성을 사용하는 객체는 항상 optional 타입이어야 한다.(해당 객체가 nil이 될 수 있기 때문에)

* Strong (강한 참조)

  * 해당 인스턴스의 소유권을 가진다.
  * 자신이 참조하는 인스턴스의 reference Count를 증가 시킨다
  * 값 지정 시점에 retain이 되고 참조가 종료되는 시점에 release가 된다.
  * 변수 선언시 디폴트가 strong

* 어느 상황에 쓰이는가

  * Weak : 대표적으로 retain cycle에 의해 메모리가 누수되는 문제를 막기 위해 사용되면,delegate 패턴

    * 메모리 부족 

      ~~~
      메모리가 부족하면 ViewController의didReceiveMemoryWarning가 호출된다.
      보통 didReceiveMemoryWarning 에서 main view를 nil 처리함으로써 main view를 포함한 subview들 까지 모두 dealloc 하여 메모리를 확보하는 동작을 구현한다.
      
      이 경우에 IBOutlet Strong 으로 Subview들을 갖고있다면, ViewController가 Strong으로 갖고있는 Reference Count 1 때문에 절대 최소 1 이하로 내려가질 않는다.
      즉, 그 Subview의 ParentView가 nil이 되더라도 SubView는 dealloc(deinit)되지 않게 된다. 보이지도 않는 뷰가 메모리를 계속 차지하게 된다.
      
      
      ~~~

      

    * 순환 참조 - 서로가 서로를 소유하고 있어 절대 메모리 해제가 되지 않는다는 것을 말한다
    * 순환 참조를 피하기 위해 weak를 사용해야 한다.

    ~~~swift
    vc.delegate = self
    // vc2안에서 vc를 선언하고 vc의 delegate 를 vc2로 지정하였다.
    // vc2는 vc를 소유하고 vc의 delegate로 vc2를 지정했기 때문에 서로를 참조하는 상황이다. 따라서 이러면 메모리가 해제가 되지 않을 수 있기 때문에 weak를 사용해야 한다.
    ~~~

  * Strong : reference Count 를 증가시켜 ARC로 인한 메모리 헤제를 피해서 객체를 안전하게 사용하고자 할 때 쓰인다.

---

###### 느낀점

일단 처음 이렇게 markdown 언어를 사용하여 공부하는 것을 기록남기기 시작했는데

앞으로 꾸준히 했으면 좋겠다

HeapObject에 type metadata의 내용을 찾고 싶었지만 자료가 많이 없고 영어로 되어 있어서 해석하기가 어려웠다 이 부분은 나중에 따로 공부를 더 해야 할 거같다

youtube강의 에서 weak,strong에 대해서 설명할 때 weak이 무조건 좋다고 설명 해주셨는데 실제로 현업에서 어떤 상황에서 많이 쓰이는 지 궁금하다

###### 참고

https://devsrkim.tistory.com/entry/Swift-%EB%A9%94%EB%AA%A8%EB%A6%AC%EB%A5%BC-%EC%B0%B8%EC%A1%B0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-Strong-Weak-Unowned

https://sujinnaljin.medium.com/ios-arc-%EB%BF%8C%EC%8B%9C%EA%B8%B0-9b3e5dc23814

https://belkadan.com/blog/2020/09/Swift-Runtime-Type-Metadata/

http://monibu1548.github.io/2018/05/03/iboutlet-strong-weak/