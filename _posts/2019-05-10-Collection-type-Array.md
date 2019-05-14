---
title: “컬렉션 타입(Collection Types) - 배열(Array)“
elements:
  - content
  - css
  - formatting
  - html
  - markup
categories:
  - Swift
tags:
  - Collection
  - Type
  - Array
---



# 컬렉션 타입(Collection Types)

Swift는 세 가지 컬렉션 타입 Array와 Dictionary, Set을 제공한다.
* Array는 같은 타입의 값을 순서대로 저장한다.
* Dictionary는 같은 타입의 값을 순서 상관없이 저장하며 유일한 식별자(Key)를 통해 값을 찾는다.
* Set은 순서가 존재하지 않고 동일한 값은 하나의 값으로 합쳐진다.
<br><br>




## *가변 컬렉션(Mutability of Collections)*
컬렉션은 만들어 진 후 컬렉션에 있는 아이템이 추가되고 재배치되고 변경되기 때문에 변수에 할당된 컬렉션은 가변적이다.
반면, 상수는 불변으로 크기나 내용은 변경할 수 없다.
<br><br>





## *배열(Array)*
배열은 같은 타입의 다중 값이 정렬된 순서로 저장한다. 배열에서 같은 값이지만 시간이 다르면 각각 다른 위치에 있다.
<br><br>





### 축약 배열 타입 문법(Array Type Shorthand Syntax)
Swift의 Array 타입은Array<SomeType>으로 작성, SomeType은 배열에 저장될 타입이다. 간단하게 쓰면[SomeType]으로 작성 가능하다.
<br><br>





### 배열 표현식(Array Literals)
배열은 하나 이상의 값을 축약 방식으로 작성하여 배열 컬렉션을 초기화 할 수 있다.
배열 표기법은 값 목록을 콤마(,)로 분리하며 한 쌍의 중괄호로 감싸여 표현한다.
```swift
[value 1, value 2, value 3]

var studyList: [String] = [“Math”, “English”]
// studyList has been initialized with two initial items
```


또한, `[String]`에 영향을 받아 타입 추론을 통해 변수에 배열을 초기화하여 할당할 수 있다.
```swift
var studyList = [“Math”, “English”]
```
<br><br>




### 배열의 접근과 수정(Accessing and Modifying an Array)

배열에 메소드와 속성 또는 아래 첨자 문법을 통해 접근 및 수정할 수 있음.
다음은 배열의 개수를 확인하는 읽기 전용 `count` 속성이다.
```swift
println(“The study list contains \(studyList.count) items.”)
// prints “The study list contains 2 items.”
```
<br>




`count `속성을 이용하지 않고 `isEmpty` 속성을 통해 빠르게 배열이 비었는지 확인이 가능하다.

```swift
if studyList.isEmpty {
	println(“The study list is empty”)
} else {
	println(“The study list is not empty”)
}
// prints “The shopping list is not empty”
```
<br>




Array 값들을 하나씩 꺼내오기 위해서는 for - in 반복문을 사용합니다.

```swift
For study in studyList {
	print(study)
}
// "Math", "English" 출력
```
<br>




각 아이템에 정수 인덱스와 그 값이 필요하다면, `enumerate` 전역 함수를 사용한다.
`enumberate` 함수는 배열에 각 아이템의 인덱스와 값으로 구성된 튜플을 반환합니다.

```swift
for (index, value) in enumerate(studyList) {
    println(“Item \(index + 1): \(value)”)
}
// Item 1: Math
// Item 2: English
```
<br>




배열에 `append` 메소드를 통해 배열 끝에 새로운 값을 추가할 수 있습니다.

```swift
studyList.append(“Science”)
// studyList now contains 3 items
```
<br>




하나 이상 적절한 값을 가진 배열을 추가 할당 연산자(+=)를 이용하여 추가할 수 있습니다.

```swift
studyList += [“History”]
// studyList now contains 4 items
studyList += [“Social”, “Arts”, “Spanish”]
// studyList now contains 7 items
```
<br>




배열에 인덱스로 접근하여 값을 얻을 수 있습니다. 또한, 인덱스로 접근하여 값을 변경할 수 있습니다.

```swift
var firstItem = studyList[0]
// firstItem is equal to “Math”

studylist[0] = "Technology"
// the first item in the list is now equal to "Technology" rather than "Math"
```
<br>


배열에 인덱스 범위를 통해 접근하여 값을 변경하거나 배열을 대체할 수 있습니다.

