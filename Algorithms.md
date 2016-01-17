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


3 partition problem
---------------------

### Given an input integer array and 3 boolean functions: isLow(), isMedium(), isHigh().<br /> 
Write a function that returns an array such that all the low numbers in the input array are sorted before all the medium numbers in the input array, which come before all the high numbers in the array.


You can assume that someone has written functions for you like:<br />
func isLow(int input) -> Bool<br />
func boolean isMedium(int input) -> Bool<br />
func boolean isHigh(int input) -> Bool<br />
<br />
You can also assume that every int will fall into one (and only one) of these categories.

For example:<br />

[5, 100, 500, 4, 105, 408, -9, 812]<br />

Low: <100<br />
Medium: 101-300<br />
High: >300<br />

input: <br />
[4, 5, -9, 100, 105, 408, 819, 500] <br />
or<br />
[4, 5, -9, 100, 105, 500, 819, 408]<br /> 
or<br />
etc.<br />

Solve this in linear time and linear space.<br />


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

### Given n color balls, each ball has one of three colors: red, blue or white.  These balls are randomly arranged in a line. Design an algorithm to rearrange the balls so that ball colors are in order:  red, white, blue.  Each time you can swap two balls.

Example: <br />
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


###Given a set of distinct integers, S, return all possible subsets. 
 
 Note: Elements in a subset must be in non-descending order. 
 The solution set must not contain duplicate subsets.
 
 For example:
 
 Input:  [1,2,3]
 
 Output: [[3], [1], [2], [1,2,3], [1,3], [2,3], [1,2], [] ]
 
```swift
func Subsets(s: [Int])-> [[Int]] {
  
  var sortedSet = s
  
  sortedSet.quickSort()
  
  var result = [[Int]]()
  
  for item: Int in sortedSet {
    
    var temp = [[Int]]()
    
    for subset in result {
      temp.append(subset + [item])
    }
    
    temp.append([item])
    
    result += temp
  }
  
  result.append([Int]())
  
  return result
}
```


###Given a Dictionary which as data like:
  { 
  '1' : 'A',
  '2' : 'B',
  '3' : 'C',
  ......
  '26' : 'Z'
  }

  Input will be an integer, return all possible combination of values from the dictionary<br /> 
  
  Example:<br /> 

  Input:  5918<br /> 
  Output: ["EIAH", "EIR"] <br /> 

  EIAH is from 5 9 1 8    <br /> 
  EIR is from 5 9 18      <br /> 
  
```swift
var Map : [ Int: Character] = [
  1: "A",
  2: "B",
  3: "C",
  4: "D",
  5: "E",
  6: "F",
  7: "G",
  8: "H",
  9: "I",
  10: "J",
  11: "K",
  12: "L",
  13: "M",
  14: "N",
  15: "O",
  16: "P",
  17: "Q",
  18: "R",
  19: "S",
  20: "T",
  21: "U",
  22: "V",
  23: "W",
  24: "X",
  25: "Y",
  26: "Z",
]


func allStrings(str: String) -> [String] {
  
  var stringBuilder = String()
  var result = [String]()
  allStringsHelper(str, start: 0, stringBuilder: &stringBuilder, result: &result);
  
  return result
}

func allStringsHelper(str: String, start: Int, inout stringBuilder: String, inout result: [String]) {
  
  if start >= str.characters.count {
    result.append(stringBuilder)
    return
  }
  
  let currentNum = Int(String(str[start]))!
  let mapedChar = Map[currentNum]!
  stringBuilder.append(mapedChar)
  
  allStringsHelper(str, start: start + 1, stringBuilder: &stringBuilder, result: &result)
  
  //Remove the last char from string builder
  stringBuilder.removeAtIndex(stringBuilder.endIndex.predecessor())
  
  if start < str.characters.count - 1 {
    let character = str[start]
    let characterNext = str[start+1]
    let numStr = String(character) + String(characterNext)
    if let num = Int(numStr) where num <= 26 {
      let c = Map[num]!
      stringBuilder.append(c)
      allStringsHelper(str, start: start + 2, stringBuilder: &stringBuilder, result: &result)
      stringBuilder.removeAtIndex(stringBuilder.endIndex.predecessor())
    }
  }
}
```



###Given a dictionary based simple password, create all possible (special character) passwords based on a provided mapping.
 
 Input: face
 Map: { a -> @, 4, A}
 
 Output: f@ce, f4ce, fAce
 
 For more details please visit: http://buttercola.blogspot.co.il/2014/11/facebook-password-combination.html
 
```swift 
func generatePasswords(str: String , dict: [Character: String]) -> [String] {
  
  var stringBuilder = String()
  var result = [String]()
  passwordHelper(str, start: 0, stringBuilder: &stringBuilder, dict: dict, result: &result);
  
  return result
}

func passwordHelper(str: String, start: Int, inout stringBuilder: String, dict: [Character: String], inout result: [String]) {
  
  if start >= str.characters.count {
    if str != stringBuilder {
      result.append(stringBuilder)
    }
    return
  }
  
  let character = str[start]
  if let mapString = dict[character] {
    for newChar in mapString.characters {
      stringBuilder.append(newChar)
      passwordHelper(str, start: start + 1, stringBuilder: &stringBuilder, dict: dict, result: &result)
      stringBuilder.removeAtIndex(stringBuilder.endIndex.predecessor())
    }
  } else  {
    stringBuilder.append(character)
    passwordHelper(str, start: start + 1, stringBuilder: &stringBuilder, dict: dict, result: &result)
    stringBuilder.removeAtIndex(stringBuilder.endIndex.predecessor())
  }
}
```

### Given a sequence of words, print all anagrams together.

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
  
