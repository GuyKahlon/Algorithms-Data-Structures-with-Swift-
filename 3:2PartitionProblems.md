# Partition Problems

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


2-Partition Problems
---------------------

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

