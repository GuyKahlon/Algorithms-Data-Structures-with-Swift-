# Sorting

###Quick Sort

```swift
extension Array where Element: Comparable {
  mutating func quickSort() {
    quickSortHelper(left: 0, right: count-1)
  }
  
  private mutating func quickSortHelper(left left: Int, right: Int) {
    
    if left >= right {
      let p = Partition(left: left, right: right)
      quickSortHelper(left: left, right: p-1)
      quickSortHelper(left: p+1, right: right)
    }
  }
  
  mutating func Partition(left left: Int, right: Int) -> Int {
    
    let pivot = self[right]
    var partition = left
    for index in left..<right {
      if self[index] < pivot {
        if partition != index {
          swap(&self[partition], &self[index])
        }
        
        partition++
      }
    }
    if partition != right {
      swap(&self[partition], &self[right])
    }
    return partition
  }
}
```

###Merge Sort
```swift
extension Array where Element: Comparable {
  
  mutating func mergeSort() {
    mergeSortHelper(0, right: self.count - 1)
  }
  
  private mutating func mergeSortHelper(left: Int, right: Int) {
    if left < right {
      let mid = (left + right) / 2
      mergeSortHelper(left, right: mid)
      mergeSortHelper(mid+1, right: right)
      merge(left, leftEnd: mid, rightStart: mid+1, rightEnd: right)
    }
  }
  
  private mutating func merge(leftStart: Int, leftEnd: Int, rightStart: Int, rightEnd: Int) {
    
    var result = [Element](count: (rightEnd - leftStart) + 1, repeatedValue: self.first!)
    var index = 0
    
    var left = leftStart
    var right  = rightStart
    
    while left <= leftEnd && right <= rightEnd{
      if self[left] < self[right] {
        result[index++] = self[left++]
      } else {
        result[index++] = self[right++]
      }
    }
    
    while left <= leftEnd {
      result[index++] = self[left++]
    }
    
    while right <= rightEnd {
      result[index++] = self[right++]
    }
    
    index = 0
    for i in leftStart...rightEnd {
      self[i] = result[index++]
    }
  }
}
```

