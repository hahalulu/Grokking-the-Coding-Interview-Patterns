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
https://leetcode.com/problems/maximum-average-subarray-i/
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
https://leetcode.com/problems/largest-subarray-length-k/
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
https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/

This problem follows the Sliding Window pattern, and we can use a similar dynamic sliding window strategy as discussed in <b>Smallest Subarray with a given sum</b>. We can use a HashMap to remember the frequency of each character we have processed. Here is how we will solve this problem:

1. First, we will insert characters from the beginning of the string until we have `K` distinct characters in the HashMap.
2. These characters will constitute our sliding window. We are asked to find the longest such window having no more than `K` distinct characters. We will remember the length of this window as the longest window so far.
3. After this, we will keep adding one character in the sliding window (i.e., slide the window ahead) in a stepwise fashion.
4. In each step, we will try to shrink the window from the beginning if the count of distinct characters in the HashMap is larger than `K`. We will shrink the window until we have no more than `K` distinct characters in the HashMap. This is needed as we intend to find the longest window.
5. While shrinking, we’ll decrement the character’s frequency going out of the window and remove it from the HashMap if its frequency becomes zero.
6. At the end of each step, we’ll check if the current window length is the longest so far, and if so, remember its length.

````
function longestSubstringWithKdistinct(str, k) {
   // Given a string, find the length of the longest substring in it with no more than K distinct characters.
  let windowStart = 0
  let maxLength = 0
  let charFrequency = {}

  //in the following loop we'll try to extend the range [windowStart, windowEnd]
  for(let windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const endChar = str[windowEnd]
    if(!(endChar in charFrequency)) {
      charFrequency[endChar] = 0
    }
    charFrequency[endChar]++
    //shrink the window until we are left with k distinct characters 
    //in the charFrequency Object
    
    while(Object.keys(charFrequency).length > k) {
      //insert characters from the beginning of the string until we have 'K' distinct characters in the hashMap 
    //these characters will consitutue our sliding window.  We are asked to find the longest such window having no more that K distinct characters.  We will remember the length of the window as the longest window so far
    //we will keep adding on character in the sliding window in a stepwise fashion
      //in each step we will try to shrink the window from the beginning if the count of distinct characters in the hashmap is larger than K. We will shrink the window until we have no more that K distinct characters in the HashMap
      const startChar = str[windowStart]
      charFrequency[startChar]--
      //while shrinking , we will decrement the characters frequency going out of the window and remove it from the HashMap if it's frequency becomes zero
      if(charFrequency[startChar] === 0) {
        delete charFrequency[startChar]
      }
      windowStart++
    }
    //after each step we will check if the current window length is the longest so far, and if so, remember it's length
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1)
  }
    return maxLength
};

longestSubstringWithKdistinct("araaci", 2)//4, The longest substring with no more than '2' distinct characters is "araa".
longestSubstringWithKdistinct("araaci", 1)//2, The longest substring with no more than '1' distinct characters is "aa".
longestSubstringWithKdistinct("cbbebi", 3)//5, The longest substrings with no more than '3' distinct characters are "cbbeb" & "bbebi".
````
- The above algorithm’s time complexity will be `O(N)`, where `N` is the number of characters in the input string. The outer for loop runs for all characters, and the inner while loop processes each character only once; therefore, the time complexity of the algorithm will be `O(N+N)`, which is asymptotically equivalent to `O(N)`
- The algorithm’s space complexity is `O(K)`, as we will be storing a maximum of `K+1` characters in the HashMap.

## Fruits into Baskets (medium)
https://leetcode.com/problems/fruit-into-baskets/

This problem follows the Sliding Window pattern and is quite similar to <b>Longest Substring with K Distinct Characters</b>. 
> In this problem, we need to find the length of the longest subarray with no more than two distinct characters (or fruit types!). 

This transforms the current problem into Longest Substring with K Distinct Characters where K=2.

````
function fruitsInBaskets(fruits) {
  let windowStart = 0; 
  let maxLength = 0; 
  let fruitFrequency = {};
  
  //try to extend the range
  for(let windowEnd = 0; windowEnd < fruits.length; window++) {
    const endFruit = fruits[windowEnd]
    if(!(endFruit in fruitFrequency)) {
      fruitFrequency[endFruit] = 0
    }
    fruitFrequency[endFruit]++
    
    //shrink the sliding window, until we are left with '2' fruits in the fruitFrequency hashMap
    while(Object.keys(fruitFrequency).length > 2) {
      const startFruit = fruits[windowStart];
      fruitFrequency[startFruit]--
      if(fruitFrequency[startFruit] === 0) {
        delete fruitFrequency[startFruit]
      }
      windowStart++
    }
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1)
  }
  return maxLength
}

