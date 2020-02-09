# Chapter 18: Tries - Kor

trie (try로 발음)는 영어 단어와 같이 컬렉션으로 표현할 수있는 데이터 저장을 전문으로하는 트리입니다.

![Untitled](/_posts/Chapter 18 Tries Kor/Untitled.png)

문자열의 각 문자는 노드에 매핑됩니다. 각 문자열의 마지막 노드는 종료 노드로 표시됩니다 (위 이미지의 점). Trie의 이점은 접두사 일치와 관련하여 살펴 보는 것이 가장 좋습니다.

이 장에서는 먼저 트라이의 성능을 배열과 비교합니다. 그런 다음 처음부터 트라이를 구현합니다!

# Example

문자열 모음이 제공됩니다. 접두사 일치를 처리하는 구성 요소를 어떻게 구축 하시겠습니까? 

한 가지 방법이 있습니다.

    class EnglishDictionary { 
    	private var words: [String] 
    	func words(matching prefix: String) -> [String] {
    		words.filter { $0.hasPrefix(prefix) }
    	}
    }

`words (matching :)`는 문자열 모음을 살펴보고 접두사와 일치하는 문자열을 반환합니다.

단어 배열의 요소 수가 적으면 합리적인 전략입니다. 그러나 수천 단어 이상을 다루는 경우 단어 배열을 통과하는 데 걸리는 시간은 용납 될 수 없습니다. `words (matching :)`의 시간 복잡도는 *O (k * n)* 입니다. 여기서 k는 모음에서 가장 긴 문자열이고 n은 확인해야 할 단어의 수입니다.

![Untitled 1](/_posts/Chapter 18 Tries Kor/Untitled 1.png)

 *Google이 구문 분석해야하는 단어의 수를 상상해보십시오* 

trie 데이터 구조는 이러한 유형의 문제에 대해 우수한 성능 특성을 갖습니다. 여러 하위를 지원하는 노드가있는 트리로서 각 노드는 단일 문자를 나타낼 수 있습니다.

검은 점으로 표시되는 특수 표시기 (터미네이터)를 사용하여 루트에서 노드로 문자 모음을 추적하여 단어를 형성합니다. 트리의 흥미로운 특징은 여러 단어가 동일한 문자를 공유 할 수 있다는 것입니다.

트라이의 성능 이점을 설명하기 위해 접두사 CU가있는 단어를 찾아야하는 다음 예를 고려하십시오.

먼저 C가 포함 된 노드로 이동합니다. 그러면 트리의 다른 분기가 검색 작업에서 빠르게 제외됩니다.

![Chapter%2018%20Tries%20Kor/Untitled%202.png](Chapter%2018%20Tries%20Kor/Untitled%202.png)

다음으로 다음 문자 U가있는 단어를 찾아야합니다. U 노드로 이동합니다.

![Chapter%2018%20Tries%20Kor/Untitled%203.png](Chapter%2018%20Tries%20Kor/Untitled%203.png)

이것이 접두사의 끝이므로 트라이는 노드 체인에 의해 형성된 모든 컬렉션을 U 노드에서 반환합니다. 이 경우 단어 CUT 및 CUTE가 반환됩니다. 이 tire에 수십만 단어가 포함되어 있다고 상상해보십시오.

트라이를 사용하여 피할 수있는 비교의 수는 상당합니다.

![Chapter%2018%20Tries%20Kor/Untitled%204.png](Chapter%2018%20Tries%20Kor/Untitled%204.png)

# 구현

항상 그렇듯이 이 장의 Starter playground 를여십시오.

# TrieNode

먼저 trie에 대한 노드를 작성하십시오. **Sources** 디렉토리에서 **TrieNode.swift** 라는 새 파일을 작성하십시오. 파일에 다음을 추가하십시오.

    public class TrieNode<Key: Hashable> {
    	// 1
    	public var key: Key?
    	// 2
    	public weak var parent: TrieNode?
    	// 3
    	public var children: [Key: TrieNode] = [:]
    	// 4
    	public var isTerminating = false
    	public init(key: Key?, parent: TrieNode?) {
    		self.key = key
    		self.parent = parent
    	}
    }