```swift
studyList[4...6] = ["Engineering", "Mathematics"]
// studyList now contains 6 items
// replace "Social", "Arts" and "Spanish" to "Engineering" and  "Mathematics"
```
<br>




배열의 `insert(atIndex:)`메소드를 호출하여 특정 인덱스에 삽입할 수 있습니다.

```swift
studyList.insert("Korean", at: 0)
// sutdyList now contains 7 items
// "Korean" is now the first item in the list
```
<br>




`.remove(at:)` `.removeLast()` `.popLast()` 메소드를 호출하여 특정 인덱스의 값을 제거할 수 있습니다.
모두 값을 지우면서 지우는 값을 리턴합니다. 참고로 removeLast()는 Array에 값이 없을 때 crash하며, popLast()는 nil을 리턴합니다.

```swift
studyList.remove(at: 0)
// studyList now contains 6 items
// "Korean" has just been removed

studyList.removeLast()
// studyList now contains 5 items
// "Mathematics" has just been removed
```
<br>




배열의 각 값에 동일한 함수를 적용 시키려면 `.map()` 메소드를 호출하여 사용하면 됩니다.
 `.map()` 메소드는 Array나 Dictionary와 같은 Collection Type의 각 값에 동일한 함수를 적용하여 결과를 도출할 때 사용합니다.
 `.map()`의 구조는 배열.map(적용함수)로 이루어져 있습니다.

```swift
let numbers = [1,2,3,4]
// 각 값의 제곱을 값으로 갖는 새로운 배열을 생성하고자 한다.

// 1) 반복문 for 사용
var squared = [Int]()
for num in nubers {
	squared.append(num*num)
}

// 2) map 사용
numbers.map({ (num: Int) -> Int in. eturn num * num})
number.map{ $0 * $0 } // Trailing Closure Syntax 사용해서 단순화
```
<br>




배열에서 특정 조건에 맞는 값들만 도출할 때 `.filter()`  메소드를 호출하여 사용합니다.  `.map()`의 구조와 유사하지만 Boolean 값을 리턴값으로 갖는 함수여야 합니다.

```swift
var myArray = [1,4,5,10]
// 값들 중 짝수만 도출하고자 한다.
myArray.filter({ (value: Int) -> Bool in return value % 2 == 0 })
myArray.filter { $0 % 2 == 0 } //Trailing Clouser Syntax 사용해서 단순화
```
<br>




배열을 하나의 값으로 통합시킬 때는 `.reduce()` 메소드를 호출하여 사용합니다. 메소드의 첫번째 인자값은 추가로 더해지는 값입니다.

```swift
// 1). 숫자들 통합하기
var myArray = [1,4,5,10]
myArray.reduce(0, +)
// 0+1+4+5+10 인 20
myArray.reduce(100, +)
// 100+1+4+5+10 인 120

// 2). 문자들을 통합하기
var myStrings = ["I", "Love", "You"]
myStrings.reduce("", +)
// "ILoveYou"
```
<br><br>




### 배열 생성과 초기화

초기화 문법을 사용하여 특정 타입의 빈 배열을 만듬.
```swift
var someInts = [Int]()
println(“someInts is of type [Int] with \(someInts.count) items.”)
// prints "someInts is of type [Int] with 0 items."
```
<br>




someInt 변수는 `[Int]`에 영향을 받습니다. 이는 `[Int]` 초기화의 결과물에 설정됩니다.
한번이라도 타입이 설정되면 `[]`로 초기화하여도 타입은 유지됨.

```swift
someInts.append(3)
// someInts now contains 1 value of type Int
someInts = []
// someInts is now an empty array, but is still of type [Int]
```
<br>




Swift에 배열 타입은 특정 크기와 그 크기에 기본 값으로 설정할 수 있는 생성자를 제공. 이 생성자에 몇 개의 아이템을 넣을 것인지(count 호출), 적합한 타입의 기본 값(repeatedValue 호출)을 넘겨줌.

```swift
var threeDoubles = [Double](_count: 3, repeatedValue: 0.0_)
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
```
<br>




서로 같은 타입의 배열이 있으면 덧셈 연산자(+)를 이용하여 배열을 생성. 이 배열 타입은 앞에 배열에서 영감받아 같은 타입.

```swift
var anotherThreeDoubles = [Double](_count: 3, repeatedValue: 2.5_)
// anotherThreeDoubles is inferred as [Double], and equals [2.5, 2.5, 2.5]
 
var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```
<br><br>