fruitsInBaskets(['A', 'B', 'C', 'A', 'C'])//3 , We can put 2 'C' in one basket and one 'A' in the other from the subarray ['C', 'A', 'C']
fruitsInBaskets(['A', 'B', 'C', 'B', 'B', 'C'])//5 , We can put 3 'B' in one basket and two 'C' in the other basket. This can be done if we start with the second letter: ['B', 'C', 'B', 'B', 'C']
````
- The above algorithm’s time complexity will be `O(N)`, where `‘N’` is the number of characters in the input array. The outer for loop runs for all characters, and the inner while loop processes each character only once; therefore, the time complexity of the algorithm will be `O(N+N)`, which is asymptotically equivalent to `O(N)`.

- The algorithm runs in constant space `O(1)` as there can be a maximum of three types of fruits stored in the frequency map.

## No-repeat Substring (hard)
https://leetcode.com/problems/longest-substring-without-repeating-characters/

This problem follows the <b>Sliding Window pattern</b>, and we can use a similar dynamic sliding window strategy as discussed in <b>Longest Substring with K Distinct Characters</b>. We can use a HashMap to remember the last index of each character we have processed. Whenever we get a repeating character, we will shrink our sliding window to ensure that we always have distinct characters in the sliding window.

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

## Longest Substring with Same Letters after Replacement (hard)
https://leetcode.com/problems/longest-repeating-character-replacement/

This problem follows the <b>Sliding Window pattern</b>, and we can use a similar dynamic sliding window strategy as discussed in <b>No-repeat Substring</b>. We can use a HashMap to count the frequency of each letter.

- We will iterate through the string to add one letter at a time in the window.
- We will also keep track of the count of the maximum repeating letter in any window (let’s call it `maxRepeatLetterCount`).
- So, at any time, we know that we do have a window with one letter repeating `maxRepeatLetterCount` times; this means we should try to replace the remaining letters.
  - If the remaining letters are less than or equal to `‘k’`, we can replace them all.
  - If we have more than `‘k’` remaining letters, we should shrink the window as we cannot replace more than `‘k’` letters.

While shrinking the window, we don’t need to update `maxRepeatLetterCount` (hence, it represents the maximum repeating count of ANY letter for ANY window). Why don’t we need to update this count when we shrink the window? Since we have to replace all the remaining letters to get the longest substring having the same letter in any window, we can’t get a better answer from any other window even though all occurrences of the letter with frequency `maxRepeatLetterCount` is not in the current window.
````
function lengthOfLongestSubstring(str, k) {
  let windowStart = 0
  let maxLength = 0
  let maxRepeatLetterCount = 0
  let charFrequency = {}
  
  //Try to extend the range [windowStart, windowEnd]
  for(let windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const endChar = str[windowEnd]
    if(!(endChar in charFrequency)) {
      charFrequency[endChar] = 0
    }
    charFrequency[endChar]++
    maxRepeatLetterCount = Math.max(maxRepeatLetterCount, charFrequency[endChar])
    
    //current window size is from windowStart to windowEnd, overall we have a letter which is
    //repeating maxRepeatLetterCount times, this mean we can have a window which has one letter
    //repeating maxRepeatLetterCount times and the remaining letters we should replace
    //if the remaining letters are more than k, it is the time to shrink the window as we
    //are not allowed to replace more than k letters
    if((windowEnd - windowStart + 1 - maxRepeatLetterCount) > k) {
      const startChar = str[windowStart]
      charFrequency[startChar]--
      windowStart++
    }
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1)
  }
  return maxLength
}

lengthOfLongestSubstring("aabccbb", 2)//5, Replace the two 'c' with 'b' to have a longest repeating substring "bbbbb".
lengthOfLongestSubstring("abbcb", 1)//4, Replace the 'c' with 'b' to have a longest repeating substring "bbbb".
lengthOfLongestSubstring("abccde", 1)//3, Replace the 'b' or 'd' with 'c' to have the longest repeating substring "ccc".
````

- The above algorithm’s time complexity will be `O(N)`, where `‘N’` is the number of letters in the input string.
- As we expect only the lower case letters in the input string, we can conclude that the space complexity will be `O(26)` to store each letter’s frequency in the HashMap, which is asymptotically equal to `O(1)`.

## Longest Subarray with Ones after Replacement (hard)
https://leetcode.com/problems/max-consecutive-ones-iii/

> Given an array containing 0s and 1s, if you are allowed to <b>replace no more than ‘k’ 0s with 1s</b>, 
> find the length of the <b>longest contiguous subarray having all 1s</b>.

This problem follows the <b>Sliding Window</b> pattern and is quite similar to <b>Longest Substring with same Letters after Replacement</b>. The only difference is that, in the problem, we only have two characters (1s and 0s) in the input arrays.

Following a similar approach, we’ll iterate through the array to add one number at a time in the window. We’ll also keep track of the maximum number of repeating 1s in the current window (let’s call it `maxOnesCount`). So at any time, we know that we can have a window with 1s repeating `maxOnesCount` time, so we should try to replace the remaining 0s. If we have more than `‘k’` remaining 0s, we should shrink the window as we are not allowed to replace more than `‘k’` 0s.

