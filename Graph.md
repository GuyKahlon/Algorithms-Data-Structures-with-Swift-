### Graph

```swift

private class GraphNode<T: Equatable>: Equatable {
  
  typealias Element = T
  
  private var adjacent = [GraphNode<Element>]()
  var data: Element
  var visited: Bool = false
  
  init(data: Element) {
    self.data = data
  }
}

private func == <E:Equatable> (lhs:GraphNode<E>, rhs:GraphNode<E>) -> Bool {
  return lhs.data == rhs.data
}


struct Graph<T: Equatable> {
  
  typealias Element = T
  
  private var root: GraphNode<Element>
  
  init(data: Element) {
    root = GraphNode(data: data)
  }
  
  func addAdjacent(data: Element) {
    root.adjacent.append(GraphNode(data: data))
  }
 
  func addAdjacent(graph: Graph<Element>) {
    root.adjacent.append(graph.root)
  }
}

```

Basic Graph traversals
```swift
  func depthFirstSearch() {
    DFSHelper(root)
  }
  
  private func DFSHelper(node: GraphNode<Element>?) {
    guard let node = node else {
      return
    }
    
    visit(node)
    node.visited = true
    
    for adj in node.adjacent {
      if adj.visited == false {
        DFSHelper(adj)
      }
    }
  }

  func breadtFirstSearch() {
    
    var queue = Queue<GraphNode<Element>>()
    
    visit(root)
    root.visited = true
    queue.enQueue(root)
    
    while !queue.isEmpty {
      let currentNode = queue.deQueue()!
      for adj in currentNode.adjacent {
        if adj.visited == false {
          visit(adj)
          adj.visited = true
          queue.enQueue(adj)
        }
      }
    }
  }
  
  private func visit(node: GraphNode<Element>) {
    print("\(node.data)")
  }
  
```


