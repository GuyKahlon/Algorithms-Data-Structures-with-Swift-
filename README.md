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
