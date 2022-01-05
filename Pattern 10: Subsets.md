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
  const subsets = [];
  
  //start by adding the empty subset
  subsets.push([])
  
  for(let i = 0; i < nums.length; i++) {
    const currentNumber = nums[i]
    
    //we will take all existing subsets and insert the current
    //number in them to create new subsets
    const n = subsets.length

    //create a new subset from the existing subset and insert
    //the current element to it
    for(let j = 0; j < n; j++) {
      
      //clone the permutation
      // const set1 = subsets[j].slice(0)
      
      // set1.push(currentNumber)
      subsets.push([...subsets[j], nums[i]])
    }
  }

  return subsets;
};


findSubsets([1, 3])
findSubsets([1, 5, 3])
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
  //sort the numbers to handle duplicates
  nums.sort((a,b) => a-b)
  
  const subsets = [];
  
  subsets.push([])
  
  let start = 0
  let end = 0
  
  for(let i = 0; i < nums.length; i++) {
    start = 0
    
    //if current and the previous elements are the same,
    //create new subsets only from the subsets
    //added in the previous step
    if(i > 0 && nums[i] === nums[i-1]) {
      start = end + 1
    }
    
    end = subsets.length - 1
    
    for(let j = start; j < end + 1; j++) {
      //create a new suset from the existing subset and add the
      //current element to it
      subsets.push([...subsets[j], nums[i]])
    }
  }
  
  return subsets;
};


subsetsWithDupe([1, 3, 3])
subsetsWithDupe([1, 5, 3, 3])
````
- Since, in each step, the number of subsets doubles (if not duplicate) as we add each element to all the existing subsets, therefore, we will have a total of `O(2ᴺ)` subsets, where `‘N’` is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be `O(N*2ᴺ)`.
- All the additional space used by our algorithm is for the output list. Since, at most, we will have a total of `O(2ᴺ)` subsets, and each subset can take up to `O(N)` space, therefore, the space complexity of our algorithm will be `O(N*2ᴺ)`.
## Permutations (medium)
https://leetcode.com/problems/permutations/

> Given a set of distinct numbers, find all of its permutations.

Permutation is defined as the re-arranging of the elements of the set. For example, `{1, 2, 3}` has the following six permutations:
1. `{1, 2, 3}`
2. `{1, 3, 2}`
3. `{2, 1, 3}`
4. `{2, 3, 1}`
5. `{3, 1, 2}`
6. `{3, 2, 1}`

If a set has `‘n’` distinct elements it will have `n!` permutations.

This problem follows the <b>Subsets</b> pattern and we can follow a similar <b>Breadth First Search (BFS)</b> approach. However, unlike <b>Subsets</b>, every permutation must contain all the numbers.

Let’s take the example mentioned below to generate all the permutations. Following a <b>BFS</b> approach, we will consider one number at a time:
1. If the given set is empty then we have only an empty permutation set: `[]`
2. Let’s add the first element `1`, the permutations will be: `[1]`
3. Let’s add the second element `3`, the permutations will be: `[3,1], [1,3]`
4. Let’s add the third element `5`, the permutations will be: `[5,3,1], [3,5,1], [3,1,5], [5,1,3], [1,5,3], [1,3,5]`
 
Let’s analyze the permutations in the 3rd and 4th step. How can we generate permutations in the 4th step from the permutations of the 3rd step?

If we look closely, we will realize that when we add a new number `5`, we take each permutation of the previous step and insert the new number in every position to generate the new permutations. For example, inserting `5` in different positions of `[3,1]` will give us the following permutations:
1. Inserting `5` before `3`: `[5,3,1]`
2. Inserting `5` between `3` and `1`: `[3,5,1]`
3. Inserting `5` after `1`: `[3,1,5]`

````
function findPermutations(nums) {
  const result = [];
  let permutations = [[]]
  let numsLength = nums.length
  
   for(let i = 0; i < nums.length; i++) {
     const currentNumber = nums[i]
     
     //we will take all existing permutations an add the
     //current number to create a new permutation
     const n = permutations.length
     
     for(let p = 0; p < n; p++){
       const oldPermutation = permutations.shift()
       console.log(oldPermutation)
       
       //create a new permutation by adding the current number at every position
       for(let j  = 0; j < oldPermutation.length + 1; j++) {
         
         //clone the permutation
         const newPermutation = oldPermutation.slice(0)
         
         //insert the current number at index j
         newPermutation.splice(j, 0, currentNumber)
         
         if(newPermutation.length === numsLength) {
           result.push(newPermutation)
         } else {
           permutations.push(newPermutation)
         }
       }
     }
   }
  return result;
};

findPermutations([1, 3, 5])
````