이 인터페이스는 발생한 다른 노드와 약간 다릅니다.

1. 키는 노드에 대한 데이터를 보유합니다. 트라이의 루트 노드에 키가 없기 때문에 이것은 선택 사항입니다.
2. TrieNode는 상위 노드에 대한 약한 참조를 보유합니다. 이 참조는 나중에 remove 메소드를 단순화합니다.
3. 이진 검색 트리에서 노드에는 왼쪽 및 오른쪽 자식이 있습니다. Trie에서 노드는 여러 다른 요소를 보유해야합니다. 이를 돕기 위해 어린이 사전을 선언했습니다.
4. 앞에서 설명한 바와 같이 isTerminating은 컬렉션의 끝을 나타내는 지표 역할을합니다.

TrieNode는 부모에 대한 약한 참조를 보유합니다. 이 참조는 나중에 remove 메소드를 단순화합니다.
이진 검색 트리에서 노드에는 왼쪽 및 오른쪽 자식이 있습니다. Trie에서 노드는 여러 다른 요소를 보유해야합니다. 이를 돕기 위해 children dictionary를 선언했습니다.

앞에서 설명한 것처럼 isTerminating은 컬렉션의 끝을 나타내는 지표 역할을합니다.

# Tries

다음으로 노드를 관리 할 tire 자체를 만듭니다. Sources 폴더에서 Trie.swift 라는 새 파일을 만듭니다. 파일에 다음을 추가하십시오.

    public class Trie<CollectionType: Collection> where CollectionType.Element: Hashable 
    { 
    		public typealias Node = TrieNode<CollectionType.Element>
    		private let root = Node(key: nil, parent: nil) 
    	
    		public init() {} 
    }

Trie 클래스는 `String`을 포함하여 Collection 프로토콜을 채택하는 모든 유형을 위해 작성되었습니다. 이 요구 사항 외에도 컬렉션 내부의 각 요소는 해시 가능해야합니다. 이것은 `TrieNode`에서 컬렉션의 요소를 children dictionary의 키로 사용하기 때문에 필요합니다.

다음으로 trie에 대해 insert, contains, remove and a prefix match (삽입, 포함, 제거 및 접두사 일치) 네 가지 작업을 구현합니다.

# Insert

Tries는 Collection을 준수하는 모든 유형으로 작동합니다. trie는 컬렉션을 가져와 각 노드가 컬렉션의 요소에 매핑되는 일련의 노드로 나타냅니다.

Trie에 다음 방법을 추가하십시오.

    public func insert(_ collection: CollectionType) {
    	// 1
    	var current = root 
    	
    	// 2
    	for element in collection {
    		if current.children[element] == nil {
    			current.children[element] = Node(key: element, parent: current)
    		}
    		current = current.children[element]!
    	}
    	
    	// 3
    	current.isTerminating = true
    }

진행중인 작업은 다음과 같습니다.

1. `current`는 루트 노드로 시작하는 순회 진행을 추적합니다.
2. trie는 컬렉션의 각 요소를 별도의 노드에 저장합니다. 컬렉션의 각 요소에 대해 먼저 노드가 현재 자식 사전에 있는지 확인합니다. 그렇지 않은 경우 새 노드를 만듭니다. 각 루프 동안 전류를 다음 노드로 이동합니다.
3. for 루프를 반복 한 후 current는 컬렉션의 끝을 나타내는 노드를 참조해야합니다. 해당 노드를 종료 노드로 표시합니다.

이 알고리즘의 시간 복잡도는 *O(k)*입니다. 여기서 k는 삽입하려는 컬렉션의 요소 수입니다. 새 컬렉션의 각 요소를 나타내는 각 노드를 통과하거나 생성해야하기 때문입니다.

# Contains

