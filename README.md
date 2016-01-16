# Algorithms Data Structures With Swift

Swift 2.1 required


### Linked List

```swift
private class LinkedListNode<T> {
  typealias Element = T
  
  let data: Element
  var next: LinkedListNode? 
  var previous: LinkedListNode?
  
  init(data: Element){
    self.data = data
  }
}


struct LinkedList<T> {
  
  typealias Element = T
  private var head: LinkedListNode<Element>?
  
  init (elements: Element...) {    
    if let first = elements.first {
      var node = LinkedListNode(data: first)
      head = node
      for x in elements[1..<elements.endIndex] {
        let next = LinkedListNode(data: x)
        node.next = next
        node = next
      }
    } 
  }
  
  var isEmpty: Bool {
    return head == nil ? true : false
  }
}

```

Add new elemnt to Tail / Head:

```swift
  mutating func appendToTail(data: Element) {
    
    let newNode = LinkedListNode<Element>(data: data)
    
    guard let head = head else {
      self.head = newNode
      return
    }
    
    var runner = head
    
    while let next = runner.next {
      runner = next
    }
    
    runner.next = newNode
  }
  
  mutating func appendToHead(data: Element) {
    
    let newNode = LinkedListNode<Element>(data: data)
    newNode.next = head
    head = newNode
  }
  ```
Count the numbers of elements in a Linked List:
```swift
  var count: UInt {
    var counter: UInt = 0
    var runner = head
    while let current = runner {
      counter++
      runner = current.next
    }
    return counter
  }
```
Return the element at index `i`:
```swift
  func objectAtIndex(index: UInt) -> T? {
    var runner = head
    var counter: UInt = 0
    
    var retValue: LinkedListNode<T>? = nil
    
    while let current = runner {
      if counter == index {
        retValue = current
        break
      } else {
        counter++
        runner = current.next
      }
    }
    
    return retValue?.data
  }
 ```
 
Remove and return the element at index `i` 
```swift
  mutating func removeAtIndex(index: UInt) -> Element? {
    
    var runner = head
    var pre: LinkedListNode<Element>?
    var counter: UInt = 0
    var retValue: LinkedListNode<Element>? = nil
    
    while let current = runner {
      if counter == index {
        if let pre = pre {
          pre.next = current.next
        } else {
          head = current.next
        }
        retValue = current
        break
      } else {
        pre = current
        counter++
        runner = current.next
      }
    }
    
    return retValue?.data
  }
```

Reverse Linked List:

```swift
  mutating func reverse() {
    head = reverseHelper(head)
  }
  
  private func reverseHelper(node: LinkedListNode<T>?) -> LinkedListNode<T>? {
    
    //First we have to handle the base case, if the the linked list is nil or the next is nil
    guard let tail = node, let next = tail.next else {
      return node
    }
    
    //Otherwise, do it in a recursive manner
    let nextReverse = reverseHelper(next) //next now is node.next
    tail.next?.next = tail //node.next now is the tail of reversed remaining, tuns the next can point to node
    tail.next = nil        //Don't forget to update node's next to nil in order to avoid cycle.
    
    return nextReverse
  }
  ```
  
  
Return the kth to last element of a linked list.
Note: We have defined k such that passing in k = 1 would return the last element, k = 2 would return to the second to last element, and so on. 
It is equally acceptable to define k such that k = 0 would return the last element.

Iterative:
```swift
  func findKthLast(k: UInt) -> T? {
    
    if isEmpty || k < 1{
      return nil
    }
    
    let size = count
    
    if k > size {
      return nil
    }
    
    let counter = size - k
    var runner = head
    
    for _ in 0..<counter {
      runner = runner?.next
    }
    
    return runner?.data
  }
  ```
  Recursive #1:
  
  ```swift
    func findKthLastRecursive(k: Int) -> Element? {
    
    var counter = 0
    return findKthLastHelper(head, k: k, counter: &counter)?.data
  }
  
    private func findKthLastHelper(head: LinkedListNode<T>?, k: Int, inout counter: Int) -> LinkedListNode<T>? {
    
    guard let currentNode = head else {
      return nil
    }
    
    
    let node = findKthLastHelper(currentNode.next, k: k, counter: &counter)
    
    counter = counter + 1
    
    if counter == k {
      return head
    }
    
    return node
  }
```
  Recursive #2: 
```swift  
  func findKthLastRecursive(k: Int) -> Element? {
    
    var node: LinkedListNode<T>? = nil
    findKthLastHelper(head, k: k, element: &node)
    return node?.data
  }
  
    /**
   This method recurses through the linked list. When it hits the end of the linked list, the method passes back a counter set to 0, Each parent call adds 1 to this counter. When the counter equals, we know we have reached the kth to last element of the linked list.
   
   - parameter node:    the current node
   - parameter k:       the kth element we want to find.
   - parameter element: inout optional parameter, this allows us to return the node value.
   
   - returns: current counter, each parent call adds 1 to this counter.
   
   
   Time complexity
   This function takes O(N) time
   
   Space complexity
   This function takes O(1) space due to the recursive calls
   
   */
  private func findKthLastHelper(node: LinkedListNode<T>?, k: Int, inout element: LinkedListNode<T>?) -> Int {
    
    guard let currentNode = node else {
      return 0
    }
    
    let counter = findKthLastHelper(currentNode.next, k: k, element: &element) + 1
    
    if counter == k {
      element = node
    }
    
    return counter
  }
  ```
