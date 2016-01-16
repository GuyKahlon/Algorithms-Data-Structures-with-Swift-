# Algorithms With Swift

Swift 2.1 required

###Quick Sort

```swift
extension Array where Element: Comparable {
  mutating func quickSort() {
    quickSortHelper(left: 0, right: count-1)
  }
  
  private mutating func quickSortHelper(left left: Int, right: Int) {
    
    if left < right {
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


### 3 partition problem

1. Given an input integer array and 3 boolean functions: isLow(int input), isMedium(int input), isHigh(int input).
Write a function that returns an array such that all the low numbers in the input array are sorted before all the medium numbers in the input array, which come before all the high numbers in the array.

You can assume that someone has written functions for you like:

func isLow(int input) -> Bool
func boolean isMedium(int input) -> Bool
func boolean isHigh(int input) -> Bool

You can also assume that every int will fall into one (and only one) of these categories.

For example:

[5, 100, 500, 4, 105, 408, -9, 812]

Low: <100
Medium: 101-300
High: >300

input: 
[4, 5, -9, 100, 105, 408, 819, 500] 
or
[4, 5, -9, 100, 105, 500, 819, 408] 
or
etc.

Solve this in linear time and linear space.


Solution:

It is closely related to the partition procedure in quick sort algorithm, where the array is partitioning into 2 parts (two-way partitioning), separated by some (randomly chosen) pivot value. The partitioning procedure runs in O(n) time, and is a bottleneck of quick sort performance. In this DNF problem, three-way partitioning is required.

For more details and great article please visit: http://www.capacode.com/array/tri-partition-aka-dutch-national-flag-problem/

```swift
extension Int {
  func isLow() -> Bool {
    return self < 100
  }
  func isMedium() -> Bool {
    return 101 <= self && self <= 300
  }
  func isHigh() -> Bool {
    return self > 301
  }
}

func Tripartition(inout array: [Int]) {
  
  var r = 0
  var w = 1
  var b = array.count - 1
  
  while w <= b {
    if array[w].isLow() {
      if r != w {
        swap(&array[r], &array[w])
      }
      r++
      w++
    } else if array[w].isHigh() {
      if w != b {
        swap(&array[w], &array[b])
      }
      b--
    } else {
      w++
    }
  }
}
```

2. Given n color balls, each ball has one of three colors: red, blue or white. 
These balls are randomly arranged in a line. Design an algorithm to rearrange the balls so that ball colors are in order: 
red, white, blue. 
Each time you can swap two balls.

Example:
Input: <br />

  [🔵, 🔵, 🔵, 🔵, ⚪️, 🔵, 🔵, 🔴]

output: <br />
  [🔴, ⚪️, 🔵, 🔵, 🔵, 🔵, 🔵, 🔵]
  

Solution:
In this question I have implemented the three-partition by two times call the standard two-partition from Quicksort algorithm

```swift

    var balls = [
      Ball.Blue,
      Ball.Blue,
      Ball.Blue,
      Ball.Blue,
      Ball.White,
      Ball.Blue,
      Ball.Blue,
      Ball.Red
    ]
    
    print(balls)
    DutchNationalFlag(&balls)
    print(balls)
    

enum Ball: Int, CustomStringConvertible, Comparable {
  case Red = 0, White = 1, Blue = 2
  
  var description: String {
    switch self {
    case .Red:
      return "🔴"
    case .White:
      return "⚪️"
    case .Blue:
      return "🔵"
    }
  }
}

func ==(x: Ball, y: Ball) -> Bool {
  return x.hashValue == y.hashValue
}
func <(x: Ball, y: Ball) -> Bool {
  return x.hashValue < y.hashValue
}

func DutchNationalFlag(inout array: [Ball]) {
  DutchNationalFlagHelprt(&array, left: 0, right: array.count - 1)
}

func DutchNationalFlagHelprt(inout balls:[Ball], left: Int, right: Int) {
  
  let p = balls.Partition(left: left, right: right)
  switch balls[p] {
  case .Red:
    balls.Partition(left: p+1, right: right)
  case .White:
    balls.Partition(left: left, right: p-1)
  case .Blue:
    balls.Partition(left: left, right: p-1)
  }
}
```



###Given a sequence of words, print all anagrams together.

```swift
extension SequenceType where Generator.Element == String {
  
  /**
   Input:
   ["cat", "dog", "tac", "god", "act", "aaa"]
   
   Output:
   ["cat", "tac", "act"]
   ["dog", "god"]
   
   */
  func findAllAnagrams() {
    
    var dic = [Int:[String]]()
    
    for item in self {
      if var arrayOfWords = dic[item.charactersHashValue] {
        arrayOfWords.append(item)
        dic[item.charactersHashValue] = arrayOfWords
      } else {
        dic[item.charactersHashValue] = [item]
      }
    }
    
    for (_, value) in dic {
      if value.count > 1 {
        print(value)
      }
    }
  }
}

extension String {
  var charactersHashValue: Int {
    var retVal = -1
    for char in self.utf8 {
      retVal += char.hashValue
    }
    return retVal
  }
}
```
  