포함은 Insert와 매우 유사합니다. Trie에 다음 방법을 추가하십시오.

    public func contains(_ collection: CollectionType) -> Bool {
    	var current = root
    	for element in collection {
    		guard let child = current.children[element] else {
    			return false
    		}
    		current = child
    	}
    	return current.isTerminating
    }

여기에서는 삽입과 비슷한 방식으로 trie를 선회합니다. 컬렉션의 모든 요소를 검사하여 컬렉션이 트리에 있는지 확인합니다. 컬렉션의 마지막 요소에 도달하면 종료 요소여야 합니다. 그렇지 않은 경우 컬렉션이 트리에 추가되지 않았으며 찾은 것은 더 큰 컬렉션의 하위 집합 일뿐입니다.

포함의 시간 복잡도는 *O(k)*입니다. 여기서 k는 컬렉션에서 찾고있는 요소의 수입니다. 컬렉션이 트라이에 있는지 여부를 확인하려면 k 노드를 통과해야하기 때문입니다.

삽입물과 포함 물을 테스트하려면 운동장 페이지로 이동하여 다음 코드를 추가하십시오.

    example(of: "insert and contains") {
    	let trie = Trie<String>()
    	trie.insert("cute")
    	if trie.contains("cute") {
    	print("cute is in the trie")
    	}
    }

다음과 같은 콘솔 출력이 나타납니다.

    ---Example of: insert and contains---
    cute is in the trie

# Remove

trie에서 노드를 제거하는 것은 조금 더 까다롭습니다. 두 개의 다른 콜렉션간에 노드를 공유 할 수 있으므로 각 노드를 제거 할 때 특히주의해야합니다.

바로 아래에 다음 방법이 포함되어 있습니다.

    public func remove(_ collection: CollectionType) {
    	// 1
    	var current = root
    	for element in collection {
    		guard let child = current.children[element] else {
    		return
    		}
    		current = child
    	}
    	guard current.isTerminating else {
    		return
    	}
    	// 2
    	current.isTerminating = false
    	// 3
    	while let parent = current.parent,
    	current.children.isEmpty && !current.isTerminating {
    		parent.children[current.key!] = nil
    		current = parent
    	}
    }

주석 별 설명 :

1. 이 부분은 기본적으로 포함을 구현하므로 익숙해 보일 것입니다. 여기에서 컬렉션이 trie의 일부인지 확인하고 컬렉션의 마지막 노드를 가리 키도록합니다.
2. isTerminating을 false로 설정하여 다음 단계에서 루프로 현재 노드를 제거 할 수 있습니다.
3. 이것은 까다로운 부분입니다. 노드를 공유 할 수 있으므로 다른 컬렉션에 속하는 요소를 부주의하게 제거하고 싶지 않습니다. 현재 노드에 다른 자식이 없으면 다른 컬렉션이 현재 노드에 의존하지 않음을 의미합니다.

또한 현재 노드가 종료 노드인지 확인하십시오. 그렇다면 다른 컬렉션에 속합니다. 전류가 이러한 조건을 만족하는 한 계속해서 부모 속성을 역 추적하고 노드를 제거합니다.

이 알고리즘의 시간 복잡도는 *O(k)*입니다. 여기서 k는 제거하려는 컬렉션의 요소 수를 나타냅니다.

playgroud 페이지로 돌아가서 아래에 다음을 추가하십시오.

    example(of: "remove") {
    	let trie = Trie<String>()
    	trie.insert("cut")
    	trie.insert("cute")
    
    	print("\n*** Before removing ***")
    
    	assert(trie.contains("cut"))
    	print("\"cut\" is in the trie")
    
    	assert(trie.contains("cute"))
    	print("\"cute\" is in the trie")
    
    	print("\n*** After removing cut ***")
    
    	trie.remove("cut")
    	assert(!trie.contains("cut"))
    	assert(trie.contains("cute"))
    	print("\"cute\" is still in the trie")
    }

다음과 같은 콘솔 출력이 나타납니다.

    ---Example of: remove---
    
    *** Before removing ***
    "cut" is in the trie
    "cute" is in the trie
    
    *** After removing cut ***
    "cute" is still in the trie