- We know that there are a total of `N!` permutations of a set with `‘N’` numbers. In the algorithm above, we are iterating through all of these permutations with the help of the two ‘for’ loops. In each iteration, we go through all the current permutations to insert a new number in them. To insert a number into a permutation of size ‘`N’` will take `O(N)`, which makes the overall time complexity of our algorithm `O(N*N!)`.
- All the additional space used by our algorithm is for the `result` list and the `queue` to store the intermediate permutations. If you see closely, at any time, we don’t have more than `N!` permutations between the result list and the queue. Therefore the overall space complexity to store `N!` permutations each containing `N` elements will be `O(N*N!)`.

### Recursive Solution
````
function permute(nums) {
  //recursion
  let subsets = []
 
  generatePermuationsRecursive(nums, 0, [], subsets)
  
  return subsets   
};

function generatePermuationsRecursive(nums, index, currentPermuation, subsets) {
    if(index === nums.length) {
      subsets.push(currentPermuation)
    } else {
      //create a new permuation by adding the current number at every position
      for(let i = 0; i < currentPermuation.length +1; i++) {
        let newPermutation = currentPermuation.slice(0)
        
        //insert nums[index] at index i
        newPermutation.splice(i, 0, nums[index])
        generatePermuationsRecursive(nums, index+1, newPermutation, subsets)
      }
    }  
  }
  ````
## String Permutations by changing case (medium)
https://leetcode.com/problems/letter-case-permutation/

> Given a string, find all of its permutations preserving the character sequence but changing case.

This problem follows the <b>Subsets</b> pattern and can be mapped to <b>Permutations</b>.

Let’s take Example-2 mentioned above to generate all the permutations. Following a <b>BFS</b> approach, we will consider one character at a time. Since we need to preserve the character sequence, we can start with the actual string and process each character (i.e., make it upper-case or lower-case) one by one:
1. Starting with the actual string: `"ab7c"`
2. Processing the first character `a`, we will get two permutations: `"ab7c", "Ab7c"`
3. Processing the second character `b`, we will get four permutations: `"ab7c", "Ab7c", "aB7c", "AB7c"`
4. Since the third character is a digit, we can skip it.
5. Processing the fourth character `c`, we will get a total of eight permutations: `"ab7c", "Ab7c", "aB7c", "AB7c", "ab7C", "Ab7C", "aB7C", "AB7C"`

Let’s analyze the permutations in the 3rd and the 5th step. How can we generate the permutations in the 5th step from the permutations in the 3rd step?

If we look closely, we will realize that in the 5th step, when we processed the new character `c`, we took all the permutations of the previous step (3rd) and changed the case of the letter `c` in them to create four new permutations.
````
function findLetterCaseStringPermutations(str) {
  const permutations = [];
  permutations.push(str)
  
  //process every character of the string one by one
  for(let i = 0; i < str.length; i++) {
    //only characters we will skip digits
    if(isNaN(parseInt(str[i]), 10)){
      //we will take all exixting permutations and change the letter case appropriately
      const n = permutations.length
      
      for(let j = 0; j < n; j++) {
        //string to array
        const chs = permutations[j].split('')
        
        //if the current character is in upper case
        //change it to lower case or vice verse
        if(chs[i] === chs[i].toLowerCase()) {
          chs[i] = chs[i].toUpperCase()
        } else {
          chs[i] = chs[i].toLowerCase()
        }
        permutations.push(chs.join(''))
      }
    }
  }
  return permutations;
};


findLetterCaseStringPermutations("ad52")
findLetterCaseStringPermutations("ab7c")
````

- Since we can have`2ᴺ` permutations at the most and while processing each permutation we convert it into a character array, the overall time complexity of the algorithm will be `O(N*2ᴺ)`.
- All the additional space used by our algorithm is for the output list. Since we can have a total of `O(2ᴺ)` permutations, the space complexity of our algorithm is `O(N*2ᴺ)`.
## Balanced Parentheses (hard)
https://leetcode.com/problems/generate-parentheses/

> For a given number `N`, write a function to generate all combination of `N` pairs of balanced parentheses.

This problem follows the <b>Subsets</b> pattern and can be mapped to <b>Permutations</b>. We can follow a similar <b>BFS</b> approach.

Let’s take Example-2 mentioned above to generate all the combinations of balanced parentheses. Following a <b>BFS</b> approach, we will keep adding open parentheses `(` or close parentheses `)`. At each step we need to keep two things in mind:

- We can’t add more than `N` open parenthesis.
- To keep the parentheses balanced, we can add a close parenthesis `)` only when we have already added enough open parenthesis `(`. For this, we can keep a count of open and close parenthesis with every combination.

Following this guideline, let’s generate parentheses for `N=3`:

