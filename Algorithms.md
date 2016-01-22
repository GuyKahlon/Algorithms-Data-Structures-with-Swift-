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


3-Partition Problems
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


###Given an array containing just 0s and 1s sort it in place in one pass.

```swift
func sortBinaryArray(inout array:[Int]) {
  
  let end = array.count - 1
  let pivot = array[end]
  var partition = 0
  
  for index in 0..<end {
    if array[index] <= pivot {
      if index != partition {
        swap(&array[index], &array[partition])
      }
      partition++
    }
  }
  
  if partition != end {
    swap(&array[end], &array[partition])
  }
}
```


3SUM Problems
---------------------
###Given a sorted array of integers and number k. The function return true if there are 2 numbers a and b in the array so that the a + b = k

```swift 
func findTargetSum(array: [Int], var leftIndex:Int, var rightIndex: Int, k: Int) -> Bool {
  
  while leftIndex < rightIndex {
    let leftValue = array[leftIndex]
    let rightValue = array[rightIndex]
    
    let sum = leftValue + rightValue
    
    switch sum {
    case let sum where sum == k:
      return true
    case let sum where sum > k:
      rightIndex--
    case let sum where sum < k:
      leftIndex++
    default:
      print("Error")
    }
  }
  
  return false
}
```


###Check if an array of integers contains two elements that sum to a third element in the array.

Solution:<br />
Actually we just need to check if there are triplet the the sum is equal to zero.<br />
And with binary search we can do it efficiently<br />

```swift
func findTriplet(array: [Int]) -> Bool {
  
  var sortArray = array
  sortArray.quickSort()
  
  for index in 0..<sortArray.count {
    if findsum(sortArray, leftIndex: 0, rightIndex: sortArray.count - 1, k: sortArray[index], jumpIndex: index) {
      return true
    }
  }
  
  return false
}

func findsum(array: [Int], var leftIndex:Int, var rightIndex: Int, k: Int, jumpIndex: Int) -> Bool {
  
  if rightIndex == jumpIndex {
    rightIndex--
  }
  
  if leftIndex == jumpIndex {
    leftIndex++
  }
  
  while leftIndex < rightIndex {
    
    let leftValue = array[leftIndex]
    let rightValue = array[rightIndex]
    
    let sum = leftValue + rightValue
    
    switch sum {
    case let sum where sum == -k:
      print("Triplet is \(array[leftIndex]), \(array[rightIndex]), \(array[jumpIndex])")
      return true
    case let sum where sum > k:
      rightIndex--
      if rightIndex == jumpIndex {
        rightIndex--
      }
    case let sum where sum < k:
      leftIndex++
      if leftIndex == jumpIndex {
        leftIndex++
      }
    default:
      print("Error")
    }
  }
  
  return false
}
```

###3Sum Smaller, Given an array of n integers numbers and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.
 
For example:
Input:  nums = [-2, 0, 1, 3], and target = 2.
Output: Return 2. 

Because there are two triplets which sums are less than 2:

[-2, 0, 1]
[-2, 0, 3]

```swift
func threeSumSmaller(array: [Int], target: Int) -> Int {
  
  var counter = 0
  
  var sortedArray = array
  sortedArray.quickSort()
  
  for i in 0..<(sortedArray.count - 2) {
    
    var j = i + 1
    var k = array.count - 1
    
    while j < k {
      if sortedArray[i] + sortedArray[j] + sortedArray[k] < target {
        counter += (k-j)
        j++
      } else {
        k--
      }
    }
  }
  
  return counter
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
  
###Given an integer array numbers and you have to return a new counts array, The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].
 
  Given numbers = [5, 2, 6, 1]
 
  To the right of 5 there are 2 smaller elements (2 and 1).
  To the right of 2 there is only 1 smaller element (1).
  To the right of 6 there is 1 smaller element (1).
  To the right of 1 there is 0 smaller element.
 
 
  Return the array [2, 1, 1, 0].


```swift
func counterSamller(numbers: [Int]) -> [Int] {
  
  var result = [Int]()
  
  var sorted = [Int](count: numbers.count, repeatedValue: Int.max)
  
  for i in (0..<numbers.count).reverse() {
    let index = findIndexForNewElementInSortedArray(sorted, val: numbers[i])
    result.append(index)
    sorted.insert(numbers[i], atIndex: index)
  }
  
  return result.reverse()
}

