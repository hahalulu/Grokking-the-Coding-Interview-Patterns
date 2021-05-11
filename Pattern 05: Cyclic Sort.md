## Pattern 5: Cyclic Sort

This pattern describes an interesting approach to deal with problems involving arrays containing numbers in a given range. For example, take the following problem:

>You are given an unsorted array containing numbers taken from the range 1 to ‘n’. The array can have duplicates, which means that some numbers will be missing. Find all the missing numbers.

To efficiently solve this problem, we can use the fact that the input array contains numbers in the range of `1` to `‘n’`. 
For example, to efficiently sort the array, we can try placing each number in its correct place, i.e., placing `‘1’` at index `‘0’`, placing `‘2’` at index `‘1’`, and so on. Once we are done with the sorting, we can iterate the array to find all indices that are missing the correct numbers. These will be our required numbers.

## Cyclic Sort (easy)
We are given an array containing `‘n’` objects. Each object, when created, was assigned a unique number from `1` to `‘n’` based on their creation sequence. This means that the object with sequence number `‘3’` was created just before the object with sequence number `‘4’`.

> Write a function to sort the objects in-place on their creation sequence number in `O(n)` and without any extra space. 

For simplicity, let’s assume we are passed an integer array containing only the sequence numbers, though each number is actually an object.
As we know, the input array contains numbers in the range of `1` to `‘n’`. We can use this fact to devise an efficient way to sort the numbers. Since all numbers are unique, we can try placing each number at its correct place, i.e., placing `‘1’` at index `‘0’`, placing `‘2’` at index `‘1’`, and so on.

To place a number (or an object in general) at its correct index, we first need to find that number. If we first find a number and then place it at its correct place, it will take us `O(N²)`, which is not acceptable.

Instead, what if we iterate the array one number at a time, and if the current number we are iterating is not at the correct index, we swap it with the number at its correct index. This way we will go through all numbers and place them in their correct indices, hence, sorting the whole array.

![](cyclicsort.png)

````
function cyclicSort (nums) {
  let i = 0;
  
  while(i < nums.length) {
    const j = nums[i] - 1//nums[i] = 3, 3-1 = 2
    if(nums[i] !== nums[j]) {//3 !== 2
      //swap
      // [nums[i], nums[j]] = [nums[j], nums[i]]
      let temp = nums[i]
      nums[i] = nums[j]
      nums[j] = temp
    } else {
      i++
    } 
  }  
  return nums;
}


cyclicSort ([3, 1, 5, 4, 2])
cyclicSort ([2, 6, 4, 3, 1, 5])
cyclicSort ([1, 5, 6, 4, 3, 2])
````
- The time complexity of the above algorithm is `O(n)`. Although we are not incrementing the index `i` when swapping the numbers, this will result in more than `‘n’` iterations of the loop, but in the worst-case scenario, the while loop will swap a total of `‘n-1’` numbers and once a number is at its correct index, we will move on to the next number by incrementing `i`. So overall, our algorithm will take `O(n) + O(n-1)` which is asymptotically equivalent to `O(n)`.
-The algorithm runs in constant space `O(1)`.

## Find the Missing Number (easy)
## Find all Missing Numbers (easy)
## Find the Duplicate Number (easy)
## Find all Duplicate Numbers (easy)
## 🌟 Find the Corrupt Pair (easy)
## 🌟 Find the Smallest Missing Positive Number (medium)
## 🌟 Find the First K Missing Positive Numbers (hard)
