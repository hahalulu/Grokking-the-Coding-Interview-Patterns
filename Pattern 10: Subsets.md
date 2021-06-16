# Pattern 10: Subsets

A huge number of coding interview problems involve dealing with <b>Permutations</b> and <b>Combinations</b> of a given set of elements. This pattern describes an efficient <b>Breadth First Search (BFS)</b> approach to handle all these problems.

# Subsets (easy)
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
