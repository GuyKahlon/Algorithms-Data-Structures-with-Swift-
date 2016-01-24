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

###Check if the Tree is balanced
For the purposes of this question, a balanced tree is defined to be a tree such that the heights of the two subtrees of any node never differ by more than one.
```swift 
  func isBalanced() -> Bool {
    if let height = checkHeight(root) {
      print("The Tree is balanced and his height is = \(height)")
      return true
    } else {
      return false
    }
  }

  private func checkHeight(node: TreeNode<T>?) -> Int? {
    
    guard let node = node else {
      return 0
    }
    
    /* Check if left subtree is balanced. */
    guard let left = checkHeight(node.left) else {
      return nil
    }
    
    /* Check if rihgt subtree is balanced. */
    guard let right = checkHeight(node.right) else {
      return nil
    }
    
    if abs(left - right) > 1 {
      return nil
    } else {
      return max(left, right) + 1
    }
  }
```

Lowest Common Ancestor - LCA without parent pointer
LCA problem with no parent pointers. Given the root of a tree and pointers to two nodes contained in that tree, return the lowest common ancestor of the two nodes.

 ```swift
extension Tree where T: Equatable {
 
  func lowestCommonAncestor(data1: T, data2: T) -> T? {
    return lowestCommonAncestorHelper(root, data1: data1, data2: data2)?.data
  }
  
  private func lowestCommonAncestorHelper(node: TreeNode<T>?, data1: T, data2: T) -> TreeNode<T>? {
    
    guard let node = node else {
      return nil
    }
    
    if node.data == data1 || node.data == data2 {
      return node
    }
    
    let findLeft = lowestCommonAncestorHelper(node.left, data1: data1, data2: data2)
    let findRight = lowestCommonAncestorHelper(node.right, data1: data1, data2: data2)
    
    if let _ = findLeft, let _ = findRight {
      return node
    }
    
    if let _ = findLeft {
      return lowestCommonAncestorHelper(node.left, data1: data1, data2: data2)
    }
    
    if let _ = findRight {
      return lowestCommonAncestorHelper(node.right, data1: data1, data2: data2)
    }

    return nil
  }
}
```
Find the n-th node in a binary tree
Write a function that takes 2 arguments: a binary tree and an integers, it should return the n-th element in the inorder traversal of the binary tree. 

Very importent is to stop traversing the tree whenever we found the nth node

 ```swift
  func findElement(nth: Int) -> T? {
    
    var num = nth
    return findElementHelper(root, n: &num)
  }
  
  private func findElementHelper(node: TreeNode<T>?, inout n: Int) -> T? {
  
    guard let node = node else {
      return nil
    }
    
    if let element = findElementHelper(node.left, n: &n) {
      return element
    }
    
    if n == 0 {
      return node.data
    } else {
      n = n-1
    }
    
    return findElementHelper(node.right, n: &n)
  }
  ```