func findIndexForNewElementInSortedArray(sorted: [Int], val: Int) -> Int {
  
  var left = 0
  var right = sorted.count - 1
  
  while left <= right {
    var mid = (left + right) / 2
    if sorted[mid] == val {
      while mid - 1 >= 0 && sorted[mid - 1] == val {
        mid--
      }
      return mid
    } else if sorted[mid] < val {
      left = mid + 1
    } else {
      right = mid - 1
    }
  }
  return left
}
```


###Add two bit strings. Given two bit sequences as strings, write a function to return the addition of the two sequences. Bit strings can be of different lengths also.
 
For example:
Input: string 1 is “1100011” and second string 2 is “10”
Output: then the function should return “1100101”.

```swift
func add2BinaryNumbers(num1: String, num2: String) -> String {
  
  var result = String()
  
  var index1 = num1.characters.count - 1
  var index2 = num2.characters.count - 1
  
  let charZero:Character = "0"
  
  var carry: UInt32 = 0
  
  while index1 >= 0 || index2 >= 0 {
    
    var bit1: UInt32 = 0
    if index1 >= 0 {
      bit1 = num1[index1].asciiValue! - (charZero.asciiValue)!
    }
    
    var bit2: UInt32 = 0
    if index2 >= 0 {
      bit2 = num2[index2].asciiValue! - charZero.asciiValue!
    }
    
    var sum = bit1 + bit2 + carry
    if sum == 3 {
      sum = 1
      carry = 1
    } else if sum == 2 {
      sum = 0
      carry = 1
    } else {
      carry = 0
    }
    
    result += "\(sum)"
    
    index1--
    index2--
  }
  
  if carry == 1 {
    result += "\(1)"
  }
  
  return String(result.characters.reverse())
}
```


###Given two strings, return true if they are one edit away from each other,else return false. An edit is insert/replace/delete a character.
 
 Examples:
 {"abc","ab"}  -> true
 {"abc","adc"} -> true
 {"abc","cab"} -> false

```swift
func isOneEdit(str1: String, str2: String) -> Bool {
  
  //The length difference between two input strings should not be more than 1.
  let diff = abs(str1.characters.count - str2.characters.count)
  if diff > 1 {
    return false
  }
  
  //When the length of the strings is same, the number of different chars should not be more than 1.
  if diff == 0 {
    var numberOfEdits = 0
    for (char1, char2) in zip(str1.characters, str2.characters) {
      if char1 != char2 {
        numberOfEdits++
      }
    }
    return numberOfEdits < 2
  }
  
  //If the length difference is 1.
  //then either one char can be inserted in the short string or deleted from the longer string.
  //Considering that, the number of different char should not be more than 1.
  return str1.rangeOfString(str2) != nil || str2.rangeOfString(str1) != nil
  //return str1.isSubstring(str2) || str2.isSubstring(str1)
}


extension String {
  
  func isSubstring(str: String) -> Bool {
    
    if str.characters.count == 0 {
      return true
    }
    
    let firstChar = str[0]
    
    for (index, character) in characters.enumerate() {
      if firstChar == character {
        
        var originIndex = index
        
        var flag = true
        for i in 0..<str.characters.count where flag {
          if originIndex >= characters.count {
            return false
          }
          
          if str[i] != self[originIndex]{
            flag = false
            break
          } else {
            originIndex++
          }
        }
        if flag {
          return true
        }
      }
    }
    
    return false
  }
}
```

###Find non repeating element -Given an array consisting of 2n+1 elements. n elements in it are married, i.e they occur twice in the array, however there is one element which only appears once in the array. I need to find that number in a single pass using constant memory. {assume all are positive numbers}
 
Eg :- 3 4 1 3 1 7 2 2 4
 
Ans:- 7

```swift
func findNonRepeatingElement(array: [Int]) -> Int {
  
  var element = 0;
  
  for item in array {
    element ^= item
  }
  return element;
}
```


Union-Find Problems
---------------------
###Number of Islands II<br /> 
A 2d grid map of m rows and n columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, count the number of islands after each addLand operation. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.<br /> 
Example:<br /> 
Given m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]].<br /> 
Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).<br /> 

0 0 0<br /> 
0 0 0<br /> 
0 0 0<br /> 
<br /> 
1 1 0<br />
0 0 1<br /> 
0 1 0<br /> 

We return the result as an array: [1, 1, 2, 3]
do it in time complexity O(k log mn), where k is the length of the positions.


Explanation about the answer algorithm
https://www.youtube.com/watch?v=UBY4sF86KEY

```swift
struct Point: Hashable {
  
  let x: Int
  let y: Int
  
  var hashValue: Int {
    return x*y + (x+y)
  }
}

func ==(lhs: Point, rhs: Point) -> Bool {
  return lhs.x == rhs.x && lhs.y == rhs.y
}

