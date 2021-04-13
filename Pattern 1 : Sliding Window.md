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
