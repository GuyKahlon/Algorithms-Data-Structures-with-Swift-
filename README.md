# Algorithms Data Structures With Swift

Swift 2.1 required

[Algorithms](https://github.com/GuyKahlon/Algorithms-Data-Structures-with-Swift-/blob/master/Algorithms.md)

[BinaryTree](https://github.com/GuyKahlon/Algorithms-Data-Structures-with-Swift-/blob/master/BinaryTree.md)
[Graph](https://github.com/GuyKahlon/Algorithms-Data-Structures-with-Swift-/blob/master/Graph.md)

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
  
Delete element from a Linked List (Remove all the elements that equals to the input element)

```swift 
extension LinkedList where T: Equatable {
  
  mutating func delete(data: Element) {
    
    while let current = head where current.data == data {
      head = current.next
    }
    
    var pre = head
    var runner = head?.next
    
    while runner != nil {
      if runner!.data == data {
        pre!.next = runner!.next
      } else {
        pre = runner
      }
      
      runner = runner!.next
    }
  }
}
```

Remove duplicates from a linked list
```swift 
  /**
   In order to remove duplicates from a linked list, we should to be able to track duplicates.
   A simple dictionary will work great here.
   
   This function, simply itrate through the linked list, adding each element to the dictionary, When we discover a duplicate element, we remove the element and continue iterating.
   We do this all in one pass since we are using a linked list.
   
   Time complexity
   This function takes O(N) time, where N is the number or elements in the linked list.
   
   Space complexity
   This function takes O (N) space, In the worst case scenario we iterate through a linked list without duplicates, and we will insert all the linked list elements to the Dictionary.
   
   
   Note: Insert and chaek opration in Dictionary take O(1)
   
   */
  func removeDuplicates() {
    
    var cacheDictionary = Dictionary<T, Bool>()
    
    var runner = head
    var pre: LinkedListNode<T>?
    
    while let current = runner {
      if let isExist = cacheDictionary[current.data] where isExist == true {
        pre?.next = current.next
      } else {
        cacheDictionary[current.data] = true
        pre = current
      }
      runner = current.next
    }
  }
```

Remove duplicates from a linked list without additional space

```swift 
extension LinkedList where T: Equatable {

  func removeDuplicatesWithoutBuffer() {
    
    guard var current: LinkedListNode<T>? = head else {
      return
    }
    
    while let currentElement = current {
      
      var runner = currentElement
      
      while let nextElement = runner.next {
        if runner.next?.data == currentElement.data {
          runner.next = nextElement.next
        } else {
          runner = nextElement
        }
      }
      
      current = currentElement.next
    }
  }
}
```
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

### Binary Tree
```swift 
private class TreeNode<T> {
  
  typealias Element = T
  
  let data: Element
  var left:  TreeNode<Element>?
  var right: TreeNode<Element>?
  
  var isLeaf: Bool {
    return left == nil && right == nil
  }
  
  init(data: Element) {
    self.data = data
  }
}

struct Tree<T> {
  
  typealias Element = T
  
  private var root: TreeNode<Element>
  
  init(rootData: Element) {
    root = TreeNode(data: rootData)
  }
  
  func insertLeftData(data: Element) {
    root.left = TreeNode(data: data)
  }
  
  func insertRightData(data: Element) {
    root.right = TreeNode(data: data)
  }
  
  func insertLeftChiled(tree: Tree<Element>) {
    root.left = tree.root
  }
  
  func insertRightChiled(tree: Tree<Element>) {
    root.right = tree.root
  }
}

```


Basic tree traversals.

```swift
extension Tree {

  func inOrder() {
    inOrederHelper(root)
  }
  
  private func inOrederHelper(node: TreeNode<Element>?) {
    guard let node = node else {
      return
    }
    
    inOrederHelper(node.left)
    print(node.data)
    inOrederHelper(node.right)
  }

  func preOrder() {
    preOrderHelper(root)
  }
  
  private func preOrderHelper(node: TreeNode<Element>?) {
    guard let node = node else {
      return
    }
    print(node.data)
    preOrderHelper(node.left)
    preOrderHelper(node.right)
  }
  
  func postOrder() {
    postOrderHelper(root)
  }
  
  private func postOrderHelper(node: TreeNode<Element>?) {
    guard let node = node else {
      return
    }
    postOrderHelper(node.left)
    postOrderHelper(node.right)
    print(node.data)
  }
}
```

More tree traversals.

```swift
extension Tree {
  
  /**
          6
        /   \
       3     9
      / \    / \
     1   4  7   11
      \  \  \   / \
      2   5  8 10 12
  
   Output:
   6
   3,9
   1,4,7,11
   2,5,8,10,12
  */

  func horizontalLevelOrder() {
    
    typealias NodeAndLevel = (node: TreeNode<T>, level: Int)
    
    var array = [[T]]()
    
    func visit(nodeAndLevel: NodeAndLevel) {
      
      if array.count == nodeAndLevel.level {
        array.append([nodeAndLevel.node.data])
      } else {
        var currentNodesForLevel = array[nodeAndLevel.level]
        currentNodesForLevel.append(nodeAndLevel.node.data)
        array[nodeAndLevel.level] = currentNodesForLevel
      }
    }
    
    var queue = Queue<NodeAndLevel>()
    queue.enQueue((root, 0))
    
    while !queue.isEmpty {
      let currentNode = queue.deQueue()!
      visit(currentNode)
      
      if let leftNode = currentNode.node.left {
        queue.enQueue(NodeAndLevel(node: leftNode, level: currentNode.level + 1))
      }
      
      if let rightNode = currentNode.node.right {
        queue.enQueue(NodeAndLevel(node: rightNode, level: currentNode.level + 1))
      }
    }
    
    for levelItems in array {
      print(levelItems)
    }
  }
  
  
    /**
   Print a binary tree in zig zag way... that is:
        6
     /   \
    3     9
   / \    / \
  1   4  7   11
   \  \  \   / \
   2   5  8 10 12
   
   Output:
   6-9-3-1-4-7-11-12-10-8-5-2
   */
   
  func zigzagHorizontalLevelOrder() {
    
    typealias NodeAndLevel = (node: TreeNode<T>, level: Int)
    
    var array = [[T]]()
    
    func visit(nodeAndLevel: NodeAndLevel) {
      
      if array.count == nodeAndLevel.level {
        array.append([nodeAndLevel.node.data])
      } else {
        var currentNodesForLevel = array[nodeAndLevel.level]
        currentNodesForLevel.append(nodeAndLevel.node.data)
        array[nodeAndLevel.level] = currentNodesForLevel
      }
    }
    
    var queue = Queue<NodeAndLevel>()
    queue.enQueue((root, 0))
    
    while !queue.isEmpty {
      let currentNode = queue.deQueue()!
      visit(currentNode)
      
      if let leftNode = currentNode.node.left {
        queue.enQueue(NodeAndLevel(node: leftNode, level: currentNode.level + 1))
      }
      
      if let rightNode = currentNode.node.right {
        queue.enQueue(NodeAndLevel(node: rightNode, level: currentNode.level + 1))
      }
    }
    
    for index in 0..<array.count {
      let item = array[index]
      
      if index % 2 == 0 {
        for data in item {
          print("\(data)-", terminator:"")
        }
      } else {
        for data in item.reverse() {
          print("\(data)-", terminator:"")
        }
      }
    }
  }
  
    /**
   Vertical Level Order
   
        6
      /   \
     3     9
    / \    / \
   1   4  7   11
    \  \  \   / \
    2   5  8 10 12
   
   Output:
   [1]
   [3, 2]
   [6, 4, 7]
   [5, 9, 8, 10]
   [11]
   [12]
   */
  func verticalLevelOrder(){
    
    var paths = [Int:[T]]()
    verticalOrderHelper(root, level:0, paths: &paths)
    
    
    //If we want to print the by level order we have to find min and max horizontal distance
    var min = Int.max
    var max = Int.min

    findMinMaxHorizontalDistance(root, level: 0, maximum: &max, minimun: &min)
    
    for level in min...max {
      if let path = paths[level] {
        print(path)        
      }
    }
  }
  
  private func verticalOrderHelper(node: TreeNode<T>?, level: Int, inout paths:[Int:[T]]){
    
    guard let node = node else {
      return
    }
    
    if let _ = paths[level] {
      paths[level]!.append(node.data)
    } else {
      paths[level] = [node.data]
    }
    
    verticalOrderHelper(node.left , level: level - 1, paths: &paths)
    verticalOrderHelper(node.right, level: level + 1, paths: &paths)
  }
  
  private func findMinMaxHorizontalDistance(node: TreeNode<T>?, level: Int, inout maximum: Int, inout minimun: Int) {
    
    guard let node = node else {
      return
    }
    
    minimun = min(minimun, level)
    maximum = max(maximum, level)
    findMinMaxHorizontalDistance(node.left,  level: level - 1, maximum: &maximum, minimun: &minimun)
    findMinMaxHorizontalDistance(node.right, level: level + 1, maximum: &maximum, minimun: &minimun)
  }
}
```
  
  Tree height
  
  ```swift
  var height: Int {
    return max(getHeight(root.left), getHeight(root.right)) + 1
  }
  
  private func getHeight(node: TreeNode<T>?) -> Int {
    
    guard let node = node else {
      return 0
    }
    
    let leftSubtreeHight = getHeight(node.left)
    let rightSubtreeHight = getHeight(node.right)
    
    return max(leftSubtreeHight, rightSubtreeHight) + 1
  }
  ```
