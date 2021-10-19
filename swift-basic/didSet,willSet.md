# 프로퍼티 옵저버

---

### 프로퍼티 옵저버란 ?

* 프로퍼티 값의 변화를 관찰하는 것으로,저장 프로퍼티에 추가 할 수 있다.
  새 값의 속성이 __현재 값과 동일 __하더라도 속성 값이 설정(set) 되면 호출된다. 

* willSet,didSet

  * willSet - 새로운 값이 설정 되기 전에 호출

  * didSet - 새로운 값이 설정 된 후에 호출

    ~~~swift
    class student {
    		var school = "홈스쿨링"{
      		willSet(newschool){
            print("\(school)에서 \(newschool)로 전학")
          }
          didSet(oldschool){
             print("\(oldschool)에서 \(school)로 전학")
          }
        }
      func 전학(newschool){
         self.school = newschool  // 전학 메소드가 호출되어 school 저장프로퍼티의 값이 새로 설정될 때 willSet이 먼저 호출되게 된다.
        // 이때 아직 값이 설정 되기 전이고 매개변수로는 설정 되어 질 새로운 값이 오게 된다.
        // 값이 설정되고 난 후 didSet이 호출된다. didSet은 값이 설정되고 난 후 이 전에 저장되어 있던 값이 매개변수로 오게 된다.
        // 매개변수를 지정하지 않으면 newValue,OldValue로 각각 willSet,didSet 에서 접근 할 수 있다.
        // willSet-> 값 변경 -> didSet
      }
    }
    ~~~

    

  * 연산 프로퍼티에서도 부모 클래스의 연산 프로퍼티를 오버라이딩 할 경우 프로퍼티 옵저버를 추가 할 수 있다.

    * 일반적으로 연산프로퍼티에서 프로퍼티 옵저버를 사용하면 오류가 발생한다. 왜냐하면 setter를 통해 값 변경을 감지 할 수 있는데 굳이 옵저버를 사용해야 하냐는 의미이다. 하지만 부모 클래스의 연산 프로퍼티를 오버라이딩 할 경우  사용이 가능하다

      ~~~swift
      class student {
          var school = "홈스쿨링"
      		var currentschool : String {
            get{
              return school
            }
            set{
              return school + "학교"
            }
        		willSet(newschool){
               // error! 'willSet' cannot be provided together with a getter    
            }
            didSet(oldschool){
               // error! 'didSet' cannot be provided together with a getter
            }
          }
        func 전학(newschool){
           self.school = newschool  
        }
      }
      
      class Jaeyoung : student{
         override var currentschool : String {
           	willSet(newschool){
              print("\(school)에서 \(newschool)로 전학")
            }
            didSet(oldschool){
               print("\(oldschool)에서 \(school)로 전학") 
            }
         }
      }
      ~~~

      

* 내가 사용한 실습하면서 사용해본 didset

  ~~~swift
  var currentIndex = 0
      {
          didSet{
              pageControl.currentPage = currentIndex
          }
      } // pageViewController에서 page가 변경되서 currentIndex가 변경 될때마다 pageControl의 currentPage값을 currentIndex값으로 초기화 해줬다.
  
  
  -------------
      @IBOutlet weak var nextButton: UIButton!{
          didSet{
              nextButton.isEnabled = false
          }
      }
  //버튼이 처음 scene이 보여 졌을 때 비활성화 상태로 만들기 위해서 사용했다.
  ~~~

  

