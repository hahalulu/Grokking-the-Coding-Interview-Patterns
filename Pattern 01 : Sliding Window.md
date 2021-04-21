## Pattern 1 : Sliding Window
In many problems dealing with an array (or a LinkedList), we are asked to find or calculate something among all the contiguous subarrays (or sublists) of a given size. For example, take a look at this problem:

> Given an array, find the average of all contiguous subarrays of size ‘K’ in it.

The efficient way to solve this problem would be to visualize each contiguous subarray as a sliding window of `‘5’` elements. This means that we will slide the window by one element when we move on to the next subarray. To reuse the sum from the previous subarray, we will subtract the element going out of the window and add the element now being included in the sliding window. This will save us from going through the whole subarray to find the sum and, as a result, the algorithm complexity will reduce to `O(N)`.

````
function findAveragesOfSubarrays(arr, k) {
  const result = []
  let windowSum = 0.0
  let windowStart = 0
  
  for(let windowEnd = 0; windowEnd < arr.length; windowEnd++) {
  
  //add the next element
   windowSum += arr[windowEnd]
   
    //slide the window, we dont need to slide if we've not hit the required window size of k
    if(windowEnd >= k-1) {
    
      //calculate the average
      result.push(windowSum/k)
      
      //subtract the element going out
      windowSum -= arr[windowStart]
      
      //slide the window ahead
      windowStart++
    }
  }
  return result  
}

findAveragesOfSubarrays([1, 3, 2, 6, -1, 4, 1, 8, 2], 5)
````
## Find Averages of Sub Arrays

### Brute Force
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

### Sliding Window Approach
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
## Maximum Sum Subarray of Size K (easy)

#### Brute Force 
````
function maxSubarrayOfSizeK(arr, k) {
  //brute force
  let maxSum = 0
  let windowSum = 0
  
  //loop through array
  for(let i = 0; i < arr.length -k + 1; i++) {
    
    //keep track of sum in current window
    windowSum = 0
    for(let j = i; j < i + k; j++) {
      windowSum += arr[j]
    }
    
    //if currentWindowSum is > maxWindowSum
    //set currentWindwoSum to maxWindowSu
    maxSum = Math.max(maxSum, windowSum)
  }
  return maxSum
}

max_sub_array_of_size_k(3, [2, 1, 5, 1, 3, 2])//9
max_sub_array_of_size_k(2, [2, 3, 4, 1, 5])//7
````
- Time complexity will be `O(N*K)`, where `N` is the total number of elements in the given array

### Sliding Window Approach
````
function maxSubarrayOfSizeK(arr, k) {
  //sliding window
  let maxSum = 0
  let windowSum = 0
  let windowStart = 0
  
  //loop through array
  for(let windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    //add the next element
    windowSum += arr[windowEnd]
    
    //slide the window, we dont need to slid if we
    //haven't hit the required window size of 'k'
    if(windowEnd >= k -1) {
      maxSum = Math.max(maxSum, windowSum)
      
      //subtract the element going out
      windowSum -= arr[windowStart]
      
      //slide the window ahead
      windowStart ++
    }
  }
  return maxSum
}


maxSubarrayOfSizeK([2, 1, 5, 1, 3, 2], 3)//9 
maxSubarrayOfSizeK([2, 3, 4, 1, 5], 2)//7 
````
- The time complexity of the above algorithm will be `O(N)`
- The space complexity of the above algorithm will be `O(1)`

## Smallest Subarray with a given sum (easy)

### Sliding Window Approach