````
function lengthOfLongestSubstring (arr, k) {
  let windowStart = 0
  let maxLength = 0
  let maxOnesCount = 0
  
  //Try to extend the range [windowStart, windowEnd]
  for(let windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    if(arr[windowEnd] === 1) {
      maxOnesCount++
    }
    
    //current window size is from windowStart to windowEnd, overall we have a 
    //maximum of 1s repeating maxOnesCount times, this means we can have a window 
    //with maxOnesCount 1s and the remaining are 0s which should replace with 1s
    //now, if the remaining 0s are more that k, it is the time to shrink the 
    //window as we are not allowed to replace more than k 0s
    if((windowEnd - windowStart + 1 - maxOnesCount) > k) {
      if(arr[windowStart] === 1) {
        maxOnesCount--
      }
      windowStart++
    }
    
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1)
  }
  
  return maxLength  
}

lengthOfLongestSubstring ([0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1], 2)//6, Replace the '0' at index 5 and 8 to have the longest contiguous subarray of 1s having length 6.
lengthOfLongestSubstring ([0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1], 3)//9, Replace the '0' at index 6, 9, and 10 to have the longest contiguous subarray of 1s having length 9.
````
- The above algorithm’s time complexity will be `O(N)`, where `‘N’` is the count of numbers in the input array.
- The algorithm runs in constant space `O(1)`.

## 🌟 Permutation in a String (hard)
https://leetcode.com/problems/permutation-in-string/

> Given a string and a pattern, find out if the string contains any permutation of the pattern.

<b>Permutation</b> is defined as the re-arranging of the characters of the string. For example, `“abc”` has the following six permutations:
- abc
- acb
- bac
- bca
- cab
- cba

If a string has `‘n’` distinct characters, it will have `n!` permutations.

This problem follows the <b>Sliding Window</b> pattern, and we can use a similar sliding window strategy as discussed in <b>Longest Substring with K Distinct Characters</b>. We can use a HashMap to remember the frequencies of all characters in the given pattern. Our goal will be to match all the characters from this HashMap with a sliding window in the given string. Here are the steps of our algorithm:
- Create a HashMap to calculate the frequencies of all characters in the pattern.
- Iterate through the string, adding one character at a time in the sliding window.
- If the character being added matches a character in the HashMap, decrement its frequency in the map. If the character frequency becomes zero, we got a complete match.
- If at any time, the number of characters matched is equal to the number of distinct characters in the pattern (i.e., total characters in the HashMap), we have gotten our required permutation.
- If the window size is greater than the length of the pattern, shrink the window to make it equal to the pattern’s size. At the same time, if the character going out was part of the pattern, put it back in the frequency HashMap.

````
function findPermutation(str, pattern) {
  //sliding window
  let windowStart = 0
  let isMatch = 0
  let charFrequency = {}
  
 for(i = 0; i < pattern.length; i++) {
   const char = pattern[i]
   if(!(char in charFrequency)) {
     charFrequency[char] = 0
   }
   charFrequency[char]++
 }
  
  //our goal is to math all the characters from charFrequency with the current window
  //try to extend the range [windowStart, windowEnd]
  for(windowEnd = 0; windowEnd < str.length; windowEnd++) {
    const endChar = str[windowEnd]
    if(endChar in charFrequency) {
      //decrement the frequency of the matched character
      charFrequency[endChar]--
      if(charFrequency[endChar] === 0) {
        isMatch++
      }
    }
    if(isMatch === Object.keys(charFrequency).length) {
      return true
    }
    
    //shrink the sliding window
    if(windowEnd >= pattern.length - 1) {
      let startChar = str[windowStart]
      windowStart++
      if(startChar in charFrequency) {
        if(charFrequency[startChar] === 0) {
          isMatch--
        }
        charFrequency[startChar]++
      }
    }
  }
  return false
}

findPermutation("oidbcaf", "abc")//true, The string contains "bca" which is a permutation of the given pattern.
findPermutation("odicf", "dc")//false
findPermutation("bcdxabcdy", "bcdxabcdy")//true
findPermutation("aaacb", "abc")//true, The string contains "acb" which is a permutation of the given pattern.
````

- The above algorithm’s time complexity will be `O(N + M)`, where `‘N’` and `‘M’` are the number of characters in the input string and the pattern, respectively.
- The algorithm’s space complexity is `O(M)` since, in the worst case, the whole pattern can have distinct characters that will go into the HashMap.

## 🌟 String Anagrams (hard)
https://leetcode.com/problems/find-all-anagrams-in-a-string/

Given a string and a pattern, find all anagrams of the pattern in the given string.

Every anagram is a permutation of a string. As we know, when we are not allowed to repeat characters while finding permutations of a string, we get `N!` permutations (or anagrams) of a string having `N` characters. For example, here are the six anagrams of the string `“abc”`:
- abc
- acb
- bac
- bca
- cab
- cba

> Write a function to return a list of starting indices of the anagrams of the pattern in the given string.

## 🌟 Smallest Window containing Substring (hard)
https://leetcode.com/problems/minimum-window-substring/
## 🌟 Words Concatenation (hard)
https://leetcode.com/problems/concatenated-words/


