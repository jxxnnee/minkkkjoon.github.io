# 1. Stack (스택)

<br>
**스택은 어디에나 있다. 쌓을 수 있는 것들을 몇 가지 예를 들어보자**

- **팬 케이크**

- **책**

- **종이**

- **현금**

**스택의 데이터 구조는 개념상 물체의 물리적 스택과 동일하다.**
**스택에 항목을 추가 할 때는 항상 맨 위에, 제거할 때도 항상 최상위 항목을 제거한다.**

Stacks are everywhere. Here are some common examples of things you would stack:

- pancakes

- books

- paper

- cash

The stack data structure is identical, in concept, to a physical stack of objects. 

When you add an item to a stack, you place it on top of the stack. 

When you remove an item from a stack, you always remove the top-most item.



<br>

## Stack operations

**스택은 유용하고 간단하다. 스택 구축의 주요 목표는 데이터에 어떻게 엑세스할지 정하는 것이다. 
Linked list 개념을 익히는데 어려움을 겪었다면, 스택이 비교적 사소함을 알게 되어 기쁠것이다.**

Stacks are useful, and also exceedingly simple. 
The main goal of building a stack is to enforce how you access your data. 
If you had a tough time with the linked list concepts, you’ll be glad to know that stacks are comparatively trivial.

<br>

**스택에 필요한 작업은 두 가지 뿐이다 :**

- **push : 스택의 최상위 요소 추가**

- **pop : 스택의 최상위 요소 제거**

There are only two essential operations for a stack:

- push: Adding an element to the top of the stack.

- pop: Removing the top element of the stack.

<br>

**즉, 데이터 구조의 한쪽 부분에서만 데이터를 추가하고 제거할 수 있다는 뜻이다.
컴퓨터 과학에서 스택은 LIFO(last-in first-out) 데이터 구조로 알려져 있다.
마지막에 추가한 요소가 가장 먼저 튀어나온다.**

This means that you can only add or remove elements from one side of the data structure. 
In computer science, a stack is known as a LIFO (last-in first-out) data structure. Elements that are pushed in last are the first ones to be popped out.

<br>

**스택은 프로그래밍의 모든 분야에서 두드러지게 사용된다. 몇 개 나열하자면**

- **iOS는 네비게이션 스택(pop and push)을 사용하여 뷰 컨트롤러를 화면에 띄우거나 제거한다.**

- **아키텍쳐 수준에서 스택을 사용하여 메모리를 할당한다. 로컬 변수에 대한 메모리도 스택을 사용하여 관리한다.**

- **미로를 빠져나온 경로를 탐색하는 알고리즘 같은 것에 숙달된다면 스택을 사용하여 역추적 하는것이 가능하다.**

Stacks are used prominently in all disciplines of programming. 
To list a few:

- iOS uses the navigation stack to push and pop view controllers into and out of view.

- Memory allocation uses stacks at the architectural level. Memory for local variables is also managed using a stack.

- Search and conquer algorithms, such as finding a path out of a maze, use stacks to facilitate backtracking.



<br>

## Implementation

**Stack.swift 라는 이름의 Playground를 생성하여 다음 내용을 입력하자.** 

```swift
public struct Stack<Element> {
		private var storage: [Element] = []
		public init() { }
}
extension Stack: CustomStringConvertible {
    public var description: String {
        return
        """
        ----top----
        \(storage.map { "\($0)" }
        		.reversed()
                .joined(separator: "\n"))
        -----------
        """

    }
}
```

<br>

**여기서는 스택의 백업 저장소를 정의했다. 스택에 맞는 스토리지 유형을 선택하는 것이 중요하다.**

**이 배열은 append 와 popLast를 통해서 한 쪽 끝에서만 지속적으로 삽입과 삭제가 이루어지기 때문에 정확한 선택이다. 이 두 가지 작업을 사용하면 스택의 LIFO(Last In First Out) 특성이 용이해진다.**

Here, you’ve defined the backing storage of your Stack. Choosing the right storage type for your stack is important. 

The array is an obvious choice, since it offers constant time insertions and deletions at one end via append and popLast. Usage of these two operations will facilitate the LIFO nature of stacks.

<br>

**CustomStringConvertible의 고급 기능 호출 체인에 대해서는 세 가지 일을 하고 있다.**

1. **저장소를 통해 요소를 문자열에 매핑하는 배열 생성. map { "\\($0)" }**
2. **reverse()를 사용하여 이전 배열을 반전시키는 새로운 배열을 생성한다.**
3. **join(separator:)을 사용하여 배열을 문자열로 평평하게 한다.**
**("\n"을 사용하여 배열 요소를 한줄로 세운다.)**

