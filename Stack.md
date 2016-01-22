### Stack

Implement Stack with a linked list.

```swift 
struct Stack<T> {
 
  typealias Element = T
  
  private var linkeList = LinkedList<Element>()
  private var counter: UInt = 0
  
  init (elements: Element...) {
    for elemnt in elements {
      push(elemnt)
    }
  }
  
  mutating func pop() -> Element? {
    guard let res = linkeList.removeAtIndex(0) else {
      return nil
    }
    counter--
    return res
  }
  
  func peak() -> Element? {
    return linkeList.objectAtIndex(0)
  }
  
  mutating func push(data: Element) {
    counter++
    linkeList.appendToHead(data)
  }
  
  var count: UInt {
    return counter
  }
  
  var isEmpty: Bool {
    return count == 0 ? true : false
  }
}

```

 A stack which that in addition to Push, Pop and Peak, also has a function Min which returns the minimum element in the stack.
 Push, pop, Peak and Min all operate in 0(1) time.
 struct StackWithMin<T: protocol<Comparable, Hashable> > {

```swift 

struct StackWithMin<T: protocol<Comparable, Hashable> > {
  
  typealias Element = T
  private var stack = Stack<Element>()
  private var minStack = Stack<Element>()

  mutating func pop() -> Element? {
    
    let resElement = stack.pop()
    
    if resElement == minStack.peak() {
      minStack.pop()
    }
    
    return resElement
  }
  
  func peak() -> Element? {
    return stack.peak()
  }
  
  func min() -> Element? {
    return minStack.peak()
  }
  
  mutating func push(data: Element) {
    
    if let currentMin = minStack.peak() {
      if data <= currentMin {
        minStack.push(data)
      }
    } else {
      minStack.push(data)
    }
    
    return stack.push(data)
  }
}
```
