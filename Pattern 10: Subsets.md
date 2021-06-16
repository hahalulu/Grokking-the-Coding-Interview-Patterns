# Pattern 10: Subsets

A huge number of coding interview problems involve dealing with <b>Permutations</b> and <b>Combinations</b> of a given set of elements. This pattern describes an efficient <b>Breadth First Search (BFS)</b> approach to handle all these problems.

## Subsets (easy)
https://leetcode.com/problems/subsets/

> Given a set with distinct elements, find all of its distinct subsets.

To generate all subsets of the given set, we can use the <b>Breadth First Search (BFS)</b> approach. We can start with an empty set, iterate through all numbers one-by-one, and add them to existing sets to create new subsets.

Let’s take the example-2 mentioned above to go through each step of our algorithm:

Given set: `[1, 5, 3]`
1. Start with an empty set: `[[]]`
2. Add the first number `1` to all the existing subsets to create new subsets: `[[],`<b>`[1]`</b>`];`
3. Add the second number `5` to all the existing subsets: `[[], [1], `<b>`[5], [1,5]`</b>`]`;
4. Add the third number `3` to all the existing subsets: `[[], [1], [5], [1,5], `<b>`[3], [1,3], [5,3], [1,5,3]`</b>`]`.

Since the input set has distinct elements, the above steps will ensure that we will not have any duplicate subsets.
````
function findSubsets(nums) {
  let subsets = []
  
  //start by adding the empty subset
  subsets.push([])
  
  for(let i = 0; i < nums.length; i++) {
    let currentNumber = nums[i]
    //we will take all existing subsets and insert the current number in them to create new subsets
    const n = subsets.length
    
    for(let j = 0; j < n; j++) {
      //create a new subset from the existing subset and insert the current element to it?
      //clone the permutation?
      subsets.push([...subsets[j], nums[i]])
    }
  }
  return subsets
}

findSubsets([1, 5, 3])
findSubsets([1, 3])
````
- Since, in each step, the number of subsets doubles as we add each element to all the existing subsets, therefore, we will have a total of `O(2ᴺ)` subsets, where `‘N’` is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be `O(N*2ᴺ)`.
- All the additional space used by our algorithm is for the output list. Since we will have a total of `O(2ᴺ)` subsets, and each subset can take up to `O(N)` space, therefore, the space complexity of our algorithm will be `O(N*2ᴺ)`.

## Subsets With Duplicates (medium)
https://leetcode.com/problems/subsets-ii/

> Given a set of numbers that might contain duplicates, find all of its distinct subsets.

This problem follows the <b>Subsets</b> pattern and we can follow a similar <b>Breadth First Search (BFS)</b> approach. The only additional thing we need to do is handle duplicates. Since the given set can have duplicate numbers, if we follow the same approach discussed in <b>Subsets</b>, we will end up with duplicate subsets, which is not acceptable. To handle this, we will do two extra things:
1. Sort all numbers of the given set. This will ensure that all duplicate numbers are next to each other.
2. Follow the same <b>BFS</b> approach but whenever we are about to process a duplicate (i.e., when the current and the previous numbers are same), instead of adding the current number (which is a duplicate) to all the existing subsets, only add it to the subsets which were created in the previous step.

Let’s take first Example mentioned below to go through each step of our algorithm:
````
Given set: [1, 5, 3, 3]  
Sorted set: [1, 3, 3, 5]
````
1. Start with an empty set: `[[]]`
2. Add the first number `1` to all the existing subsets to create new subsets: `[[], [1]]`;
3. Add the second number `3` to all the existing subsets: `[[], [1], [3], [1,3]]`.
4. The next number `3` is a duplicate. If we add it to all existing subsets we will get:
    ````
    [[], [1], [3], [1,3], [3], [1,3], [3,3], [1,3,3]]
    ````
````
We got two duplicate subsets: [3], [1,3]  
Whereas we only needed the new subsets: [3,3], [1,3,3] 
````
To handle this instead of adding `3` to all the existing subsets, we only add it to the new subsets which were created in the previous <b>3rd</b> step:
````
    [[], [1], [3], [1,3], [3,3], [1,3,3]]
````
5. Finally, add the forth number `5` to all the existing subsets: `[[], [1], [3], [1,3], [3,3], [1,3,3], [5], [1,5], [3,5], [1,3,5], [3,3,5], [1,3,3,5]]`

````
function subsetsWithDupe(nums) {
  let subsets = []
  nums.sort((a, b) => a-b)
  
  //start by adding the empty subset
  subsets.push([])
  
  let start = 0
  let end = 0
  
  for(let i = 0; i < nums.length; i++) {
    //if the current and previous elements are the same, 
    //create new subsets only from the subsets added in the previous step
   
    end = subsets.length
    
    for(let j = start; j < end; j++) {
      //create a new subset from the existing subset and add the current element to it
      subsets.push([...subsets[j], nums[i]])
      
      if(nums[i + 1] === nums[i]) {
        start = end
      } else {
        start = 0
      }
    }
  }
  return subsets
}

subsetsWithDupe([1, 5, 3, 3])
subsetsWithDupe([1, 3, 3])
````
- Since, in each step, the number of subsets doubles (if not duplicate) as we add each element to all the existing subsets, therefore, we will have a total of `O(2ᴺ)` subsets, where `‘N’` is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be `O(N*2ᴺ)`.
- All the additional space used by our algorithm is for the output list. Since, at most, we will have a total of `O(2ᴺ)` subsets, and each subset can take up to `O(N)` space, therefore, the space complexity of our algorithm will be `O(N*2ᴺ)`.
