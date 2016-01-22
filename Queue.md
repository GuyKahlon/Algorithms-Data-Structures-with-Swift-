### Queue

The major difference between a Queue and a Stack is the order.
Stack is LIFO - Last In Firest Out and Queue is FIFO -Firest In Firest Out.

```swift 
struct Queue<T> {
  
  typealias Element = T
  
  private var enQueueStack = Stack<Element>()
  private var deQueueStack = Stack<Element>()
  
  var count: UInt {
    return enQueueStack.count + deQueueStack.count
  }
  
  var isEmpty: Bool {
    return count == 0 ? true : false
  }
  
  mutating func enQueue(data: Element) {
    enQueueStack.push(data)
  }
  
  mutating func deQueue() -> Element? {
    
    if deQueueStack.count == 0 {
      shiftStacks()
      return deQueueStack.pop()
    } else {
      return deQueueStack.pop()
    }
  }
  
  mutating func peak() -> Element? {
    if deQueueStack.count == 0 {
      shiftStacks()
      return deQueueStack.peak()
    } else {
      return deQueueStack.peak()
    }
  }
  
  private mutating func shiftStacks() {
    while enQueueStack.count != 0 {
      let data = enQueueStack.pop()!
      deQueueStack.push(data)
    }
  }
}
```