1. Start with an empty combination: `“”`
2. At every step, let’s take all combinations of the previous step and add `(` or `)` keeping the above-mentioned two rules in mind.
3. For the empty combination, we can add `(` since the count of open parenthesis will be less than `N`. We can’t add `)` as we don’t have an equivalent open parenthesis, so our list of combinations will now be: `(`
4. For the next iteration, let’s take all combinations of the previous set. For `(` we can add another `(` to it since the count of open parenthesis will be less than `N`. We can also add `)` as we do have an equivalent open parenthesis, so our list of combinations will be: `((`, `()`
5. In the next iteration, for the first combination `((`, we can add another `(` to it as the count of open parenthesis will be less than `N`, we can also add `)` as we do have an equivalent open parenthesis. This gives us two new combinations: `(((` and `(()`. For the second combination `()`, we can add another `(` to it since the count of open parenthesis will be less than `N`. We can’t add `)` as we don’t have an equivalent open parenthesis, so our list of combinations will be: `(((`, `(()`, `()(`
6. Following the same approach, next we will get the following list of combinations: `“((()”, “(()(”, “(())”, “()((”, “()()”`
7. Next we will get: `“((())”, “(()()”, “(())(”, “()(()”, “()()(”`
8. Finally, we will have the following combinations of balanced parentheses: `“((()))”, “(()())”, “(())()”, “()(())”, “()()()”`
9. We can’t add more parentheses to any of the combinations, so we stop here.

````
class ParenthesesString {
  constructor(str, openCount, closeCount) {
    this.str = str;
    this.openCount = openCount;
    this.closeCount = closeCount;
  }
}


function generateValidParentheses(num) {
  let result = [];
  let queue = []
  
  queue.push(new ParenthesesString('', 0, 0))
  
  while(queue.length > 0) {
    const ps = queue.shift()
    
    //if we've reached the maximum number of open and closed
    //parentheses, add to the result
    if(ps.openCount === num && ps.closeCount === num) {
      result.push(ps.str)
    } else {
      if(ps.openCount < num) {
        //if we can add an open parentheses, add it
        queue.push(new ParenthesesString(`${ps.str}(`, ps.openCount + 1, ps.closeCount))  
      }
      if(ps.openCount > ps.closeCount) {
        //if we can add a close parentheses, add it
        queue.push(new ParenthesesString(`${ps.str})`, ps.openCount, ps.closeCount + 1))
      }
    }
  }
  return result;
};


generateValidParentheses(2)
generateValidParentheses(3)
````
- Let’s try to estimate how many combinations we can have for `N` pairs of balanced parentheses. If we don’t care for the ordering - that `)` can only come after `(` - then we have two options for every position, i.e., either put open parentheses or close parentheses. This means we can have a maximum of `2ᴺ` combinations. Because of the ordering, the actual number will be less than `2ᴺ`
- If you see the visual representation of Example-2 closely you will realize that, in the worst case, it is equivalent to a binary tree, where each node will have two children. This means that we will have `2ᴺ` leaf nodes and `2ᴺ-1` intermediate nodes. So the total number of elements pushed to the queue will be `2ᴺ−1`, which is asymptotically equivalent to `O(2ᴺ)`. While processing each element, we do need to concatenate the current string with `(` or `)`. This operation will take `O(N)`, so the overall time complexity of our algorithm will be `O(N*2ᴺ)`. <i>This is not completely accurate but reasonable enough to be presented in the interview.</i>

- All the additional space used by our algorithm is for the output list. Since we can’t have more than `O(2ᴺ)` combinations, the space complexity of our algorithm is `O(N*2ᴺ)`.

### Recursive Solution 
````
function generateValidParentheses(num) {
  const result = [];
  const parenthesesString = Array(2 * num);
  generateValidParenthesesRecursive(num, 0, 0, parenthesesString, 0, result);
  return result;
}


function generateValidParenthesesRecursive(num, openCount, closeCount, parenthesesString, index, result) {
  // if we've reached the maximum number of open and close parentheses, add to the result
  if (openCount === num && closeCount === num) {
    result.push(parenthesesString.join(''));
  } else {
    if (openCount < num) { // if we can add an open parentheses, add it
      parenthesesString[index] = '(';
     generateValidParenthesesRecursive(num, openCount + 1, closeCount, parenthesesString, index + 1, result);
    }
    if (openCount > closeCount) { // if we can add a close parentheses, add it
      parenthesesString[index] = ')';
      generateValidParenthesesRecursive(num, openCount, closeCount + 1, parenthesesString, index + 1, result);
    }
  }
}

generateValidParentheses(2)
generateValidParentheses(3)
````
## Unique Generalized Abbreviations (hard)
https://leetcode.com/problems/generalized-abbreviation/
## 🌟 Evaluate Expression (hard)
https://leetcode.com/problems/different-ways-to-add-parentheses/
## 🌟 Structurally Unique Binary Search Trees (hard)
https://leetcode.com/problems/unique-binary-search-trees-ii/
## 🌟 Count of Structurally Unique Binary Search Trees (hard)
https://leetcode.com/problems/unique-binary-search-trees/
