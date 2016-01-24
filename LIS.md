# Longest Increasing Subsequence

The longest Increasing Subsequence (LIS) problem is to find the length of the longest subsequence of a given sequence such that all elements of the subsequence are sorted in increasing order.<br /> 
For example:
length of LIS for:<br />
{ 10, 22, 9, 33, 21, 50, 41, 60, 80 } <br />
is 6 and LIS is:<br />
{10, 22, 33, 50, 60, 80}.

```swift
func LongestIncreasingSubsequence(array: [Int]) -> Int {
  
  var lis = [[Int]](count: array.count, repeatedValue: [Int]())
 
  lis[0].append(array[0])

  for i in 1..<array.count {
    for j in 0..<i {
      if array[j] < array[i] && lis[i].count < lis[j].count + 1 {
        lis[i] = lis[j]
      }
    }
    lis[i].append(array[i])
  }
  
  
  var max = 0
  
  for index in 0..<lis.count {
    let currentSubsequence = lis[index]
    if currentSubsequence.count > max {
      max = index
    }
  }
  
  print("The longest subsequence is \(lis[max])")
  
  return lis[max].count
}
```


###Given a string, find the longest substring that contains only two unique characters.<br /> 
For example:<br />
given "abcbbbbcccbdddadacb". <br />
The longest substring that contains 2 unique character is:<br />
"bcbbbbcccb".<br />

```Swift
func LengthOfLongestSubstringTwoDistinct(s: String) -> Int {
  
  var j = -1
  var maxLen = 0
  var i = 0
  var starIndex = 0
  var endIndex = 0
  
  for k in 1..<s.characters.count {
    
    if s[k] == s[k-1]{
      continue
    }
    
    if j >= 0 && s[k] != s[j] {
      maxLen = max(maxLen, k - i)
      i = k-1
    }
    j = k-1
  }

  maxLen = max(maxLen, s.characters.count - i)
  return maxLen
}
```

###Given a string, find the longest substring that contains only two unique characters.

```Swift
func maxSubStringKUniqueChars(s: String, k: Int) -> Int {
  
  var maxLen = 0
  var i = 0
  
  var dic = [Character : Int]()
  
  for index in 0..<s.characters.count {
    
    let currentChar = s[index]

    if var counter = dic[currentChar] {
      dic[currentChar] = counter++
    } else {
      dic[currentChar] = 1
    }
    
    var allDiffChars = dic.keys.count
    
    if allDiffChars > k {
      maxLen = max(maxLen, index - i)
      
      //The number od steps to ge back is k-1, becouse now we have k+1 different chars.
      var newI = k-1
      while allDiffChars < k - 1 {
        let cahr = s[newI]
        if let count = dic[cahr] {
          if count == 1 {
            dic[currentChar] = nil
          } else {
            dic[currentChar]!--
          }
          newI--
          allDiffChars = dic.keys.count
        }
      }
      i = newI
    }
  }
  
  maxLen = max(maxLen, s.characters.count - i)
  return maxLen
}
```


###Given an unsorted array of nonnegative integers, find a continous subarray which adds to a given number.
Examples:<br />
Input: arr[] = {1, 4, 20, 3, 10, 5}, sum = 33<br />
Ouptut: Sum found between indexes 2 and 4<br />

Input: arr[] = {1, 4, 0, 0, 3, 10, 5}, sum = 7<br />
Ouptut: Sum found between indexes 1 and 4<br />

Input: arr[] = {1, 4}, sum = 0<br />
Output: No subarray found<br />

```Swift
func subarraySumToTarget(array: [UInt], target: UInt) -> (start: Int, end: Int)? {
  
  var i: Int = 0
  var sum: UInt = 0
  
  for index in 0..<array.count {
    
    sum += array[index]
  
    while sum > target {
      sum -= array[i]
      i++
    }
    
    if sum == target {
      return (start:i, end: index)
    }
  }
  
  return nil
}
```
