> CustomStringConvertible은 문자열을 커스터마이징 할 때 사용하는 프로토콜이다.
> 필수 함수로 description: String { get }을 사용하고 있다.

For the fancy chain of function calls in CustomStringConvertible, you are doing three things:
1. Creating an array that maps the elements to String via storage.map { "\\($0)" }
2. Creating a new array that reverses the previous array using reversed().
3. Flattening out the array into a string by using joined(separator:). 
You separate the elements of the array using the newline character "\n".

This will allow you to customize a human-readable representation of the Stack that you’ll be using.



<br>

<br>

## Push and Pop Operations

**Stack 구조체에 다음 코드를 추가한다.**

Add the following two operations to your Stack:
```swift
public mutating func push(_ element: Element) {
	storage.append(element)
}
@discardableResult
public mutating func pop() -> Element? {
	return storage.popLast()
}
```



**그리고 다음 코드를 가장 아래에 추가한다.**

Fairly straightforward! In the playground page, write the following:
```swift
example(of: "using a stack") {
    var stack = Stack<Int>()
    stack.push(1)
    stack.push(2)
    stack.push(3)
    stack.push(4)
    print(stack)
    
    if let poppedElement = stack.pop() {
        assert(4 == poppedElement)
        print("Popped: \(poppedElement)")
    }
}
```

<br>
**실행해보면 다음과 같은 결과를 얻을 수 있다.**

You should see the following output:
```
---Example of using a stack---
----top----
4
3
2
1
-----------
Popped: 4
```

**Push와 Pop 둘 다 O(1)의 시간 복잡성을 가진다.**

push and pop both have a O(1) time complexity.



<br>

<br>

## Non-essential Operations

**스택을 좀 더 편하게 해주는 두 가지의 작업이 있다. 다음 코드를 추가하자.**

There are a couple of nice-to-have operations that make a stack easier to use. In Stack.swift, add the following to Stack:

```swift
public func peek() -> Element? {
	return storage.last
}
public var isEmpty: Bool {
	return peek() == nil
}
```
<br>

**peek은 흔히 말하는 스택 인터페이스 작업이다. 엿보자는 생각은 내용물을 변형시키지 않고 스택의 최상위 요소를 살펴보는 것이다.**

peek is an operation that is often attributed to the stack interface. The idea of peek is to look at the top element of the stack without mutating its contents.

<br>

<br>

## Less is More

**스택에 Swift Collection Protocol을 적용할 수 있는지 궁금할 수 있다.** 

**스택의 목적은 데이터에 접근하는 경우의 수를 제한하는 것이며, Collection과 같은 프로토콜을 채택하는 것은 반복자와 구독을 통해 모든 Element를 노출시킴으로써 이 목표에 어긋나게 될것이다.**

**이 경우에는 적을 수록 더 좋다!** 

**액세스 순서가 보장되도록 기존 배열을 가져와 스택으로 변환하는 것이 좋을 수 있다. 물론 배열 요소를 loop 하거나 각 요소를 push 할 수 있다.**

You may have wondered if you could adopt the Swift collection protocols for the stack. 

A stack’s purpose is to limit the number of ways to access your data, and adopting protocols such as Collection would go against this goal by exposing all the elements via iterators and the subscript. 

In this case, less is more! 

You might want to take an existing array and convert it to a stack so that the access order is guaranteed.  Of course it would be possible to loop through the array elements and push each element.



<br>

**그러나 기본적인 프라이빗 스토리지를 설정하는 Initializer를 작성할 수 있으므로 Stack 구현사항에 다음 코드를 추가한다.**

However, since you can write an initializer that just sets the underlying private storage. Add the following to your stack implementation:

```swift
public init(_ elements: [Element]) {
	storage = elements
}
```
<br>

**이제, 이 example을 메인 playgrounddp 추가한다.**

Now, add this example to the main playground:

```swift
example(of: "initializing a stack from an array") {
    let array = ["A", "B", "C", "D"]
    var stack = Stack(array)
    print(stack)
    if let poppedElement = stack.pop() {
        print("Popped: \(poppedElement)")
    }
}
```

<br>

**이 코드는 문자열 스택을 만들고 최상위 요소인 "D"를 pop 한다.**

**Swift 컴파일러는 배열에서 요소 유형을 추론할 수 있으므로 더 번거로운 Stack&#60;String&#62; 대신 Stack을 사용할 수 있다는 점에 유의하자.**

This code creates a stack of strings and pops the top element "D".

Notice that the Swift compiler can type infer the element type from the array so you can use Stack instead of the more verbose StackStack&#60;String&#62;.

<br>

**한 단계 더 나아가서 배열 리터럴에 초기화 가능한 스택을 만들 수 있다.**
**다음 코드를 Stack 구현에 추가하자.**

