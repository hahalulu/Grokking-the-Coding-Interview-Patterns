# Grokking-the-Coding-Interview-Patterns

## Pattern 1 : Sliding Window

### Find Averages of Sub Arrays

#### Brute Force
````
function find_averages_of_subarrays(K, arr) {
  //brute force
   let result = []
   for(let i = 0; i < arr.length-K+1; i++){
     //find sum of next k elements
     sum = 0
     for(let j = i; j < i + K; j++) {
       sum += arr[j]
     }
     result.push(sum/K)
   }
   return result
  

}

find_averages_of_subarrays(5, [1, 3, 2, 6, -1, 4, 1, 8, 2])
````

#### Sliding Window Approach
````
function find_averages_of_subarrays(K, arr) {
  //sliding window approach
  const results = []
  let sum = 0
  let windowStart = 0;
  for(let windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    sum += arr[windowEnd]//add the next element
    //slide the window, we don't need to slide if we've not hit the require window size of 'k'
    if(windowEnd >= K -1) {
      results.push(sum/K)//calculate the average
      sum -= arr[windowStart] //subtract the element going out
      windowStart += 1//slide the window ahead
    }
  }
return results
}

find_averages_of_subarrays(5, [1, 3, 2, 6, -1, 4, 1, 8, 2])
````
### Maximum Sum Subarray of Size K (easy)

#### Brute Force 
````
function max_sub_array_of_size_k(k, arr) {
  //brute force
  let maxWindowSum = 0
  let currentwindowSum = 0
  //loop through array
  for(let i = 0; i < arr.length-k+1; i++) {
    currentwindowSum = 0
    //keep track of sum in current window
    for(j = i; j < i + k; j++) {
      currentwindowSum += arr[j]
    }
    //if currentWindowSum is > maxWindowSum
    //set currentWindwoSum to maxWindowSum
    maxWindowSum = Math.max(maxWindowSum, currentwindowSum)
  }
  return maxWindowSum
};

max_sub_array_of_size_k(3, [2, 1, 5, 1, 3, 2])//9
max_sub_array_of_size_k(2, [2, 3, 4, 1, 5])//7
````
- time complexity will be `O(N*K)`, where `N` is the total number of elements in the given array
#### Sliding Window Approach
````
function max_sub_array_of_size_k(k, arr) {
  //sliding window
  let maxWindowSum = 0, currentwindowSum = 0, windowStart = 0
  
  for(windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    currentwindowSum += arr[windowEnd]//add the next element 
    //slide the window
    //we don't need to slide if we have not hit 
    //the required window size of k
    if(windowEnd >= k - 1) {
      maxWindowSum = Math.max(maxWindowSum, currentwindowSum);
      currentwindowSum -= arr[windowStart]//subtract the element going out
      windowStart += 1 //slide the window ahead
    }
  }
  return maxWindowSum
};

max_sub_array_of_size_k(3, [2, 1, 5, 1, 3, 2])//9
max_sub_array_of_size_k(2, [2, 3, 4, 1, 5])//7
````
- The time complexity of the above algorithm will be `O(N)`

### Smallest Subarray with a given sum (easy)

#### Sliding Window Approach

````
function smallest_subarray_with_given_sum(s, arr) {
  let windowSum = 0, minLength = Infinity, windowStart = 0;
  
  for(windowEnd = 0; windowEnd < arr.length; windowEnd++){
    //add elements from the beggining of the array 
    //until thier sum becomes >= to s
    windowSum += arr[windowEnd]
    //remember the length of this window at the 
    //smallest window so far
    //keep adding one element in the sliding window
    while(windowSum >= s) {
      //1. check if the current window length is the small so far,
      //if so remeber it's length
      minLength = Math.min(minLength, windowEnd-windowStart+1)
      //while trying to shrink the window from the beggining
      //shrink the window until the window's sum 
  //is smaller than s again
      windowSum -= arr[windowStart]
      //2. subtract the first element of the window from the running sum to shrink the sliding window
      windowStart += 1
    }
  }
  if(minLength === Infinity) {
    return 0
  }
  return minLength;
};

smallest_subarray_with_given_sum(7, [2, 1, 5, 2, 3, 2])//2
smallest_subarray_with_given_sum(7, [2, 1, 5, 2, 8])//1
smallest_subarray_with_given_sum(8, [3, 4, 1, 1, 6])//3
````

- The time complexity of the above algorithm will be `O(N)`. The outer for loop runs for all elements, and the inner while loop processes each element only once; therefore, the time complexity of the algorithm will be `O(N+N)`), which is asymptotically equivalent to `O(N)`.
- The algorithm runs in constant space O(1)O(1).

### Longest Substring with K Distinct Characters (medium)

#### Sliding Window Approach

````
function longest_substring_with_k_distinct(str, k) {
   // Given a string, find the length of the longest substring in it with no more than K distinct characters.
  let windowStart = 0, maxLength = 0, charFrequency = {}

  //in the following loop we'll try to extend the range [windowStart, windowEnd]
  for(let windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const rightChar = str[windowEnd]
    if(!(rightChar in charFrequency)){
      charFrequency[rightChar] = 0
    }
    charFrequency[rightChar] +=1
     
    //insert charactes from the beginning of the string until we have 'K' distinct characters in the hashMap 
  //these characters will consitutue our sliding window.  We are asked to find the longest such window having no more that K distinct characters.  We will remember the length of the window as the longest window so far
  //we will keep adding on character in the sliding window in a stepwise fashion
    while(Object.keys(charFrequency).length > k){
      //in each step we will try to shrink the window from the beggining if the count of distinct characters in the hashmap is larger than K. We will shrink the window until we have no more that K distinct characters in the HashMap
  //while shrinking , we will decrement the characters frequency going out of the window and remove it from the HashMap if it's frequency becomes zero
      const leftChar = str[windowStart]
      charFrequency[leftChar] -= 1
      if(charFrequency[leftChar]=== 0){
        delete charFrequency[leftChar]
      }
      windowStart += 1//shrink the window
    }
    //after each step we will check if the current window length is the longest so far, and if so, remember it's length
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1)
    
  }
  return maxLength;
};

