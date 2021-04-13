# Pattern: Two Pointer

## Pair with Target Sum  aka "Two Sum" (easy) 🌴

### Hash Table Solution 
````
function pair_with_targetsum(nums, target) {
  //Instead of using a two-pointer or a binary search approach, we can utilize a HashTable to search for the required pair. We can iterate through the array one number at a time. Let’s say during our iteration we are at number ‘X’, so we need to find ‘Y’ such that “X + Y == TargetX+Y==Target”. We will do two things here:
  const arr = {}
  for(let i = 0; i < nums.length; i ++){
    let item = nums[i]
     //Search for ‘Y’ (which is equivalent to “Target - XTarget−X”) in the HashTable. If it is there, we have found the required pair
  //Otherwise, insert “X” in the HashTable, so that we can search it for the later numbers.
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
function pair_with_targetsum(nums, target) {
  let start = 0
  let end = nums.length - 1
  
  while(start < end) {
    let currentSum = nums[start] + nums[end]
    if(currentSum === target) {
      return [start, end]
    }
    if (target > currentSum) {
      start++
    }
    else {
      end--
    }
  }
    return [-1, -1]
}
````
### Remove Duplicates (easy)

````
function remove_duplicates(arr) {
  
  // assume array is sorted
  
  //two-pointers
  //keep one pointer for the iterating array
  let i = 1
  //one point for placing the next non dupe number
  let nextNonDupe = 1
  
  // shift elements whenever we encounter dupes
  while(i < arr.length) {
    if(arr[nextNonDupe - 1] !== arr[i]) {
      arr[nextNonDupe] = arr[i]
      nextNonDupe++
    }
    i++
  }
  return nextNonDupe
};

remove_duplicates([2, 3, 3, 3, 6, 9, 9])//4 
remove_duplicates([2, 2, 2, 11])//2 
````
- The time complexity of the above algorithm will be `O(N`), where `‘N’` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

````
function removeElement(arr, key) {
  //two-pointers
  //index of the next element which is not 'key'
  let nextElement = 0
  
  for(let i = 0; i< arr.length; i++) {
    if(arr[i] !== key) {
      arr[nextElement] = arr[i]
      nextElement++
    }
  }
  return nextElement
};

removeElement([3, 2, 3, 6, 3, 10, 9, 3], 3)//4
removeElement([2, 11, 2, 2, 1], 2)//2
````
- The time complexity of the above algorithm will be `O(N)`, where `‘N’` is the total number of elements in the given array.
- The algorithm runs in constant space O(1)O(1).
