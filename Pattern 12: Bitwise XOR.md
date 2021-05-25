# Pattern 12: Bitwise XOR

<b>XOR</b> is a logical bitwise operator that returns `0` (false) if both bits are the same and returns `1` (true) otherwise. In other words, it only returns `1` if exactly one bit is set to `1` out of the two bits in comparison.

|     A    |     B     |  A xor B  |
|:---:|:---:|:---:|
| 0|0|0|
| 0|1|1|
| 1|0|1|
| 1|1|0|

It is surprising to know the approaches that the XOR operator enables us to solve certain problems. For example, let’s take a look at the following problem:

> Given an array of `n-1` integers in the range from `1` to `n`, find the one number that is missing from the array.

A straight forward approach to solve this problem can be:
1. Find the sum of all integers from `1` to `n`; let’s call it `s1`.
2. Subtract all the numbers in the input array from `s1`; this will give us the missing number.
````
function findMissingNumber(arr) {
  const n = arr.length + 1
  
  //find sum of all numbers from 1 to n
  let s1 = 0
  for(let i = 1; i <= n; i++) {
    s1 += i
  }
  
  //subtract all numbers in input from sum
  arr.forEach((num) => {
    s1 -= num
  })
  
  //s1, is now the missing number
  return s1
}

findMissingNumber([1,5,2,6,4])//3
````
- The time complexity of the above algorithm is `O(n)` and the space complexity is `O(1)`.
### What could go wrong with the above algorithm? 
> While finding the sum of numbers from `1` to `n`, we can get integer overflow when `n` is large.

How can we avoid this? Can XOR help us here?

Remember the important property of XOR that it returns 0 if both the bits in comparison are the same. In other words, XOR of a number with itself will always result in 0. This means that if we XOR all the numbers in the input array with all numbers from the range `1` to `n` then each number in the input is going to get zeroed out except the missing number. Following are the set of steps to find the missing number using XOR:
1. XOR all the numbers from `1` to `n`, let’s call it `x1`.
2. XOR all the numbers in the input array, let’s call it `x2`.
3. The missing number can be found by `x1` XOR `x2`.
````
function findMissingNumber(arr) {
  const n = arr.length + 1
  
  //x1 represents XOR of all values from 1 to n
  //find sum of all numbers from 1 to n
  let x1 = 1
  for(let i = 2; i <= n; i++) {
    x1 = x1 ^ i
  }
  
  //x2 represents XOR of all values in arr
  let x2 = arr[0]
  for(let i = 1; i < n-1; i++) {
    x2 = x2 ^ arr[i]
  }
  
  //missing number is the xor of x1 and x2
  return x1 ^ x2
}

findMissingNumber([1,5,2,6,4])//3
````
- The time complexity of the above algorithm is `O(n)` and the space complexity is `O(1)`. The time and space complexities are the same as that of the previous solution but, in this algorithm, we will not have any integer overflow problem.
### Important properties of XOR to remember 
Following are some important properties of XOR to remember:

- Taking XOR of a number with itself returns 0, e.g.,
  - 1 ^ 1 = 0
  - 29 ^ 29 = 0
- Taking XOR of a number with 0 returns the same number, e.g.,
  - 1 ^ 0 = 1
  - 31 ^ 0 = 31
- XOR is Associative & Commutative, which means:
  - (a ^ b) ^ c = a ^ (b ^ c)
  - a ^ b = b ^ a
## Single Number (easy)
https://leetcode.com/problems/single-number/
> In a non-empty array of integers, every number appears twice except for one, find that single number.

One straight forward solution can be to use a <b>HashMap</b> kind of data structure and iterate through the input:
- If number is already present in <b>HashMap</b>, remove it.
- If number is not present in <b>HashMap</b>, add it.
- In the end, only number left in the <b>HashMap</b> is our required single number.
````
function singleNumber(arr) {
  //HashMap
  let numberMap = {}
  //if number is not in HashMap, add it
  for(let i of arr) {
    numberMap[i] = (numberMap[i] || 0) + 1
  }
  //if number is already in HashMap, remove it
  //number left at the end is out rquired single number
  return numberMap
}

findMissingNumber([1, 4, 2, 1, 3, 2, 3])//4
findMissingNumber([7, 9, 7])//9
````
Time and space complexity Time Complexity of the above solution will be `O(n)`and space complexity will also be `O(n)`.

Can we do better than this using the <b>XOR</b> Pattern?

Recall the following two properties of XOR:
- It returns zero if we take XOR of two same numbers.
- It returns the same number if we XOR with zero.
So we can XOR all the numbers in the input; duplicate numbers will zero out each other and we will be left with the single number.

````
function singleNumber(arr) {
  //So we can XOR all the numbers in the input; duplicate numbers will zero out each other and we will be left with the single number.
  let num = 0
  
  for(let i = 0; i < arr.length; i++) {
    //console.log(num, arr[i], num^arr[i], "CL")
    num ^= arr[i]
  }
  return num
}

singleNumber([1, 4, 2, 1, 3, 2, 3])//4
singleNumber([7, 9, 7])//9
````
- Time complexity of this solution is `O(n)` as we iterate through all numbers of the input once.
- The algorithm runs in constant space `O(1)`.
## Two Single Numbers (medium)
https://leetcode.com/problems/single-number-iii/
## Complement of Base 10 Number (medium)
https://leetcode.com/problems/complement-of-base-10-integer/
## 🌟 Flip Binary Matrix(hard)
## 