You can go a step further and make your stack initializable from an array literal. 
Add this to your stack implementation:

```swift
extension Stack: ExpressibleByArrayLiteral {
	public init(arrayLiteral elements: Element...) {
	storage = elements
	}
}
```

<br>

**이제, 메인 Playgroud로 돌아가서 다음 코드를 추가하자.**

Now go back to the main playground page and add:
```swift
example(of: "initializing a stack from an array literal") {
    var stack: Stack = [1.0, 2.0, 3.0, 4.0]
    print(stack)
    if let poppedElement = stack.pop() {
        print("Popped: \(poppedElement)")
    }
}
```

<br>

**이 코드는 Double 유형의 Stack을 만들고 최상위 값 4.0을 pop 한다.
다시 말하지만, type 추론을 사용하면 더 번거로운 StackStack&#60;Double&#62;을 사용하지 않아도 된다.**

This creates a stack of Doubles and pops the top value 4.0. 
Again, type inference saves you from having to type the more verbose StackStack&#60;Double&#62;

<br>

**Stack은 Tree와 Graph를 검색하는 문제에 매우 중요하다. 미로를 통해 길을 찾는다고 상상해보자.**

**당신이 왼쪽, 오른쪽 또는 직선으로 향하는 결정점에 도달할 때마다, 가능한 모든 결정을 Stack에 밀어 넣을 수 있다. 막다른 골목에 다다랐을 때, 스택에서 튀어나온것에 의해서 탈출하거나 막다른 골목에 부딫힐 때까지 계속 단순히 되짚어 갈 뿐이다.**

Stacks are crucial to problems that search trees and graphs. Imagine finding your way through a maze. 

Each time you come to a decision point of left, right or straight, you can push all possible decisions onto your stack. When you hit a dead end, simply backtrack by popping from the stack and continuing until you escape or hit another dead end.

<br>

<br>

## Key Points

**• 스택은 LIFO(last-in first-out) 유형의 데이터 구조이다.**

**• 매우 단순함에도 불구하고 많은 문제에 대한 핵심 데이터 구조이다.**

**• 스택에 필요한 두 가지 필수 작업은 요소를 추가하는 Push와 요소를 제거하는 Pop 뿐이다.**

• A stack is a LIFO, last-in first-out, data structure.

• Despite being so simple, the stack is a key data structure for many problems.

• The only two essential operations for the stack are the push method for adding elements and the pop method for removing elements.

<br>
<br>

## 더 알아보자

#### 1. Collection protocol

Collection은 일반적인 “집합 컨테이너”를 묘사하는 프로토콜인데, 실질적으로는 Sequence 프로토콜을 상속하면서 한 가지 개념(기능)을 추가한 것으로 이해할 수 있다. 그것은 임의의 인덱스를 통해서 개별 원소를 액세스할 수 있는 점이다. 따라서 Sequence와 달리 여러번이고 순회할 수 있고, 순회 시 내부 자료가 소모되지 않는다.

Collection 내의 모든 원소는 집합 구조 내에서 자신의 위치를 특정지을 수 있는 인덱스를 갖는다. 그런 동시에 Sequence를 상속하기도 했다. 하지만 이것은 Collection이 되기 위해 Sequence가 되어야 한다는 뜻은 아니다. Collection이 되면 자동으로 Sequence의 동작을 모두 수행할 수 있다는 것이다.

<https://soooprmx.com/archives/7049>

<br>

#### 2. Initializer

Initialize는 초기화라는 뜻이다. 여기서 초기화는 모두 0 값으로 보내버리는 그런 초기화가 아니고 **변수나 상수의 초기값을 정해준다**는 의미이다. 

<https://m.blog.naver.com/jdub7138/220379745883>

<br>

#### 3. Array Literal

여기서 리터럴은 어떠한 값을 명칭하는 것이 아니라 변수 및 상수에 저장되는 **'값 자체'**를 일컫는 말이다. 정수 리터럴, 문자열 리터럴, 배열 리터럴 등.. 언어의 한 요소로서 리터럴이라고 불린다.

조금 더 풀어서 설명하자면

변수나 상수는 메모리에 할당된 '공간' 에 대한 특징을 이야기 하는것이고
리터럴은 이 공간에 저장되는 '값' 자체를 의미한다. (Int, String, Char, Float...)

리터럴은 상수와 마찬가지로 메모리 어딘가에 값이 변하지 않도록 저장이되지만 그 이름이 없다.
즉, 리터러이란 컴파일시 프로그램 내에 정의되어있는 그대로 정확히 해석되어야 할 값을 의미한다.

<https://asfirstalways.tistory.com/21>