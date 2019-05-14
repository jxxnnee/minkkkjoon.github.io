---
title: “컬렉션 타입(Collection Types) - 딕셔너리(Dictionaries)“
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
  - Dictionaries
---



## *딕셔너리(Dictionaries)*

딕셔너리는 같은 타입의 여러 값을 저장하고 있는 컨테이너입니다. 
사전처럼 “Key : Value” 구조로 이루어져 있습니다. 우리가 사전에서 Apple의 뜻을 찾는다고 할 때, Apple 이라는 단어가 Key이고, Apple의 뜻은 Value 라고 할 수 있습니다.

Swift의 딕셔너리는 Objective-C의 NSDictionary와 NSMutableDictionary 클래스와는 다름. Swift의 배열과 마찬가지로 딕셔너리는 정해진 타입의 객체만 저장이 가능함.{: .notice--info}
<br><br>



### 축약 딕셔너리 타입 문법(Dictionary Type Shorthand Syntax)

Swift 딕셔너리 타입은`Dictionary<KeyType, ValueType>`으로 쓰며, `KeyType`은 딕셔너리 키로 사용되는 값 타입 입니다.
`ValueType`은 딕셔너리에 저장되는 값으로 키에 매칭되는 값 타입 입니다.

딕셔너리 타입의 축약 문법은 `[KeyType: ValueType]`
<br><br>





### Dictionaries 기본 형태

```swift
// Dictionary 타입을 추론하여 변수의 타입을 지정할 수 있다.
var myDict = ["Germany":1, "France":2, "Sweden":3, "Egypt":4]

// 자료형 지정
var myDict: [String:Int]

// 자료형 지정 및 초기화
var myDict: [String:Int]()

// 다른 형태
var myDict = Dictionary<String, Int>()
```

<br><br>



### Dictionary  접근과 수정(Accessing and Modifying a Dictionary)

딕셔너리는 메소드와 속성 또는 서브스크립트 문법을 통해 접근과 수정이 가능합니다. 
딕셔너리에서 아이템 갯수를 찾을 때 읽기 전용 count 속성을 통해 확인할 수 있습니다.

```swift
println("The airports dictionary contains \(myDict.count) items.")
// "The airports dictionary contains 4 items." 가 출력된다.
```

<br><br>



isEmpty 속성을 통해 빈 딕셔너리인지 확인이 가능합니다.

```swift
if myDict.isEmpty {
	println("myDict 딕셔너리는 비어있습니다.")
} else {
	println("myDict 딕셔너리는 비어있지 않습니다.")
}
// "myDict 딕셔너리는 비어있지 않습니다." 가 출력된다.
```

<br>



서브스크립트 문법을 통해 딕셔너리에 새로운 아이템을 추가 할 수 있습니다.

```swift
myDict["England"] = 5
// Key : "England" , Value : 5 인 아이템이 추가되었다.
```

<br>

서브스크립트 문법을 사용하여 특정 키에 연관된 값을 변경할 수 있습니다.

```swift
myDict["England"] = 6
// "England" 의 Value 5 가 6 으로 변경되었다.
```

<br>



또한, 서브스크립트 문법을 통해 값을 확인할 수 있는데 이때 값이 없다면 nil을 반환합니다.

```swift
if let countryNumber = myDict["Sweden"] {
	println("The number of the country is \(countryNumber).")
} else {
	println("That country is not in the country dictionary.")
}
// "The number of the country is 3." 이 출력된다.
```

<br>



서브스크립트 문접을 통해 딕셔너리에 할당된 값을 nil로 할당하여 아이템을 제거할 수 있습니다.

```swift
myDict["China"] = 5

myDict["China"] = nil
// Key값이 China인 아이템을 삭제했다.
myDict = [:]	
// 완전 제거
```

<br><br>



### Dictionary 값 가져오기 

Array 때와 마찬가지로 for-in 반복문을 사용하여 각각의 값들을 가져올 수 있습니다.
각각의 딕셔너리의 아이템은 `(key, value)` 튜플로 반환되는데 일시적인 상수나 변수로 튜플의 멤버로 분리할 수 있습니다.

```swift
for (countryName, countryCode) in myDict {
	println("\(countryName), \(countryCode)")
}
// Germany : 1
// France : 2
// Sweden : 3
// Egypt : 4
// England : 6

for country in myDict {
	print("\(country)")
}
// ("Germnay", 1), ("France", 2), ("Sweden", 3), ("Egypt", 4), ("England", 6) 튜플 형식이 출력된다.  
```

<br>



딕셔너리의 `keys`와 `values` 속성을 가지고 접근한 키나 값의 컬렉션을 반복하여 검색할 수 있습니다.

```swift
for countryName in myDict.keys {
	println("Country Name : \(countryName)")
}
// Country Name : Germany
// Country Name : France
// Country Name : Sweden
// Country Name : Egypt
// Country Name : England

for countryCode in myDict.values {
	println("Country Code : \(countryCode)")
}
// Country Code : 1
// Country Code : 2
// Country Code : 3
// Country Code : 4
// Country Code : 6

```

<br>



### 빈 딕셔너리 생성(Creating an Empty Dictionary)

배열처럼 딕셔너리도 초기화 문법을 사용하여 특정 타입의 빈 딕셔너리를 만들 수 있음.

```
var namesOfIntegers = [Int: String]()
// namesOfIntegers is an empty [Int: String] dictionary
```

배열과 마찬가지로 이미 타입이 정해져 있는 딕셔너리를 빈 딕셔너리로 초기화하여도 타입은 그대로 유지됩니다. 빈 딕셔너리 표현법은 `[:]`로 사용합니다.

```
namesOfIntegers[16] = “sixteen”
// namesOfIntegers now contains 1 key-value pair
namesOfIntegers = [:]
// namesOfIntegers is once again an empty dictionary of type [Int: String]
```

<br><br><br>

