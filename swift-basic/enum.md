# enum

---

오늘 간단한 movie app을 만들면서 enum을 사용 했을 때 개념이 부족하다고 생각이 들었다.

따라서 enum에 대해서 다시 한번 공부 해보려고 한다.!

### enum(Enumerations)

* 한국말로는 열거형!, 같은 주제로 연관된 데이터들을 프로퍼티로 구성하여 나타내는 자료형이다

* 타입 ___분류___를 좀 더 쉽게 할 수 있는 개념이다.

  ~~~swift
  //example
  enum BookType{
  	case fiction
  	case comics
  	case workbook
  }
  
  var bookstyle = BookType.comics
  
  func saveBook(book: BookType){
    	switch book{
        case .fiction:
        case .comics:
        case .workbook:
      }
  }
  ~~~

* 공통된 주제에 대해서 이미 __정해놓은 입력 값__ 만 선택해서 사용 하고 싶을 때 사용하는 것이 enum

* enum을 사용할 경우 코드 가독성이 향상되고 오타로 인한 오류 발생률 이 적어져서 안전성이 향상된다.

* Heap에 저장되는 String과 달리 enum은 stack 에 저장되므로 성능면에서도 향상 된다.

* 열거 형을 타입으로 지정한 경우, 타입을 추론 할 수 있는 경우에는 점문법으로 접근가능

  ~~~swift
  enum BookType{
  	case fiction
  	case comics
  	case workbook
  }
  var type : BookType = .comics
  ~~~

* enum에는 원시값(raw value)을 설정할 수 있다. - Number type,String type,Character type

  ~~~swift
  enum  BookRating : Int{
    case good  // 0
    case bad   // 1
    case soso  // 2
  }
  
  enum  BookRating : Int{
    case good   // 0
    case bad  = 10 // 10
    case soso  // 11
  }
  //Int타입의 enum은 raw value가 이전 case의 raw value 값에다가 1을 더한 값이 된다.
  
  enum  BookRating : Double{
    case good  = 1.0
    case bad   = 2.0
    case soso  //  error
  }
  // Number type 의 enum은 이전 case의 raw value 값에다가 1을 더한 값이 되는데 정수값이 아닌 값과 1을 더할 수 없기 때문에 오류가 난다
  //따라서 Number type은 정수가 아닌경우 raw value를 전부 지정해주거나 지정하지 않은 case의 이전 case는 정수여야 한다.
  
  enum  BookRating : Double{
    case good  = 1.0
    case bad   = 2
    case soso  //  3.0
  }
  
  //----------------------------------
  
  enum  BookRating : character{
    case good  = "g"
    case bad   = "b"
    case soso  // error
  }// character 타입의 enum또한 이전 case의 raw value가 character 타입이기 때문에 정수와 더하기 연산을 할수 없다
  
  // ------------------------
  enum  BookRating : String{
    case good  // good
    case bad   // bad
    case soso  // soso
  } // string 타입의 enum은 raw value를 지정하지 않은 경우 case이름이 raw value가 된다.
  ~~~

* eunm의 raw value를 사용하면 모든 case가 동일한 type의 raw value를 가져야 하고 case 별로 미리 지정된 한가지 값만 사용할 수 있는 단점이 있음

  이를 보안한 게 __연관값__(Associated Value)!!

  * case에 따라서 사용되어지는 값의 타입이나 수,종류가 다를 때 사용하면 좋은 것 같다.

  ~~~swift
  enum AppleProduct{
  	case IPhone(model:String)
    case Mac(model:String,size:Int)
  }
  let product : AppleProduct = .IPhone(model:"IPhone12mini")
  
  //switch 문에서 유용하게 사용 할 수 있다.
  
  switch product{
    case .IPhone: break
    case .IPhone("IPhone12mini"): break
    case .Mac(let model,let size): break// 연관값 바인딩
    case let .Mac(model,size): break 
    default: break
  }
  
  if case let .IPhone(model) = product{
    	print(model)
  }
  ~~~

### movie app을 만들면서 사용했던 eunm

~~~swift
enum RequestType{
    case justURL(urlString : String)
    case searchMovie(queryItems : [URLQueryItem])
}//query parameter없이 url로만 호출하는 경우와 query parameter와 함께 url를 request하는 경우 두가지를 enum으로 분류 했다.
enum RequestError : Error{
    case badURL
}

func buildRequest(type: RequestType) throws -> URLRequest{
        switch type {
        case .justURL(urlString: let urlString):
            guard let hasURL = URL(string: urlString) else{ throw RequestError.badURL }
                var request = URLRequest(url: hasURL)
                request.httpMethod = "GET"
                return request
            
        case .searchMovie(queryItems: let queryItems):
            var components = URLComponents(string: "https://itunes.apple.com/search")
            components?.queryItems = queryItems
            guard let hasURL = components?.url else { throw RequestError.badURL }
                var request = URLRequest(url: hasURL)
                request.httpMethod = "GET"
                return request
        }
    }
~~~

---

### 느낀점

이번에 enum을 공부하면서 왜 string 보다 enum이 성능면에서 좋은 지도 궁금해졌다 

따라서 다음번에는 stack 과 heap 그리고 value type과 reference type 의 메모리 성능에 대해서 공부 해보려고 한다!

### 참고

https://minhamina.tistory.com/114

https://babbab2.tistory.com/116

https://babbab2.tistory.com/117?category=828998