longest_substring_with_k_distinct("araaci", 2)//4, The longest substring with no more than '2' distinct characters is "araa".
longest_substring_with_k_distinct("araaci", 1)//2, The longest substring with no more than '1' distinct characters is "aa".
longest_substring_with_k_distinct("cbbebi", 3)//5, The longest substrings with no more than '3' distinct characters are "cbbeb" & "bbebi".
````
- The above algorithm’s time complexity will be `O(N)`, where `N` is the number of characters in the input string. The outer for loop runs for all characters, and the inner while loop processes each character only once; therefore, the time complexity of the algorithm will be `O(N+N)`, which is asymptotically equivalent to `O(N)`
- The algorithm’s space complexity is `O(K)`, as we will be storing a maximum of `K+1` characters in the HashMap.


## Pattern: Tree Breadth First Search

### Binary Tree Level Order Traversal (easy) 😕
````
class Deque {
    constructor() {
        this.front = this.back = undefined;
    }
    addFront(value) {
        if (!this.front) this.front = this.back = { value };
        else this.front = this.front.next = { value, prev: this.front };
    }
    removeFront() {
        let value = this.peekFront();
        if (this.front === this.back) this.front = this.back = undefined;
        else (this.front = this.front.prev).next = undefined;
        return value;
    }
    peekFront() { 
        return this.front && this.front.value;
    }
    addBack(value) {
        if (!this.front) this.front = this.back = { value };
        else this.back = this.back.prev = { value, next: this.back };
    }
    removeBack() {
        let value = this.peekBack();
        if (this.front === this.back) this.front = this.back = undefined;
        else (this.back = this.back.next).back = undefined;
        return value;
    }
    peekBack() { 
        return this.back && this.back.value;
    }
}

class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null; 
  }
};


function traverse (root) {
  result = [];
  if(root === null ) {
    return result
  }
  
  const queue = new Deque()
  //Start by pushing the root node to the queue.
  queue.addFront(root)
  //Keep iterating until the queue is empty.
  let currentLevel = []
  while (queue.length > 0) {
    const levelSize = queue.length
    //In each iteration, first count the elements in the queue (let’s call it levelSize). We will have these many nodes in the current level.
     
    for(i = 0; i < levelSize; i++) {
      TreeNode = queue.removeFront()
      //add the node to the current level
      currentLevel.push(TreeNode.val)
      //insert the children of current node in the queue
      if(TreeNode.left !== null) {
        queue.addBack(TreeNode.left)
      }
    }
    if(TreeNode.right !== null) {
      queue.addBack(TreeNode.right)
    }
  }
  
  result.push(currentLevel)
  
  //Next, remove levelSize nodes from the queue and push their value in an array to represent the current level.
  //After removing each node from the queue, insert both of its children into the queue.
  //If the queue is not empty, repeat from step 3 for the next level.
  return result;
};



var root = new TreeNode(12);
root.left = new TreeNode(7);
root.right = new TreeNode(1);
root.left.left = new TreeNode(9);
root.right.left = new TreeNode(10);
root.right.right = new TreeNode(5);
console.log(`Level order traversal: ${traverse(root)}`);
````

- The time complexity of the above algorithm is `O(N)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` as we need to return a list containing the level order traversal. We will also need `O(N)` space for the queue. Since we can have a maximum of `N/2` nodes at any level (this could happen only at the lowest level), therefore we will need `O(N)` space to store them in the queue.

#### Easier to understand solution 
````
function TreeNode(val, left, right) {
  this.val = (val===undefined ? 0 : val)
  this.left = (left===undefined ? null : left)
  this.right = (right===undefined ? null : right)
 }
 
 const levelOrder = function  (root) {
  //If root is null return an empty array
  if(!root) return []
  
  const queue = [root] //initialize the queue with root
  const levels = [] //declare output array
  
  while(queue.length !== 0) {
    const queueLength = queue.length//get the length prior to deque
    const currLevel = []//declare this level
    //loop through to exahuast all options and only to include nodes at currLevel
    for(let i = 0; i < queueLength; i++) {
      //get next node
      const current = queue.shift()
      if(current.left) {
        queue.push(current.left)
      }
      if(current.right) {
        queue.push(current.right)
      }
      //after we add left and right for current, we add to currLevel
      currLevel.push(current.val)
    }
    //Level has been finished. Push into output array
    levels.push(currLevel) 
  }
  return levels
};



levelOrder([3,9,20,null,null,15,7])//[[3],[9,20],[15,7]]
levelOrder([1])//[[1]]
levelOrder([])//[]
````

### Reverse Level Order Traversal (easy)
````
function TreeNode(val, left, right) {
  this.val = (val===undefined ? 0 : val)
  this.left = (left===undefined ? null : left)
  this.right = (right===undefined ? null : right)
 }
 
 const traverse = function(root) {
  if(!root) return []
   const queue = [root]
   
   const levels = []
   
   while(queue.length !== 0) {
     const queueLength = queue.length
     const currLevel = []
     for (let i = 0; i < queueLength; i++) {
       const current = queue.push()
       if(current.left) {
         queue.push(current.left)
       }
       if(current.right) {
         queue.push(current.right)
       }
       currLevel.push(current.val)
     }
     levels.unshift(currLevel)
   }
   
   return levels
}
 
 traverse([[12], [7,1], [9, 10, null, 5]])
````



