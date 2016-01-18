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
  
  func depthFirstSearch() {
    DFSHelper(root)
  }
}

```
