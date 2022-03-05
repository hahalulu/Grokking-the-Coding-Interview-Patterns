# Pattern 15: 0-1 Knapsack (Dynamic Programming)

<b>0/1 Knapsack pattern</b> is based on the famous problem with the same name which is efficiently solved using <b>Dynamic Programming (DP)</b>.

In this pattern, we will go through a set of problems to develop an understanding of <b>DP</b>. We will always start with a brute-force recursive solution to see the overlapping subproblems, i.e., realizing that we are solving the same problems repeatedly.

After the recursive solution, we will modify our algorithm to apply advanced techniques of <b>Memoization</b> and <b>Bottom-Up Dynamic Programming</b> to develop a complete understanding of this pattern.

Let’s jump onto our first problem.

## 0/1 Knapsack (medium)
https://leetcode.com/problems/maximum-earnings-from-taxi/

 > Given the weights and profits of `N` items, we are asked to put these items in a knapsack with a capacity `C`. The goal is to get the `maximum profit` out of the knapsack items. Each item can only be selected once, as we don’t have multiple quantities of any item.

Let’s take Merry’s example, who wants to carry some fruits in the knapsack to get `maximum profit`. Here are the weights and profits of the fruits:
- `Items: { Apple, Orange, Banana, Melon }`
- `Weights: { 2, 3, 1, 4 }`
- `Profits: { 4, 5, 3, 7 }`
- `Knapsack capacity: 5`

Let’s try to put various combinations of fruits in the knapsack, such that their total weight is not more than `5`:
- `Apple + Orange (total weight 5) => 9 profit`
- `Apple + Banana (total weight 3) => 7 profit`
- `Orange + Banana (total weight 4) => 8 profit`
- `Banana + Melon (total weight 5) => 10 profit`

This shows that `Banana + Melon` is the best combination as it gives us the `maximum profit`, and the total weight does not exceed the capacity.
#
> Given two integer arrays to represent weights and profits of `N` items, we need to find a subset of these items which will give us maximum profit such that their cumulative weight is not more than a given number `C`. Each item can only be selected once, which means either we put an item in the knapsack or we skip it.

A basic <b>brute-force solution</b> could be to try all combinations of the given items (as we did above), allowing us to choose the one with `maximum profit` and a weight that doesn’t exceed `C`. Take the example of four items `A, B, C, and D`, as shown in the diagram below. To try all the combinations, our algorithm will look like:
![](knapsack.png)

All <b>green boxes</b> have a total weight that is less than or equal to the capacity `7`, and all the <b>red ones</b> have a weight that is more than `7`. The best solution we have is with items `[B, D]` having a total profit of `22` and a total weight of `7`.


### Brute-Force Solution
````
function solveKnapsack(profits, weights, capacity) {
  
  function knapsackRecursive(profits, wights, capacity, currentIndex){
    //check base case
    if(capacity <= 0 || currentIndex >= profits.length) return 0
    
    //recursive call after choosing the element at currentIndex
    // create a new set which INCLUDES item at currentIndex if the total weight does not exceed the capacity, and 
    let currentProfit = 0
    
    if(weights[currentIndex] <= capacity) {
      currentProfit = profits[currentIndex] + knapsackRecursive(profits, weights, capacity-weights[currentIndex] , currentIndex + 1)
    }
    
    // recursively process the remaining capacity and items
    // WITHOUT item at currentIndex
    let currentProfitminusIndexItem = knapsackRecursive(profits, weights, capacity, currentIndex + 1)
    
    // return the set from the above two sets with higher profit 
    return Math.max(currentProfit, currentProfitminusIndexItem)
  }
  
  
  return knapsackRecursive(profits, weights, capacity, 0);
};

console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16], [1, 2, 3, 5], 7)}`);
console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16], [1, 2, 3, 5], 6)}`);
````
#### Time & Space Complexity
- The above algorithm’s time complexity is exponential `O(2ⁿ)`, where `n` represents the total number of items. This can also be confirmed from the above recursion tree. As we can see, we will have a total of `31` 😲 recursive calls – calculated through `(2ⁿ) + (2ⁿ) - 1`, which is asymptotically equivalent to `O(2ⁿ)`.
- The space complexity is `O(n)`. This space will be used to store the recursion stack. Since the recursive algorithm works in a depth-first fashion, which means that we can’t have more than `n` recursive calls on the call stack at any time.

## Overlapping Sub-problems
Let’s visually draw the recursive calls to see if there are any overlapping sub-problems. As we can see, in each recursive call, `profits` and `weights` arrays remain constant, and only `capacity` and `currentIndex` change. For simplicity, let’s denote capacity with `c` and `currentIndex` with `i`:
![](subproblems.png)
We can clearly see that `c:4, i=3` has been called twice. Hence we have an <b>overlapping sub-problems pattern</b>. We can use <b>[Memoization](https://en.wikipedia.org/wiki/Memoization)</b> to solve <b>overlapping sub-problems</b> efficiently.
### Top-down Dynamic Programming with Memoization
<b>[Memoization](https://en.wikipedia.org/wiki/Memoization)</b> is when we store the results of all the previously solved <b>sub-problems</b> and return the results from memory if we encounter a problem that has already been solved.

Since we have two changing values (`capacity` and `currentIndex`) in our recursive `function knapsackRecursive()`, we can use a two-dimensional array to store the results of all the solved sub-problems. As mentioned above, we need to store results for every sub-array (i.e., for every possible index `i`) and every possible capacity `c`.

Here is the code with <b>memoization</b>
````
function solveKnapsack(profits, weights, capacity) {
  const memo = []
  
  function knapsackRecursive(profits, weights, capacity, currentIndex){
    //check base case
    if(capacity <= 0 || currentIndex >= profits.length) return 0
    
    memo[currentIndex] = memo[currentIndex] || []
    
    if(typeof memo[currentIndex][capacity] !== 'undefined') {
      return memo[currentIndex][capacity]
    }
    
    //recursive call after choosing the element at currentIndex
    // create a new set which INCLUDES item at currentIndex if the total weight does not exceed the capacity, and 
    let currentProfit = 0
    
    if(weights[currentIndex] <= capacity) {
      currentProfit = profits[currentIndex] + knapsackRecursive(profits, weights, capacity-weights[currentIndex] , currentIndex + 1)
    }
    
    // recursively process the remaining capacity and items
    // WITHOUT item at currentIndex
    let currentProfitMinusIndexItem = knapsackRecursive(profits, weights, capacity, currentIndex + 1)
    
    // return the set from the above two sets with higher profit 
    memo[currentIndex][capacity] = Math.max(currentProfit, currentProfitMinusIndexItem)
    return memo[currentIndex][capacity]
  }

  return knapsackRecursive(profits, weights, capacity, 0, memo);
};

console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16], [1, 2, 3, 5], 7)}`);
console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16], [1, 2, 3, 5], 6)}`);
````
#### Time & Space Complexity 
- Since our memoization array `memo[profits.length][capacity+1]` stores the results for all subproblems, we can conclude that we will not have more than `N*C` subproblems (where `N` is the number of items and `C` is the knapsack capacity). This means that our time complexity will be `O(N*C)`.
- The above algorithm will use `O(N*C)` space for the memoization array. Other than that, we will use `O(N)` space for the recursion call-stack. So the total space complexity will be `O(N*C + N)`, which is asymptotically equivalent to `O(N*C)`.


## 
## 
## 
## 

## 🌟
## 🌟
## 🌟
