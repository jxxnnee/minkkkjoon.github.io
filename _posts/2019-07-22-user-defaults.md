---
title: "데이터 저장 - UserDefault, 프로퍼티 리스트 (2) "
elements:
  - content
  - css
  - formatting
  - html
  - markup
categories:
  - Swift
tags:
  - Swift
  - Data
  - Propertylist
  - UserDefault




---



# 2.1 UserDefaults 란?

UserDefaults는 **런타임(Runtime) 환경에서 동작하는 객체**이다. 앱이 실행되는 동안 기본 저장소에 접근하여 데이터를 가져오고 기록하는 역할을 한다.

반대로 이야기 하면 **앱이 실행되고 있지 않는 환경에서는 접근 할 수 없다**. 한마디로 플레이그라운드에서는 UserDefault 객체에 접근 할 수 없다는 뜻이다. 플레이 그라운드 상태에서 객체에 접근하더라도 값이 저장되지도 않을 뿐더러 가져온 데이터들은 모두 nil 값이다.

UserDefaults는 **싱글톤 패턴으로 설계**되어 있어 **딱 하나의 인스턴스만 생성**되며, 앱 전체가 이 하나의 인스턴스를 공유하여 사용하는 형태를 띈다. 따라서 UserDefaults를 사용할 때에는 직접 초기화 메소드를 호출하여 인스턴스를 생성하지 않고 이미 생성되어 있는 인스턴스를 참조하여 사용해야 한다.



### 2.1.1 동시성 문제

이렇게 하나의 자원이나 객체를 공유하여 사용하는 경우에는 예상하기 힘든 여러 문제가 발생할 수 있다. 그중에서 가장 유의해야 할 것이 바로 동시성(Concurrency) 문제, 그중에서도 경쟁 조건의 문제이다.

예를 들어 100명 선착순으로 나눠주는 쿠폰이 있는데 현재 카운트가 99인 시점에서 신청인 A가 출현했고 신청과정에서 신청인 B가 추가로 들어왔을때 생기는 문제를 보도록 하자

1. A가 쿠폰 신청을 위해 버튼을 눌렀을 때 읽어들인 현재 카운트는 99 이다.
2. A를 위해 읽어들인 값에 +1을 하여 결과값 100을 계산한다.
3. B가 쿠폰 신청을 위해 버튼을 눌렀을 때 읽어들인 현재 카운트는 99 이다.
4. A를 위해 계산된 값 100을 카운트 값으로 변경 후 쿠폰을 발행한다.
5. B를 위해 읽어들인 값에 +1을 하여 결과값 100을 계산한다.
6. B를 위해 계산된 값 100을 카운트 값으로 변경 후 쿠폰을 발행한다.

결과적으로 카운트의 최종 값은 100이지만 실제 쿠폰 발행 갯수는 101건이 발생하게 된다.

하나의 객체만 생각했을 때 전혀 문제가 되지 않는 로직이더라도 여러 객체가 동시에 하나의 자원에 접근하여 읽고 쓸 수 있을때에는 예상치 못한 문제들이 발생할 수 있다.

다행히 UserDefault는 먼저 들어온 요청에 **우선권을 부여하여 스스로 잠금을 걸고, 데이터를 읽고 쓰는 블로킹(Blocking)** 이라고 불리는 이 알고리즘 덕분에 동시에 여러곳에서 호출하여 데이터를 읽고 쓰더라도 순서가 꼬이거나 서로 영향을 끼칠 걱정 없이 마음 놓고 사용할 수 있다.

이것을 개발 용어로 **스레드로부터 안전하도록(Thread-safe) 설계되어 있다** 라고 표현한다.

<br>



### 2.1.2 UserDefaults 객체의 API

UserDefaults 객체는 데이터를 읽고 쓰기 위해 여러 개의 메소드를 제공한다. 다음은 읽기/쓰기 관련 주요 메소드 목록이다.

<br>