func numIsland(n: Int, m: Int, operations: [Point]) -> [Int] {
  
  var result = [Int]()
  
  var parent = [Point: Point]()
  var counter = 0
  
  func createNewIsalnd(point: Point) {
    parent[point] = point
  }
  
  func getAdjacent(point: Point) -> [Point] {
    var adjacent = [Point]()
    
    //Top
    if point.x-1 >= 0 {
      adjacent.append(Point(x: point.x-1, y: point.y))
    }
    
    //Right
    if point.y+1 < m {
      adjacent.append(Point(x: point.x, y: point.y+1))
    }
    
    //Buttom
    if point.x+1 < n  {
      adjacent.append(Point(x: point.x+1, y: point.y))
    }
    
    //Left
    if point.y-1 < n {
      adjacent.append(Point(x: point.x, y: point.y-1))
    }
    
    return adjacent
  }
  
  func find(point: Point) -> Point? {
    
    guard let p = parent[point] else {
      return nil
    }
    
    var root: Point? = p
    
    while let par = parent[p] where par != root {
      root = par
    }
    
    return root
  }
  
  func union(root1: Point, root2: Point) {
    parent[root1] = root2
  }
  
  var islands = Matrix<Float>(rows: n, columns: m, repeatedValue: 0)
  
  for p in operations {
    
    if islands[p.x, p.y] == 0 {
      
      islands[p.x, p.y] = 1
      createNewIsalnd(p)
      counter++
      
      for adj in getAdjacent(p) {
        if let root = find(adj) {
          if root != p {
            counter--
            union(root, root2: p)
          }
        }
      }
      result.append(counter)
    }
  }
  

  return result
}
```



###Min Distance between two nodes

Given a 2D m*n board, 0 means an empty space we can pass, and 1 means it is a block we cannot pass.
Give a starting and ending point, find out the min distance we can walk from one point to another. We can move up, down, left and right.<br />

Return the min distance between two nodes. If we can never reach, return -1.<br />
 
 For example:<br />
 0 0 0 0<br />
 S 0 0 0<br />
 0 0 0 E<br />
 
 return 4<br />
 
 0 0 0 0<br />
 S 1 1 0<br />
 0 0 1 E<br />
 
 return 6.s<br />

 
 Understand the problem:<br />
 Use BFS, traverse the matrix layer by layer, until we reach the ending point.<br />
 
 ```swift
func findMinPath(matrix: Matrix<Float>, start: Point, end: Point) -> Int {
  
  
  func getAdjacent(p: Point) -> [Point]{
    
    var adjacent = [Point]()
    //Up
    if p.x - 1 >= 0 && matrix[p.x-1, p.y] != 1.0 {
      adjacent.append(Point(x:p.x-1, y: p.y))
    }
    
    //Left
    if p.y+1 < matrix.columns && matrix[p.x, p.y+1] != 1.0{
      adjacent.append(Point(x:p.x, y: p.y+1))
    }
    
    //Down
    if p.x+1 < matrix.rows && matrix[p.x+1, p.y] != 1.0{
      adjacent.append(Point(x:p.x+1, y: p.y))
    }
    
    //Right
    if p.y-1 >= 0 && matrix[p.x, p.y-1] != 1.0{
      adjacent.append(Point(x:p.x, y: p.y-1))
    }
    
    return adjacent
  }
  
  
  var visited = Matrix<Float>(rows: matrix.rows, columns:matrix.columns, repeatedValue: 0.0)
  var queue = Queue<(point: Point, level: Int)>()
  queue.enQueue((start, 0))
  
  while !queue.isEmpty {
    
    let current = queue.deQueue()!
    if current.point == end {
      return current.level
    }
    visited[current.point.x, current.point.y] = 1.0
    
    for adj in getAdjacent(current.point) {
      if visited[adj.x, adj.y] == 0.0 {
        queue.enQueue((adj, current.level + 1))
      }
    }
  }
  
  
  return -1
}
```

###Smallest Rectangle Enclosing Black Pixels<br />
An image is represented by a binary matrix with 0 as a white pixel and 1 as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location (x, y) of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.
<br />
For example, given the following image:<br />
 ["0010",<br />
  "0110",<br />
  "0100"]<br />
 
 and x = 0, y = 2.<br />
 Return 6.<br />
 DFS Solution:<br />
 Look for the borders for the black pixels.

```swift
func smallestRectangleEnclosingBlackPixels(matrix: Matrix<Float>, point: Point) -> Int {

  func getAdjacent(p: Point) -> [Point]{
    
    var adjacent = [Point]()
    //Up
    if p.x - 1 >= 0 && matrix[p.x-1, p.y] == 1.0 {
      adjacent.append(Point(x:p.x-1, y: p.y))
    }
    
    //Left
    if p.y+1 < matrix.columns && matrix[p.x, p.y+1] == 1.0{
      adjacent.append(Point(x:p.x, y: p.y+1))
    }
    
    //Down
    if p.x+1 < matrix.rows && matrix[p.x+1, p.y] == 1.0{
      adjacent.append(Point(x:p.x+1, y: p.y))
    }
    
    //Right
    if p.y-1 >= 0 && matrix[p.x, p.y-1] == 1.0{
      adjacent.append(Point(x:p.x, y: p.y-1))
    }
    
    return adjacent
  }

  var minX = point.x
  var maxX = point.x
  var minY = point.y
  var maxY = point.y
  
  var visited = Matrix<Float>(rows: matrix.rows, columns: matrix.columns, repeatedValue: 0.0)
  
  func smallestRectangleHelper(point: Point){
    
    visited[point.x, point.y] = 1.0
    
    minX = min(minX, point.x)
    maxX = max(maxX, point.x)
    minY = min(minY, point.y)
    maxY = max(maxY, point.y)
    
    for adj in getAdjacent(point) {
      if visited[adj.x, adj.y] == 0.0 {
        smallestRectangleHelper(adj)
      }
    }
  }
  
  smallestRectangleHelper(point)
  return ((maxX - minX) + 1) * ((maxY - minY) + 1)
}
```







