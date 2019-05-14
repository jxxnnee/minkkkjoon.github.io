---
title: “컬렉션 타입(Collection Types) - 집합(Set)“
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
  - Set
---



# *집합(Set)*

Set 은 집합입니다. 수학에서의 집합과 마찬가지로 여러 값 들의 묶음을 말하며, 이 값들은 순서가 존재하지 않고 동일한 값은 하나의 값으로 합쳐집니다.
<br><br>



### Set의 기본형태

Set에 들어갈 수 있는 값들은 String, Int, Bool 등으로서 자체적으로 Hash Value를 생성할 수 있는 (Hashable) 자료형들 입니다.
Hash Value는 자기 자신의 값을 Int로 표현하는 것으로 a == b 를 비교하는데 쓰이는 값 입니다. Set에 들어갈 수 있는 값들이 Hashable 해야하는 이유는 바로 이렇게 값 비교가 가능하여 동일한 중복값이 오지 못하게 막을 수 있어야 하기 때문입니다.

```swift
let myValue = true
myValue.hashValue
// 1 출력

let myInt = 5
myInt.hashValue
// 5 출력

1.hashValue == true.hashValue
// true

let myString = "Hello"
myString.hashValue
// 47994633090349462328
```

<br>



빈 Set 생성

```swift
var emptySet = Set<String>()
var emptySet2 : Set<String> = []
```

<br>



Set의 형태는 Array와 동일합니다. 
이에 Set을 만들 때에는 아래처럼 Set 자료형을 반드시 표기해야 합니다.

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip Hop", "Rap", "Ballade"]
```

<br><br>



### 집합에 속한 엘리먼트에 접근 하기

여러 메소드들을 이용하여 집합에 속한 엘리먼트들에 접근하여 삭제 추가 등…. 여러 작업을 할 수 있습니다.

```swift
favoriteGenres.insert("Jazz")	// 추가
favoriteGenres.remove("Rock") // 제거
favoriteGenres.contains("Funk") // 포함확인
favoriteGenres.count // 3
favortieGenres.isEmpty // false
favoriteGenres.indexOf("Hip Hop") // "Hip Hop"의 인덱스


for genre in favoriteGenres.sorted() {
		print(genre)
}
// iterate

let item = favoriteGenres.remove("Jazz")
print(item)
// 값이 존재하여 정상적으로 제거 된 경우 해당 값을 반환하고, 집합 내에 존재하지 않아 제거할 수 없는 경우에는 nil 값을 반환합니다.

if let idx = favoriteGenres.indexOf("Classical") {
		var otherItem = forceAwakens.removeAtIndex(idx)
		print(otherItem)
}
// 인덱스의 값을 이용하여 해당하는 엘리먼트를 집합에서 제거 할 수 있습니다.

for character in ["Hip Hop", "Rap"] {
		let character = favoriteGenres.remove(character)
		print(character)
}
// for문을 이용하여 한번에 여러개의 값을 추가 하거나 삭제 시킬 수 있습니다.

favoriteGenres.removeAll()
// 집합에 포함된 모든 엘리먼트를 제거 할 수 있습니다.
```

<br><br>



### Set 집합 메서드

```swift
let a: Set = [1, 3, 5, 7, 9]
let b: Set = [0, 2, 4, 6, 8]
let c: Set = [2, 3, 5, 7]

a.union(b).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9] 합집합 A u B

a.intersection(c).sorted()
// [3, 5, 7] 교집합 A n C 

a.subtracting(c).sorted()
// [1, 9] 차집합 A - C

a.symmertricDifference(c).sorted()
// [1, 2, 9] (A n C)의 여집합 : (A u C) - (A n C)
```

<br>



isSubsetOf(_:) : 집합이 파라미터의 부분집합인지 True, False로 리턴
isSupersetOf(_:) : 집합이 파라미터의 상위집합인지 True, False로 리턴
isStrictSubsetOf(_:) : 집합이 파라미터의 부분집합인지 True, False로 리턴. 단, 파라미터와 같은 집합일 경우 False리턴
isStrictSupersetOf(_:) : 집합이 파라미터의 상위집합인지 True, False로 리턴. 단, 파라미터와 같은 집합일 경우 False리턴
isDisjointWith(_:) : 집합이 파라미터와 교집합이 없을 때 True를 리턴. 교집합이 있으면 False를 리턴.

```swift
let A : Set = [1, 3, 5, 7, 9]
let B : Set = [3, 5]
let C : Set = [2, 4, 6]
let D : Set = [3, 5]

B.isSubsetOf(A) //True
B.isStrictSubsetOf(D) //False
A.isSupersetOf(B) //True
D.isStrictSupersetOf(B) //False
A.isDisjointWith(C) //True
```





참조 - [SwiftCollection Types 정리](http://minsone.github.io/mac/ios/swift-collection-types-summary)

[아이폰 개발	Swift 3 컬렉션 자료형 (Array, Dictionary, Set) : 네이버 블로그]: https://m.blog.naver.com/PostView.nhn?blogId=jdub7138&logNo=220924351499&proxyReferer=https%3A%2F%2Fwww.google.com%2F

[스위프트 - 집합 (Set) · Out of Bedlam](https://outofbedlam.github.io/swift/2016/03/26/Set/)