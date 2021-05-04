# Pattern: Two Pointer

In problems where we deal with sorted arrays (or LinkedLists) and need to find a set of elements that fulfill certain constraints, the Two Pointers approach becomes quite useful. The set of elements could be a pair, a triplet or even a subarray. For example, take a look at the following problem:

> Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.

To solve this problem, we can consider each element one by one (pointed out by the first pointer) and iterate through the remaining elements (pointed out by the second pointer) to find a pair with the given sum. The time complexity of this algorithm will be `O(N^2)` where `‘N’` is the number of elements in the input array.

Given that the input array is sorted, an efficient way would be to start with one pointer in the beginning and another pointer at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do not, we will do one of two things:
1. If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

## Pair with Target Sum  aka "Two Sum" (easy) 🌴 
https://leetcode.com/problems/two-sum/
> Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.

Write a function to return the indices of the two numbers (i.e. the pair) such that they add up to the given target.

Since the given array is sorted, a brute-force solution could be to iterate through the array, taking one number at a time and searching for the second number through <b>Binary Search</b>. The time complexity of this algorithm will be `O(N*logN)`. Can we do better than this?

We can follow the <b>Two Pointers</b> approach. We will start with one pointer pointing to the beginning of the array and another pointing at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do, we have found our pair; otherwise, we will do one of two things:
1. If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.
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

Instead of using a two-pointer or a binary search approach, we can utilize a <b>HashTable</b> to search for the required pair. We can iterate through the array one number at a time. Let’s say during our iteration we are at number `‘X’`, so we need to find `‘Y’` such that `“X + Y == TargetX+Y==Target”`. We will do two things here:
1. Search for `‘Y’` (which is equivalent to `“Target - XTarget−X”`) in the HashTable. If it is there, we have found the required pair.
2. Otherwise, insert `“X”` in the HashTable, so that we can search it for the later numbers.
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

> Given an array of sorted numbers, <b>remove all duplicates</b> from it. You should <b>not use any extra space</b>; after removing the duplicates in-place return the length of the subarray that has no duplicate in it.

In this problem, we need to remove the duplicates in-place such that the resultant length of the array remains sorted. As the input array is sorted, therefore, one way to do this is to shift the elements left whenever we encounter duplicates. In other words, we will keep one pointer for iterating the array and one pointer for placing the next non-duplicate number. So our algorithm will be to iterate the array and whenever we see a non-duplicate number we move it next to the last non-duplicate number we’ve seen.

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

removeDuplicates([2, 3, 3, 3, 6, 9, 9])//4, The first four elements after removing the duplicates will be [2, 3, 6, 9].
removeDuplicates([2, 2, 2, 11])//2, The first two elements after removing the duplicates will be [2, 11].
````
- The time complexity of the above algorithm will be `O(N)`, where `‘N’` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

### Remove Element
https://leetcode.com/problems/remove-element/

> Given an unsorted array of numbers and a target ‘key’, remove all instances of ‘key’ in-place and return the new length of the array.

````
function removeElement(arr, key) {
  //pointed for index of the next element which is not the key
  let nextElement = 0
  
  for(i = 0; i < arr.length; i++) {
    if(arr[i] !== key) {
      arr[nextElement] = arr[i]
      nextElement++
    }
  }
  return nextElement
}

removeElement([3, 2, 3, 6, 3, 10, 9, 3], 3)//4, The first four elements after removing every 'Key' will be [2, 6, 10, 9].
removeElement([2, 11, 2, 2, 1], 2)//2, The first four elements after removing every 'Key' will be [2, 6, 10, 9].
````
- The time complexity of the above algorithm will be `O(N)`, where `‘N’` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

## Squaring a Sorted Array (easy)
https://leetcode.com/problems/squares-of-a-sorted-array/

This is a straightforward question. The only trick is that we can have negative numbers in the input array, which will make it a bit difficult to generate the output array with squares in sorted order.

An easier approach could be to first find the index of the first non-negative number in the array. After that, we can use <b>Two Pointers</b> to iterate the array. One pointer will move forward to iterate the non-negative numbers, and the other pointer will move backward to iterate the negative numbers. At any step, whichever number gives us a bigger square will be added to the output array. 

Since the numbers at both ends can give us the largest square, an alternate approach could be to use two pointers starting at both ends of the input array. At any step, whichever pointer gives us the bigger square, we add it to the result array and move to the next/previous number according to the pointer. 

