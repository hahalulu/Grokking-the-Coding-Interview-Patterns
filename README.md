# Grokking-the-Coding-Interview-Patterns

## Pattern 1 : Sliding Window

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
### Maximum Sum Subarray of Size K (easy)

### Brute Force 
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