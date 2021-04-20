# Pattern: Two Pointer

In problems where we deal with sorted arrays (or LinkedLists) and need to find a set of elements that fulfill certain constraints, the Two Pointers approach becomes quite useful. The set of elements could be a pair, a triplet or even a subarray. For example, take a look at the following problem:

> Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.

To solve this problem, we can consider each element one by one (pointed out by the first pointer) and iterate through the remaining elements (pointed out by the second pointer) to find a pair with the given sum. The time complexity of this algorithm will be `O(N^2)` where `‘N’` is the number of elements in the input array.

Given that the input array is sorted, an efficient way would be to start with one pointer in the beginning and another pointer at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do not, we will do one of two things:
1. If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

## Pair with Target Sum  aka "Two Sum" (easy) 🌴 
https://leetcode.com/problems/two-sum/



### Brute Force

````
function pair_with_targetsum(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for(let j = 1; j < nums.length; j++) {
      if((nums[i] + nums[j]) === target) {
        //they cannot be at the same index
        if(i !== j) {
          return [i, j]
        }
      } 
    }
  }
}
````

### Two pointer Solution
* Assume the input is sorted
````
function pairWithTargetSum(arr, target) {
  let start = 0
  let end = arr.length-1
  
  while(start < end) {
    let currentSum = arr[start] + arr[end]
    
    if(currentSum === target) {
      return [start, end]
    }
    
    if(currentSum < target) {
      start++
    } else {
      end--
    }
  }
  return 0  
}

pairWithTargetSum([1, 2, 3, 4, 6], 6)//[1,3]
pairWithTargetSum([2, 5, 9, 11], 11)//[0,2]
````
- The time complexity of the above algorithm will be `O(N)`, where `‘N’` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

### Hash Table Solution 
````
function pair_with_targetsum(nums, target) {
  //Instead of using a two-pointer or a binary search approach, 
  //we can utilize a HashTable to search for the required pair. 
  //We can iterate through the array one number at a time. 
  //Let’s say during our iteration we are at number ‘X’, 
  //so we need to find ‘Y’ such that “X + Y == TargetX+Y==Target”. 
  
  //We will do two things here:
  const arr = {}
  for(let i = 0; i < nums.length; i ++){
    let item = nums[i]
     //1. Search for ‘Y’ (which is equivalent to “Target - XTarget−X”) in the HashTable. 
     //If it is there, we have found the required pair
     
     //2. Otherwise, insert “X” in the HashTable, so that we can search it for the later numbers.
    if(target - item in arr) {
      return [arr[target - item], i]
    }
    arr[nums[i]] = i
  }
  return [-1, -1]
}

pair_with_targetsum([1, 2, 3, 4, 6], 6)//[1, 3]
pair_with_targetsum([2, 5, 9, 11], 11)//[0, 2]
pair_with_targetsum([2, 7, 11, 15], 9)//[0, 1]
pair_with_targetsum([3, 2, 4], 6)//[1, 2]
pair_with_targetsum([3, 3], 6)//[0, 1]
````

- The time complexity of the above algorithm will be `O(N)`, where `‘N’` is the total number of elements in the given array.
- The space complexity will also be `O(N)`, as, in the worst case, we will be pushing `‘N’` numbers in the HashTable.
## Remove Duplicates (easy)
https://leetcode.com/problems/remove-duplicates-from-sorted-array/

* Assume the input is sorted
````
function removeDuplicates(arr) {

  //shift the elements left when we encounter duplicates
  
  //one pointer for iterating
  let i = 1
  
  //one pointer for placing this next non-duplicate
  let nextNonDupe = 1

  while(i < arr.length) {
    if(arr[nextNonDupe -1] !== arr[i]) {
      arr[nextNonDupe] = arr[i]
      nextNonDupe++
    }
    i++
  }
  return nextNonDupe  
}

removeDuplicates([2, 3, 3, 3, 6, 9, 9])//4
removeDuplicates([2, 2, 2, 11])//2
````
- The time complexity of the above algorithm will be `O(N)`, where `‘N’` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

### Remove Element
https://leetcode.com/problems/remove-element/

````
function removeElement(arr, key) {
  //index of the next element which is not 'key'
  let nextElement = 0
  
  for(i=0; i < arr.length; i++){
    if(arr[i] !== key) {
      arr[nextElement] = arr[i]
      nextElement++
    }
  }
  return nextElement
}

removeElement([3, 2, 3, 6, 3, 10, 9, 3], 3)//4
removeElement([2, 11, 2, 2, 1], 2)//2
````
- The time complexity of the above algorithm will be `O(N)`, where `‘N’` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

## Squaring a Sorted Array (easy)
````
function makeSquares(arr) {
  const n = arr.length
  squares = Array(n).fill(0)
  let highestSquareIndex = n -1
  let left = 0
  let right = n -1
  while(left <= right){
    let leftSquare = arr[left] * arr[left]
    let rightSquare = arr[right] * arr[right]
    
    if(leftSquare > rightSquare) {
      squares[highestSquareIndex] = leftSquare
      left++
    } else {
      squares[highestSquareIndex] = rightSquare
      right--
    }
    highestSquareIndex--
  }
  
  return squares;
};


makeSquares([-2, -1, 0, 2, 3])//[0, 1, 4, 4, 9]
makeSquares([-3, -1, 0, 1, 2])//[0, 1, 1, 4, 9]
````
- The above algorithm’s time complexity will be `O(N)` as we are iterating the input array only once.
- The above algorithm’s space complexity will also be `O(N)`; this space will be used for the output array.