````
function makeSquares(arr) {
  //The only trick is that we can have negative numbers in the input array, which will make it a bit difficult to generate the output array with squares in sorted order.
  //An easier approach could be to first find the index of the first non-negative number in the array. 
  //After that, we can use Two Pointers to iterate the array. 
  //One pointer will move forward to iterate the non-negative numbers
  //the other pointer will move backward to iterate the negative numbers. At any step, whichever number gives us a bigger square will be added to the output array.
  //Since the numbers at both ends can give us the largest square, an alternate approach could be to use two pointers starting at both ends of the input array. At any step, whichever pointer gives us the bigger square, we add it to the result array and move to the next/previous number according to the pointer. 
  
  const n = arr.length;
  let squares = Array(n).fill(0)
  let highestSquareIndex = n - 1
  let start = 0
  let end = n-1
  while(start<= end) {
    let startSquare = arr[start] * arr[start]
    let endSquare = arr[end] * arr[end]
    
    if(startSquare > endSquare) {
      squares[highestSquareIndex]  = startSquare
      start++
    } else {
      squares[highestSquareIndex]  = endSquare
      end--
    }
    highestSquareIndex--
  }
  return squares
}

makeSquares([-2, -1, 0, 2, 3])//[0, 1, 4, 4, 9]
makeSquares([-3, -1, 0, 1, 2])//[0, 1, 1, 4, 9]
````
- The above algorithm’s time complexity will be `O(N)` as we are iterating the input array only once.
- The above algorithm’s space complexity will also be `O(N)`; this space will be used for the output array.

## 🌟 Triplet Sum to Zero (medium)
https://leetcode.com/problems/3sum/

> Given an array of unsorted numbers, find all unique triplets in it that add up to zero.

This problem follows the <b>Two Pointers</b> pattern and shares similarities with <b>Pair with Target Sum</b>. A couple of differences are that the input array is not sorted and instead of a pair we need to find triplets with a target sum of zero.

To follow a similar approach, first, we will sort the array and then iterate through it taking one number at a time. Let’s say during our iteration we are at number `‘X’`, so we need to find `‘Y’` and `‘Z’` such that `X + Y + Z == 0`. At this stage, our problem translates into finding a pair whose sum is equal to `“-X”` (as from the above equation `Y + Z == -X`).

Another difference from Pair with Target Sum is that we need to find all the unique triplets. To handle this, we have to skip any duplicate number. Since we will be sorting the array, so all the duplicate numbers will be next to each other and are easier to skip.

````
function searchTriplets(arr) {
  arr.sort((a, b) => a-b)
  const triplets = [];
  
  for(i = 0; i< arr.length;i++) {
    
    if(i>0 && arr[i] === arr[i -1]){
      //skip the same element to avoid dupes
      continue
    }
    searchPair(arr, -arr[i], i+1, triplets)
  }
  return triplets;
};

function searchTriplets(arr) {
  arr.sort((a, b) => a -b)
  const triplets = []
  
  for(i = 0; i < arr.length; i++) {
    if(i > 0 && arr[i] === arr[i -1]){
      //skip same element to avoid duplicates
      continue
    }
    searchPair(arr, -arr[i], i + 1, triplets)
  }
  return triplets
};

function searchPair(arr, targetSum, start, triplets) {
  let end = arr.length -1
  while(start < end) {
    const currentSum = arr[start] + arr[end]
    if(currentSum === targetSum) {
      //found the triplet
      triplets.push([-targetSum, arr[start], arr[end]])
      start++
      end--
      while(start < end && arr[start] === arr[start-1]) {
        //skip same element to avoid duplicates
        start++
      }
      while(start < end && arr[end] === arr[end + 1]) {
        //skip same element to avoid duplicates
        end--
      }
    } else if(targetSum > currentSum) {
      //we need a pair with a bigger sum
      start++
    } else {
      //we need a pair with a smaller sum
      end--
    }
  }
}

searchTriplets([-3, 0, 1, 2, -1, 1, -2]) //[[-3, 1, 2], [-2, 0, 2], [-2, 1, 1], [-1, 0, 1]]
searchTriplets([-5, 2, -1, -2, 3]) //[[-5, 2, 3], [-2, -1, 3]]
````
- Sorting the array will take `O(N * logN)`. The `searchPair()` function will take `O(N)`. As we are calling `searchPair()` for every number in the input array, this means that overall `searchTriplets()` will take `O(N * logN + N^2)`, which is asymptotically equivalent to `O(N^2)`.
- Ignoring the space required for the output array, the space complexity of the above algorithm will be `O(N)` which is required for sorting.