# Prefix matching

trie에 가장 상징적인 알고리즘은 Prefix matching 알고리즘입니다. 

Trie.swift의 하단에 다음을 작성하십시오.

    public extension Trie where CollectionType: RangeReplaceableCollection {
    
    }

Prefix matching 알고리즘은이 확장 안에 있으며 CollectionType은 RangeReplaceableCollection으로 제한됩니다. 이는 알고리즘이 RangeReplaceableCollection 유형의 append 메소드에 액세스해야하기 때문에 필요합니다.

그런 다음 확장 내에 다음 방법을 추가하십시오.

    func collections(startingWith prefix: CollectionType) -> [CollectionType] {
    	// 1
    	var current = root
    	for element in prefix {
    		guard let child = current.children[element] else {
    			return []
    		}
    		current = child
    	}
    	// 2
    	return collections(startingWith: prefix, after: current)
    }


트라이에 접두사가 포함되어 있는지 확인하여 시작합니다. 그렇지 않으면 빈 배열을 반환합니다.
접두사 끝을 표시하는 노드를 찾은 후 재귀 도우미 메서드 collection(startingWith : after :)을 호출하여 현재 노드 뒤의 모든 시퀀스를 찾습니다.

다음으로 도우미 메소드의 코드를 추가하십시오.

    private func collections(startingWith prefix: CollectionType, after node: Node) 
    	-> [CollectionType] {
    
    	// 1
    	var results: [CollectionType] = []if node.isTerminating {
    		results.append(prefix)
    	}
    
    	// 2
    	for child in node.children.values {
    		var prefix = prefix
    		prefix.append(child.key!)
    		results.append(contentsOf: collections(startingWith: prefix, after: child))
    	}
    
    	return results
    }

1. 결과를 보유 할 배열을 만듭니다. 현재 노드가 종료 노드 인 경우 결과에 추가합니다.
2. 다음으로 현재 노드의 자식을 확인해야합니다. 모든 하위 노드에 대해 반복적으로 collections (startingWith : after :)를 호출하여 다른 종료 노드를 찾습니다.

collection(startingWith :)의 시간 복잡도는 *O(k * m)*입니다. 여기서 *k*는 접두사와 일치하는 가장 긴 컬렉션을 나타내고 *m*는 접두사와 일치하는 컬렉션 수를 나타냅니다.

배열의 시간 복잡도는 *O(k * n)*입니다. 여기서 *n*은 컬렉션의 요소 수입니다.

각 컬렉션이 균일하게 분산 된 대규모 데이터 집합의 경우 접두사 일치에 배열을 사용하는 것과 비교하여 성능이 훨씬 향상됩니다.

스핀을 위한 메소드를 취할 시간입니다. Playground 페이지로 다시 이동하여 다음을 추가하십시오.

    example(of: "prefix matching") {
    let trie = Trie<String>()
    trie.insert("car")
    trie.insert("card")
    trie.insert("care")
    trie.insert("cared")
    trie.insert("cars")
    trie.insert("carbs")
    trie.insert("carapace")
    trie.insert("cargo")
    
    print("\nCollections starting with \"car\"")
    let prefixedWithCar = trie.collections(startingWith: "car")
    print(prefixedWithCar)
    
    print("\nCollections starting with \"care\"")
    let prefixedWithCare = trie.collections(startingWith: "care")
    print(prefixedWithCare)
    
    }

다음과 같은 콘솔 출력이 나타납니다.

    ---Example of: prefix matching---
    
        Collections starting with "car"
        ["car", "carbs", "care", "cared", "cars", "carapace", "cargo",
        "card"]
    
        Collections starting with "care"
        ["care", "cared"]

## **키 포인트**

- 시도는 접두사 일치와 관련하여 뛰어난 성능 지표를 제공합니다.
- 개별 노드를 여러 다른 값간에 공유 할 수 있으므로 시도는 상대적으로 메모리 효율적입니다.

    예를 들어 "car", "carbs"및 "care"는 단어의 처음 세 글자를 공유 할 수 있습니다.