````
function smallestSubarrayWithGivenSum(arr, s) {
  //sliding window, BUT the window size is not fixed
  let windowSum = 0
  let minLength = Infinity
  let windowStart = 0
  
  //First, we will add-up elements from the beginning of the array until their sum becomes greater than or equal to ‘S.’
  for(windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    
    //add the next element
    windowSum += arr[windowEnd]
    
    //shrink the window as small as possible
    //until windowSum is small than s
    while(windowSum >= s) {
      //These elements will constitute our sliding window. We are asked to find the smallest such window having a sum greater than or equal to ‘S.’ We will remember the length of this window as the smallest window so far.
      //After this, we will keep adding one element in the sliding window (i.e., slide the window ahead) in a stepwise fashion.
       //In each step, we will also try to shrink the window from the beginning. We will shrink the window until the window’s sum is smaller than ‘S’ again. This is needed as we intend to find the smallest window. This shrinking will also happen in multiple steps; in each step, we will do two things:
  //Check if the current window length is the smallest so far, and if so, remember its length.
      minLength = Math.min(minLength, windowEnd - windowStart + 1)
      
  //Subtract the first element of the window from the running sum to shrink the sliding window.
      windowSum -= arr[windowStart]
      windowStart++
    }
  } 
  if(minLength === Infinity) {
    return 0
  }
  return minLength
}


smallestSubarrayWithGivenSum([2, 1, 5, 2, 3, 2], 7)//2
smallestSubarrayWithGivenSum([2, 1, 5, 2, 8], 7)//1
smallestSubarrayWithGivenSum([3, 4, 1, 1, 6], 8)//3

````

- The time complexity of the above algorithm will be `O(N)`. The outer for loop runs for all elements, and the inner while loop processes each element only once; therefore, the time complexity of the algorithm will be `O(N+N)`), which is asymptotically equivalent to `O(N)`.
- The algorithm runs in constant space `O(1)`.

## Longest Substring with K Distinct Characters (medium)

### Sliding Window Approach

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

## Fruits into Baskets (medium)
https://leetcode.com/problems/fruit-into-baskets/

````
function fruitsInBaskets(fruits) {
  let windowStart = 0; 
  let maxLength = 0; 
  let fruitFrequency = {};
  
  //try to extend the range
  for(let windowEnd = 0; windowEnd < fruits.length; window++) {
    const rightFruit = fruits[windowEnd]
    if(!(rightFruit in fruitFrequency)) {
      fruitFrequency[rightFruit] = 0
    }
    fruitFrequency[rightFruit]++
    //shrink the sliding window, until we are left with '2' fruits in the fruitFrequency hashMap
    while(Object.keys(fruitFrequency).length > 2) {
      const leftFruit = fruits[windowStart];
      fruitFrequency[leftFruit]--
      if(fruitFrequency[leftFruit] === 0) {
        delete fruitFrequency[leftFruit]
      }
      windowStart++
    }
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1)
  }
  return maxLength
}

fruitsInBaskets(['A', 'B', 'C', 'A', 'C'])//3 
fruitsInBaskets(['A', 'B', 'C', 'B', 'B', 'C'])//5
````

## No-repeat Substring (hard)
https://leetcode.com/problems/longest-substring-without-repeating-characters/

````
function nonRepeatSubstring(str) {
  // sliding window with hashmap
  
  let windowStart = 0
  let maxLength = 0
  let charIndexMap = {}
  
  //try to extend the range [windowStart, windowEnd]
  for(let windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const endChar = str[windowEnd]
    //if the map already contains the endChar, 
    //shrink the window from the beginning 
    //so that we only have on occurance of endChar
    if(endChar in charIndexMap) {
      //this is tricky; in the current window, 
      //we will not have any endChar after
      //it's previous index. and if windowStart
      //is already ahead of the last index of
      //endChar, we'll keep windowStart
      windowStart = Math.max(windowStart, charIndexMap[endChar] + 1)
    }
    //insert the endChar into the map
    charIndexMap[endChar] = windowEnd
    //remember the maximum length so far
    maxLength = Math.max(maxLength, windowEnd - windowStart+1)
  } 
  return maxLength
};

nonRepeatSubstring("aabccbb")//3
nonRepeatSubstring("abbbb")//2
nonRepeatSubstring("abccde")//3
````
- The above algorithm’s time complexity will be `O(N)`, where `‘N’` is the number of characters in the input string.
- The algorithm’s space complexity will be `O(K)`, where `K` is the number of distinct characters in the input string. This also means `K<=N`, because in the worst case, the whole string might not have any repeating character, so the entire string will be added to the HashMap. Having said that, since we can expect a fixed set of characters in the input string (e.g., 26 for English letters), we can say that the algorithm runs in fixed space `O(1)`; in this case, we can use a fixed-size array instead of the HashMap.