|    데이터 타입    |           값을 읽어올 때            |               값을 저장할 때                |
| :---------------: | :---------------------------------: | :-----------------------------------------: |
|    문자열 타입    |           string(forKey:)           | set(&#95;:forKey:) 또는 setValue(_:forKey:) |
|     정수 타입     |          integer(forkey:)           |                    &#34;                    |
|     실수 타입     | float(forkey:) 또는 double(forkey:) |                    &#34;                    |
| 참/거짓 논리 타입 |            bool(forkey:)            |                    &#34;                    |
|    Base64 타입    |            data(forkey:)            |                    &#34;                    |
|     배열 타입     |           array(forkey:)            |                    &#34;                    |
|   딕셔너리 타입   |         dictionary(forkey:)         |                    &#34;                    |
|     범용 타입     | object(forkey:) 또는 value(forkey:) |                    &#34;                    |
|     Nil 타입      |                  -                  |            setNilValueForKey(_:)            |

<br>

위 표에서 값을 읽어오는 메소드는 종류별로 나눠져있지만, 저장할 때 사용하는 메소드는 범용 메소드를 사용하는 것처럼 착각하기 쉽다. 하지만 사실 setValue(_:forKey:)만 범용 메소드이고, set(&#95;:forKey:)는 전용 메소드이다.

set(&#95;:forKey:)는 메소드 오버로딩(Overlading)되어 있기 때문에 동일한 이름을 가지고 있을 뿐이다. 모든 메소드의 목록을 살펴보면 이 점을 쉽게 이해할 수 있다.

> set(&#95; value:Any?, forKey:String)
>
> set(&#95; value:Bool, forKey:String)
>
> set(&#95; value:Double, forKey:String)
>
> set(&#95; value:Float, forKey:String)
>
> set(&#95; value:Int, forKey:String)

<br>

### 2.1.2 UserDefaults 객체를 통한 데이터 처리

UserDefaults 객체의 다양한 메소드를 사용하여 기본 저장소에 데이터를 읽고 쓰는 방법을 알아 보자

다음은 UserDefaults 객체를 통해 데이터를 저장하고 읽어들이는 예제이다.

```swift
let plist = UserDefaults.standard

//데이터를 저장할 때
plist.set("민경준", forkey: "이름")
plist.set(26, forkey: "나이")
plist.set("남", forkey: "성별")

plist.synchronize()

//데이터를 읽어올 때
let name = plist.string(forkey: "이름") // String 타입으로 반환
let age = plist.integer(forkey: "나이") // Integer 타입으로 반환
let sex = plist.object(forkey: "성별") as? NSString // Any -> NSString 타입으로 반환
```

<br>

UserDefaults 객체 덕분에 우리는 데이터를 읽거나 쓰기 위해 프로퍼티 리스트 파일을 직접 참조할 필요가 없다. standard 속성을 통해 제공되는 표준 사용자 기본 저장소에 데이터를 저장하거나 읽기만 하면 될 뿐이다.

UserDefaults를 통해 읽어들인 데이터는 옵셔널 타입으로 처리된다. 이것은 데이터를 읽어오는 과정에서 nil 값이 반환될 가능성이 있기 때문이다.

- 입력된 키에 해당하는 데이터가 없을 때
- nil값으로 저장 되어 있을 때
- 원하는 타입으로 캐스팅하지 못하고 실패했을 때

<br>

UserDefaults **객체는 인메모리 캐싱(In-memory Caching)** 매커니즘을 사용한다. 실제 저장된 위치에서 데이터를 매번 새로 읽어들이는 것이 아니라 성능상 유리하도록 한번 읽어들인 데이터를 메모리에 캐싱해 두고 재사용하는 것을 의미한다. 

인메모리 캐싱은 성능의 향상을 가져오지만, 이로 인해 기본 저장소와 메모리 간에 서로 데이터가 일치 하지 않을 가능성이 있다. 이를 현장 용어로 **싱크(Sync)가 맞지 않는다** 라고 이야기하고, 또 다른 말로 **데이터가 서로 동기화 되지 않았다** 라고도 한다.

이 같은 상황을 방지하기 위해 캐싱된 데이터를 갱신하여 양쪽의 데이터를 일치시켜 주어야 한다. 이를 동기화 처리, 또는 싱크 처리 라고 부르고, 위 예제에서 사용된 syncronize() 메소드가 이 역할을 한다.



# 2.2 UserDefaults를 사용한 데이터 저장 실습

테이블 뷰의 Content 속성을 Static Cells로 설정 한 뒤, 각 셀의 스타일을 Custom 타입으로 설정한다.

다음 그림을 참고하여 각 테이블 셀들을 구성하자.

![table-cell-object](/images/table-cell-object.png)

------

- 첫번 째 셀 : "이름" 레이블과 "민경준" 레이블
- 두번 째 셀 : "성별" 레이블과 세그먼트 컨트롤
- 세번 째 셀 : "결혼 여부" 레이블과 스위치 컨트롤



### 2.2.1 기본 코드 및 아웃렛 변수, 액션 메소드 구현하기



STEP 1. 

커스텀 클래스인 ListViewController 를 만들어 사용자 정보 화면과 연결 시킨다.

![live-view-controller-1](/images/list-view-controller-1.png)

![live-view-controller-2](/images/list-view-controller-2.png)

------



STEP 2.

각 컨트롤에 아웃렛 변수 및 액션 메소드를 연결한다.

![connect-outlet-action-1](/images/connect-outlet-action—1.png)

![connect-outlet-action-2](/images/connect-outlet-action—2.png)

------

<br>

### 2.2.2 이름 편집 기능 구현



STEP 1.

ListViewController 클래스에 tableView(_:didSelectRowAt:) 메소드를 추가한다.

레이블에는 액션 메소드를 직접 연결할 수 없기 때문에 그 대안으로 테이블 셀을 이용해야 한다. 테이블 셀 역시 액션 메소드를 직접. 연결 할 수는 없지만, 델리게이트 메소드를 통해 비슷한 결과를 만들어 낼 수 있다.

![add-table-view](/images/add-table-view.png)

------



STEP 2.

tableView(_:didSelectRowAt:) 메소드에 다음과 같은 내용을 작성한다.

![edit-table-view](/images/edit-table-view.png)

------



여기서 .addTextField() 메서드의 $0 은 메서드의 원본에는 존재하는 매개변수 configurationHandler 의 값을, UIAlertAction(title:style:) 메서드의 (_) in 은 매개변수 handler 의 값을 클로저 방식으로 표현한것이다. 

![add-text-field](/images/add-text-field.png)

![ui-alert-action](/images/ui-alert-action.png)

------



클로저에 관한 내용은 아래의 링크를 참고하는것이 좋다.

[Swift 클로저 및 고차 함수 이해하기]: https://academy.realm.io/kr/posts/closure-and-higher-order-functions-of-swift/

<br>

### 2.2.3 데이터 저장 및 불러오기

필요한 화면과 기본 기능이 모두 구현되었으므로, 마지막 과정으로 프로퍼티 리스트에 연동해 보자.



STEP 1.

changeGender(_:) 에 "gender"를 키로 하는 데이터 저장 구문을 작성한다.

![edit-change-gender](/images/edit-change-gender.png)

------



STEP 2.

changeMarried(_:) 메소드도 동일한 방식으로 "married"를 키로 하는 데이터 저장 구문을 작성한다.

![edit-change-married](/images/edit-change-married.png)

------



STEP 3. 

tableView(_:didSelectRowAt:) 메소드의 비어 있는 버튼 액션에도 "name" 을 키로 하여 값을 저장하는 구문을 작성한다.

![edit-change-name](/images/edit-change-name.png)

------



alert.textField?[0].text 에서 textField가 배열 방식인 이유는 UIAlertController 객체가 가질 수 있는 텍스트 필드의 개수가 하나 이상이기 때문이다. 또, textField 속성을 옵셔널 타입으로 정의되어 있는 이유는 alert 창에 입력폼이 하나도 없을 경우, textFields 속성은 기본 값으로 nil 값을 가지게 되기 때문이다.

![text-field-default-nil](/images/text-field-default-nil.png)

------



STEP 4.

알림창의 버튼 액션 내부에 코드 한 줄을 추가 한다.

![add-text-value](/images/add-text-value.png)

------



다음과 같이 작동하는 것을 확인 할 수 있다.

![change-name-test-1](/images/change-name-test-1.png)

![change-name-test-2](/images/change-name-test-2.png)

![change-name-test-3](/images/change-name-test-3.png)

------



STEP 5.

ListViewController 클래스에 viewDidLoad 메소드를 구현하고, 다음과 같이 저장된 데이터를 불러오는 구문을 작성한다. 불러온 데이터는 각 컨트롤의 속성에 맞게 설정한다.

![add-view-did-load](/images/add-view-did-load.png)

------



STEP 6.

값을 변경 후 앱을 종료하고 다시 실행 했을 때 값이 유지가 되는지 확인한다.

![change-object-test-1](/images/change-object-test-1.png)

![change-object-test-2](/images/change-object-test-2.png)

![change-object-test-3](/images/change-object-test-3.png)

------



위와 같이 잘 작동하는것을 확인 할 수 있다. 