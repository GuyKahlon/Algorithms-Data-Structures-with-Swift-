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



