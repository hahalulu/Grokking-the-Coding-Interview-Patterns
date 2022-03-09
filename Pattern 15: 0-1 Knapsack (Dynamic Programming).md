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
````js
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
````js
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
### Bottom-up Dynamic Programming
Let’s try to populate our ~~memo~~`dp[][]` array from the above solution by working in a <b>bottom-up</b> fashion. Essentially, we want to find the `maximum profit` for every sub-array and every possible capacity. <b>This means that `dp[i][c]` will represent the maximum knapsack profit for capacity `c` calculated from the first `i` items</b>.

So, for each item at index `i` (`0 <= i < items.length`) and capacity `c` (`0 <= c <= capacity`), we have two options:

1. Exclude the item at index `i`. In this case, we will take whatever profit we get from the sub-array excluding this item => `dp[i-1][c]`
2. Include the item at index `i` if its weight is not more than the capacity. In this case, we include its profit plus whatever profit we get from the remaining capacity and from remaining items => `profit[i] + dp[i-1][c-weight[i]]`
Finally, our optimal solution will be maximum of the above two values:

`dp[i][c] = max (dp[i-1][c], profit[i] + dp[i-1][c-weight[i]])`

````js
function solveKnapsack(profits, weights, capacity) {
  //bottom-up dynamic programming approach
  const n = profits.length;

  if (capacity <= 0 || n == 0 || weights.length != n) return 0;

  const dp = Array(n)
    .fill(0)
    .map(() => Array(capacity + 1).fill(0));
 

  //populate the capacity=0 columns; with 0 capacity we have 0 profit
  for (let i = 0; i < n; i++) {
    dp[i][0] = 0;
  }

  //if we have only one weight, we will take it if it is not more than the capacity
  for (let c = 0; c <= capacity; c++) {
    if (weights[0] <= c) {
      dp[0][c] = profits[0];
    }
  }

  //process all sub-arrays for all the capacities
  for (let i = 1; i < n; i++) {
    for (let c = 1; c <= capacity; c++) {
      let profitWithI = 0;
      let profitMinusI = 0;
      //include the item, if its not more than the capacity
      if (weights[i] <= c) profitWithI = profits[i] + dp[i - 1][c - weights[i]];

      //exclude the item
      profitMinusI = dp[i - 1][c];

      //take the maximum
      dp[i][c] = Math.max(profitWithI, profitMinusI);
      // console.log(dp)
    }
  }
  //maximum profit with be at the bottom-right corner
  return dp[n - 1][capacity];
}

console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16],[1, 2, 3, 5],7)}`);
console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16],[1, 2, 3, 5],6)}`);
````
#### Time & Space Complexity 
- The above solution has the time and space complexity of `O(N*C)`, where `N` represents total items, and `C` is the maximum capacity.

### How can we find the selected items?
As we know, the final profit is at the bottom-right corner. Therefore, we will start from there to find the items that will be going into the knapsack.

As you remember, at every step, we had two options: include an item or skip it. If we skip an item, we take the profit from the remaining items (i.e., from the cell right above it); if we include the item, then we jump to the remaining profit to find more items.

Let’s understand this from the above example:
![](dpselected.png)

1. `22` did not come from the top cell (which is `17`); hence we must include the item at index `3` (which is item `D`).
2. Subtract the profit of item `D` from `22` to get the remaining profit `6`. We then jump to profit `6` on the same row.
3. `6` came from the top cell, so we jump to row `2`.
4. Again, `6` came from the top cell, so we jump to row `1`.
5. `6` is different from the top cell, so we must include this item (which is item `B`).
6. Subtract the profit of `B` from `6` to get profit `0`. We then jump to profit `0` on the same row. As soon as we hit zero remaining profit, we can finish our item search.
7. Thus, the items going into the knapsack are `{B, D}`.

Let’s write a function to print the set of items included in the knapsack.
````js
function solveKnapsack(profits, weights, capacity) {
  //bottom-up dynamic programming approach
  const n = profits.length;

  if (capacity <= 0 || n == 0 || weights.length != n) return 0;

  const dp = Array(n)
    .fill(0)
    .map(() => Array(capacity + 1).fill(0));
 

  //populate the capacity=0 columns; with 0 capacity we have 0 profit
  for (let i = 0; i < n; i++) {
    dp[i][0] = 0;
  }

  //if we have only one weight, we will take it if it is not more than the capacity
  for (let c = 0; c <= capacity; c++) {
    if (weights[0] <= c) {
      dp[0][c] = profits[0];
    }
  }

  //process all sub-arrays for all the capacities
  for (let i = 1; i < n; i++) {
    for (let c = 1; c <= capacity; c++) {
      let profitWithI = 0;
      let profitMinusI = 0;
      //include the item, if its not more than the capacity
      if (weights[i] <= c) profitWithI = profits[i] + dp[i - 1][c - weights[i]];

      //exclude the item
      profitMinusI = dp[i - 1][c];

      //take the maximum
      dp[i][c] = Math.max(profitWithI, profitMinusI);
      
    }
  }
  
  //**function to print the set of items included in the knapsack**//
  let selectedWeights = ''
  let totalProfit = dp[weights.length-1][capacity]
  let remainingCapacity = capacity
  for(let i = weights.length-1; i > 0; i--){
    if(totalProfit != dp[i-1][remainingCapacity]){
      selectedWeights = `{${weights[i]}lbs @ $${profits[i]}}${selectedWeights}`
      remainingCapacity -= weights[i]
      totalProfit -= profits[i]
    }
  }
  
  if(totalProfit != 0) selectedWeights = `${weights[0]} ${selectedWeights}`
  
  console.log(`Selected weights : ${selectedWeights} with Total knapsack profit of ---> $ ${dp[n-1][capacity]}`)
  
  
  //maximum profit with be at the bottom-right corner
  return dp[n - 1][capacity];
}

console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16],[1, 2, 3, 5],7)}`);
console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16],[1, 2, 3, 5],6)}`);
````

### Challenge
Can we improve our <b>bottom-up DP</b> solution even further? Can you find an algorithm that has `O(C)` space complexity?
````js
function solveKnapsack(profits, weights, capacity) {
  //optimal O(C) bottom-up dynamic programming approach
  const n = profits.length;

  if (capacity <= 0 || n == 0 || weights.length != n) return 0;

  //we only need one previous row to find the optimal solutin,
  //overall we need 2 rows
  //the above solution is similar to the previous solution
  //the only difference is that
  //we use i%2 instead of i and (i-1)%2 instead of i-1
  const dp = Array(2)
    .fill(0)
    .map(() => Array(capacity + 1).fill(0));

  //if we have only one weight, we will take it if it is not more than the capacity
  for (let c = 0; c <= capacity; c++) {
    if (weights[0] <= c) {
      dp[0][c] = dp[1][c] = profits[0];
    }
  }

  //process all sub-arrays for all the capacities
  for (let i = 1; i < n; i++) {
    for (let c = 1; c <= capacity; c++) {
      let profitWithI = 0;
      let profitMinusI = 0;
      //include the item, if its not more than the capacity
      if (weights[i] <= c)
        profitWithI = profits[i] + dp[(i - 1) % 2][c - weights[i]];

      //exclude the item
      profitMinusI = dp[(i - 1) % 2][c];

      //take the maximum
      dp[i % 2][c] = Math.max(profitWithI, profitMinusI);
    }
  }
  //**function to print the set of items included in the knapsack**
  let selectedWeights = '';
  let totalProfit = dp[(weights.length - 1) % 2][capacity];
  let remainingCapacity = capacity;
  for (let i = weights.length - 1; i > 0; i--) {
    if (totalProfit != dp[(i - 1) % 2][remainingCapacity]) {
      selectedWeights = `{${weights[i]}lbs @ $${profits[i]}}${selectedWeights}`;
      remainingCapacity -= weights[i];
      totalProfit -= profits[i];
    }
  }

  if (totalProfit != 0) selectedWeights = `${weights[0]} ${selectedWeights}`;

  console.log(
    `Selected weights : ${selectedWeights} with Total knapsack profit of ---> $ ${
      dp[(n - 1) % 2][capacity]
    }`
  );

  //maximum profit with be at the bottom-right corner
  return dp[(n - 1) % 2][capacity];
}

console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16],[1, 2, 3, 5],7)}`);
console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16],[1, 2, 3, 5],6)}`);
````

The solution above is similar to the previous solution; the only difference is that we use `i%2` instead of `i` and `(i-1)%2` instead of `i-1`. This solution has a <b>space complexity</b> of `O(2*C) = O(C)`, where `C` is the knapsack’s maximum capacity.

This <b>space optimization solution</b> can also be implemented using a single array. It is a bit tricky, but the intuition is to use the same array for the previous and the next iteration!

If you see closely, we need two values from the previous iteration: `dp[c]` and `dp[c-weight[i]]`

Since our inner loop is iterating over `c:0-->capacity`, let’s see how this might affect our two required values:
1. When we access `dp[c]`, it has not been overridden yet for the current iteration, so it should be fine.
2. `dp[c-weight[i]]` might be overridden if `weight[i] > 0`. Therefore we can’t use this value for the current iteration.

To solve the second case, we can change our inner loop to process in the reverse direction: `c:capacity-->0`. This will ensure that whenever we change a value in `dp[]`, we will not need it again in the current iteration.

````js
function solveKnapsack(profits, weights, capacity) {
  //space optimization solution, O(C) bottom-up dynamic programming approach
  const n = profits.length;

  if (capacity <= 0 || n == 0 || weights.length != n) return 0;

  //we only need one previous row to find the optimal solutin,
  //overall we need 2 rows
  //the above solution is similar to the previous solution
  //the only difference is that
  //we use i%2 instead of i and (i-1)%2 instead of i-1
  const dp = Array(capacity + 1).fill(0);

  //if we have only one weight, we will take it if it is not more than the capacity
  for (let c = 0; c <= capacity; c++) {
    if (weights[0] <= c) {
      dp[c] = profits[0];
    }
  }

  //process all sub-arrays for all the capacities
  for (let i = 1; i < n; i++) {
    for (let c = capacity; c >= 0; c--) {
      let profitWithI = 0;
      let profitMinusI = 0;
      //include the item, if its not more than the capacity
      if (weights[i] <= c)
        profitWithI = profits[i] + dp[c - weights[i]];

      //exclude the item
      profitMinusI = dp[c];

      //take the maximum
      dp[c] = Math.max(profitWithI, profitMinusI);
    }
  }
  //**function to print the set of items included in the knapsack**
  let selectedWeights = '';
  let totalProfit = dp[capacity];
  let remainingCapacity = capacity;
  //*look into this for loop
  // for (let i = weights.length - 1; i > 0; i--) {
  //   if (totalProfit != dp[(i - 1) % 2][remainingCapacity]) {
  //     selectedWeights = `{${weights[i]}lbs @ $${profits[i]}}${selectedWeights}`;
  //     remainingCapacity -= weights[i];
  //     totalProfit -= profits[i];
  //   }
  // }

  // if (totalProfit != 0) selectedWeights = `${selectedWeights}`;
  console.log(`Selected weights : ${selectedWeights} with Total knapsack profit of ---> $ ${dp[capacity]}`);

  //maximum profit with be at the bottom-right corner
  return dp[capacity];
}

console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16],[1, 2, 3, 5],7)}`);
console.log(`Total knapsack profit: ---> $${solveKnapsack([1, 6, 10, 16],[1, 2, 3, 5],6)}`);
````
## Equal Subset Sum Partition (medium)
https://leetcode.com/problems/partition-equal-subset-sum/

>Given a set of positive numbers, find if we can partition it into two subsets such that the sum of elements in both subsets is equal.

This problem follows the <b>0/1 Knapsack pattern</b>. A basic <b>brute-force</b> solution could be to try all combinations of partitioning the given numbers into two sets to see if any pair of sets has an equal sum.

Assume that `S` represents the total sum of all the given numbers. Then the two equal subsets must have a sum equal to `S/2`. This essentially transforms our problem to: <i>"Find a subset of the given numbers that has a total sum of S/2"</i>.

So our <b>brute-force</b> algorithm will look like:

````js
function canPartition (num) {
  //brute force
  let sum = 0
  for(let i = 0; i < num.length; i++) sum += num[i]
  
  //if sum is an odd number, we can't have two subset with equal sum
  if(sum % 2 !== 0) return false
  
  return canPartitionRecursive (num, sum/2, 0)

  function canPartitionRecursive (num, sum, currentIndex){
    //recursive base case check
    if(sum === 0) return true
    
    if(num.length === 0 || currentIndex >= num.length) return false
    
    //recursive call after choosing the number at currentIndex
    //if the number at currentIndex exceed the sum, we shouldn't process
    if(num[currentIndex] <= sum){
      if(canPartitionRecursive (num, sum-num[currentIndex], currentIndex+1))
      return true
    }
    
    //recursive call after excluding the number at currentIndex
    return canPartitionRecursive (num, sum, currentIndex+1)
  
  }
  return false
};

console.log(`Can partition: ${canPartition ([1, 2, 3, 4])}`)//True
//The given set can be partitioned into two subsets with equal sum: {1, 4} & {2, 3}
console.log(`Can partition: ${canPartition ([1, 1, 3, 4, 7])}`)//True
//The given set can be partitioned into two subsets with equal sum: {1, 3, 4} & {1, 7}
console.log(`Can partition: ${canPartition ([2, 3, 4, 6])}`)//False
//The given set cannot be partitioned into two subsets with equal sum.
````
- The time complexity of the above algorithm is exponential `O(2ⁿ)`, where `n` represents the total number. 
- The space complexity is `O(n)`, which will be used to store the recursion stack.

### Top-down Dynamic Programming with Memoization
We can use memoization to overcome the overlapping sub-problems. As stated in previous lessons, memoization is when we store the results of all the previously solved sub-problems so we can return the results from memory if we encounter a problem that has already been solved.

Since we need to store the results for every subset and for every possible sum, therefore we will be using a two-dimensional array to store the results of the solved sub-problems. The first dimension of the array will represent different subsets and the second dimension will represent different `sums` that we can calculate from each subset. These two dimensions of the array can also be inferred from the two changing values (`sum` and `currentIndex`) in our recursive function `canPartitionRecursive()`.

Here is the code for <b>Top-down Dynamic Programming with Memoization</b>:
````js
function canPartition (num) {
  //Top-down DP with memoization
  let sum = 0
  for(let i = 0; i < num.length; i++) sum += num[i]

  //if sum is an odd number, we can't have two subset with equal sum
  if(sum % 2 !== 0) return false
  
  const dp = []
  return canPartitionRecursive (num, sum/2, 0)

  function canPartitionRecursive (dp, num, sum, currentIndex){
    //recursive base case check
    if(sum === 0) return true
    
    if(num.length === 0 || currentIndex >= num.length) return false
    
    dp[currentIndex] = dp[currentIndex] || []
    
    //if we have not already processed a similar problem
    if(typeof dp[currentIndex][sum] === 'undefined'){
      //recursive call after choosing the number at currentIndex
      //if the number at currentIndex exceed the sum, we shouldn't process
      if(num[currentIndex] <= sum){
      if(canPartitionRecursive (dp, num, sum-num[currentIndex], currentIndex+1))
        dp[currentIndex][sum] = true
        
        return true
        
      }  
    }
    
    //recursive call after excluding the number at currentIndex
    return dp[currentIndex][sum] = canPartitionRecursive (dp, num, sum, currentIndex+1)  
  }
  return dp[currentIndex][sum]
};



console.log(`Can partition: ${canPartition ([1, 2, 3, 4])}`)//True
//The given set can be partitioned into two subsets with equal sum: {1, 4} & {2, 3}
console.log(`Can partition: ${canPartition ([1, 1, 3, 4, 7])}`)//True
//The given set can be partitioned into two subsets with equal sum: {1, 3, 4} & {1, 7}
console.log(`Can partition: ${canPartition ([2, 3, 4, 6])}`)//False
//The given set cannot be partitioned into two subsets with equal sum.
````
- The above algorithm has the time and space complexity of `O(N*S)`, where `N` represents total numbers and `S` is the total sum of all the numbers.
### Bottom-up Dynamic Programming
Let’s try to populate our `dp[][]` array from the above solution by working in a <b>bottom-up</b> fashion. Essentially, we want to find if we can make all possible sums with every subset. This means, `dp[i][s]` will be `true` if we can make the sum `s` from the first `i` numbers.

So, for each number at index `i` (`0 <= i < num.length`) and sum `s` (`0 <= s <= S/2`), we have two options:
1. Exclude the number. In this case, we will see if we can get `s` from the subset excluding this number: `dp[i-1][s]`
2. Include the number if its value is not more than `s`. In this case, we will see if we can find a subset to get the remaining sum: `dp[i-1][s-num[i]]`
If either of the two above scenarios is `true`, we can find a subset of numbers with a sum equal to `s`.

Let’s start with our <i>base case of zero capacity</i>:

From the above visualization, we can clearly see that it is possible to partition the given set into two subsets with equal sums, as shown by bottom-right cell: `dp[3][5] => T`

````js
function canPartition (num) {
  //Bottom-up Dynamic Programming
  const n = num.length
  
  let sum = 0
  for(let i = 0; i < num.length; i++) sum += num[i]
 
  //if sum is an odd number, we can't have two subset with equal sum
  if(sum % 2 !== 0) return false
  
  //we are trying to find a subset of given numbers that has a total sum of sum/2
  sum /= 2
  
  const dp = Array(n).fill(false).map(()=> Array(sum +1).fill(false))
  
  //populate the sum = 0 columns, as can always for 0 sum with an empty set
  for(let i = 0; i < n; i++) dp[i][0] = true
  
  //with only one number, we can form a subset when he required sum is equal to its value
  for(let s =1; s<= sum; s++){
    dp[0][s] = num[0] == s
  }
  
  //process all subsets for all sums
  for(let i =1; i < n; i++){
    for(let s = 1; s <= sum; s++) {
      //if we can get the sum s with the number at index i
      if(dp[i-1][s]){
        dp[i][s] = dp[i-1][s]
      } else if(s >= num[i]){
        //else if we can find a subset to get the remaining sum
        dp[i][s] = dp[i-1][s-num[i]]
      }
    }
  }
  
  //the bottom right corner will have our answer
  return dp[n-1][sum]
};

console.log(`Can partition: ${canPartition ([1, 2, 3, 4])}`)//True
//The given set can be partitioned into two subsets with equal sum: {1, 4} & {2, 3}
console.log(`Can partition: ${canPartition ([1, 1, 3, 4, 7])}`)//True
//The given set can be partitioned into two subsets with equal sum: {1, 3, 4} & {1, 7}
console.log(`Can partition: ${canPartition ([2, 3, 4, 6])}`)//False
//The given set cannot be partitioned into two subsets with equal sum.
````
- The above solution the has time and space complexity of `O(N*S)`, where `N` represents total numbers and `S` is the total sum of all the numbers.

## 🔍 Subset Sum (medium)
https://www.techiedelight.com/subset-sum-problem/
> Given a set of positive numbers, determine if a subset exists whose sum is equal to a given number `S`.

This problem follows the <b>0/1 Knapsack pattern</b> and is quite similar to <b>Equal Subset Sum Partition</b>. A basic <b>brute-force</b> solution could be to try all subsets of the given numbers to see if any set has a sum equal to `S`.

So our <b>brute-force</b> algorithm will look like:
 ````js
 for each number 'i' 
  create a new set which INCLUDES number 'i' if it does not exceed 'S', and recursively 
     process the remaining numbers
  create a new set WITHOUT number 'i', and recursively process the remaining numbers 
return true if any of the above two sets has a sum equal to 'S', otherwise return false
````
Since this problem is quite similar to <b>Equal Subset Sum Partition</b>, let’s jump directly to the <b>bottom-up dynamic programming</b> solution.

We’ll try to find if we can make all possible sums with every subset to populate the array `dp[TotalNumbers][S+1]`.

For every possible sum `s` (where `0 <= s <= S`), we have two options:

1. Exclude the number. In this case, we will see if we can get the sum `s` from the subset excluding this number => `dp[index-1][s]`
2. Include the number if its value is not more than `s`. In this case, we will see if we can find a subset to get the remaining sum => `dp[index-1][s-num[index]]`
 
If either of the above two scenarios returns `true`, we can find a subset with a sum equal to `s`.
 
 Here is the code for our <b>bottom-up dynamic programming</b> approach:
 ````js
 function canPartition(nums, sum) {
  //bottom-up dynamic programming approach
  let n = nums.length;
  
  const dp = Array(n)
    .fill(false)
    .map(() => Array(sum + 1).fill(false));

  //populate the sum=0 columns, as we can always for 0 sum with an empty set
  for (let i = 0; i < n; i++) dp[i][0] = true;

  //with only one number, we can form a subset only when the required sum is equal to its value
  for (let s = 1; s <= sum; s++) dp[0][s] = nums[0] === s;

  //process all subsets for all lsum
  for (let i = 1; i < nums.length; i++) {
    for (let s = 1; s <= sum; s++) {
      //if we can get the sum s without the number at index i
      if (dp[i - 1][s]) {
        dp[i][s] = dp[i - 1][s];
      } else if (s >= nums[i]) {
        //else include the number and see if we can find a subset to get the remaining sum
        dp[i][s] = dp[i - 1][s - nums[i]];
      }
    }
  }
  //the bottom right corner will have our answer
  return dp[nums.length - 1][sum];
}

console.log(`Can partitioning be done: ---> ${canPartition([1, 2, 3, 4], 6)}`);
//True
//The given set has a subset whose sum is '6': {1, 2, 3}

console.log(
  `Can partitioning be done: ---> ${canPartition([1, 2, 7, 1, 5], 10)}`
);

//True
//The given set has a subset whose sum is '10': {1, 2, 7}

console.log(`Can partitioning be done: ---> ${canPartition([1, 3, 4, 8], 6)}`);
//False
//The given set does not have any subset whose sum is equal to '6'.
 ````
 
 - The above solution has the time and space complexity of `O(N*S)`, where `N` represents total numbers and `S` is the required sum.

### Challenge
- [ ] Can we improve our <b>bottom-up DP</b> solution even further? Can you find an algorithm that has `O(S)` space complexity?
````js
function canPartition(nums, sum) {
  //O(S) space bottom-up dynamic programming approach
  let n = nums.length;

  const dp = Array(sum + 1).fill(false);

  //sum=0, as we can always have 0 sum with an empty set
  dp[0] = true;

  //with only one number, we can form a subset only when the required sum is equal to its value
  for (let s = 1; s <= sum; s++) dp[s] = nums[0] == s;

  //process all subsets for all lsum
  for (let i = 1; i < nums.length; i++) {
    for (let s = sum; s >= 0; s--) {
      // if dp[s]==true, this means we can get the sum s without
      //num[i], then move on to the next number else we can include num[i]
      //and see if e can find a subset to get the remaining sum

      if (!dp[s] && s >= nums[i]) {
        //else include the number and see if we can find a subset to get the remaining sum
        dp[s] = dp[s - nums[i]];
      }
    }
  }

  return dp[sum];
}

console.log(`Can partitioning be done: ---> ${canPartition([1, 2, 3, 4], 6)}`);
//True
//The given set has a subset whose sum is '6': {1, 2, 3}

console.log(
  `Can partitioning be done: ---> ${canPartition([1, 2, 7, 1, 5], 10)}`
);

//True
//The given set has a subset whose sum is '10': {1, 2, 7}

console.log(`Can partitioning be done: ---> ${canPartition([1, 3, 4, 8], 6)}`);
//False
//The given set does not have any subset whose sum is equal to '6'.

````

## Minimum Subset Sum Difference (hard)
https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/
> Given a set of positive numbers, partition the set into two subsets with minimum difference between their subset sums.

This problem follows the <b>0/1 Knapsack pattern</b> and can be converted into a <b>Subset Sum</b> problem.

Let’s assume `S1` and `S2` are the two desired subsets. A basic <b>brute-force</b> solution could be to try adding each element either in `S1` or `S2` in order to find the combination that gives the minimum sum difference between the two sets.

So our <b>brute-force</b> algorithm will look like:
````js
for each number 'i' 
  add number 'i' to S1 and recursively process the remaining numbers
  add number 'i' to S2 and recursively process the remaining numbers
return the minimum absolute difference of the above two sets 
````
Here is the code for the <b>brute-force</b> solution:

````js
function canPartition(nums) {
  //brute force

  function canPartitionRecursive(nums, currentIndex, sum1, sum2) {
    //recursive base check
    if (currentIndex === nums.length) return Math.abs(sum1 - sum2);

    //recursive call after including the number at the
    //currentIndex in the first set
    const difference1 = canPartitionRecursive(
      nums,
      currentIndex + 1,
      sum1 + nums[currentIndex],
      sum2
    );

    //recursive call after including the number at the
    //currentIndex in the second set
    const difference2 = canPartitionRecursive(
      nums,
      currentIndex + 1,
      sum1,
      sum2 + nums[currentIndex]
    );

    return Math.min(difference1, difference2);
  }
  return canPartitionRecursive(nums, 0, 0, 0);
}

console.log(`Can partitioning be done: ---> ${canPartition([1, 2, 3, 9])}`);
//3
//We can partition the given set into two subsets where minimum absolute difference between the sum of numbers is '3'. Following are the two subsets: {1, 2, 3} & {9}.

console.log(`Can partitioning be done: ---> ${canPartition([1, 2, 7, 1, 5])}`);
//0
//We can partition the given set into two subsets where minimum absolute difference between the sum of number is '0'. Following are the two subsets: {1, 2, 5} & {7, 1}.

console.log(`Can partitioning be done: ---> ${canPartition([1, 3, 100, 4])}`);
//92
//We can partition the given set into two subsets where minimum absolute difference between the sum of numbers is '92'. Here are the two subsets: {1, 3, 4} & {100}.
````
- Because of the two recursive calls, the time complexity of the above algorithm is exponential `O(2ⁿ)`, where `n` represents the total number. 
- The space complexity is `O(n)` which is used to store the recursion stack.

### Top-down Dynamic Programming with Memoization
We can use <b>memoization</b> to overcome the overlapping sub-problems.

We will be using a two-dimensional array to store the results of the solved sub-problems. We can uniquely identify a sub-problem from `currentIndex` and `sum1` as `sum2` will always be the sum of the remaining numbers.
````js
function canPartition(nums) {
  //Top-down Dynamic Programming with Memoization
  let sum = 0;
  for (let i = 0; i < nums.length; i++) sum += nums[i];
  const dp = [];

  function canPartitionRecursive(nums, currentIndex, sum1, sum2) {
    //recursive base check
    if (currentIndex === nums.length) return Math.abs(sum1 - sum2);

    dp[currentIndex] = dp[currentIndex] || [];

    //check if we have not already process similar problem
    if (typeof dp[currentIndex][sum1] === 'undefined') {
      //recursive call after including the number at the
      //currentIndex in the first set
      const difference1 = canPartitionRecursive(
        nums,
        currentIndex + 1,
        sum1 + nums[currentIndex],
        sum2
      );

      //recursive call after including the number at the
      //currentIndex in the second set
      const difference2 = canPartitionRecursive(
        nums,
        currentIndex + 1,
        sum1,
        sum2 + nums[currentIndex]
      );
      dp[currentIndex][sum1] = Math.min(difference1, difference2);
    }
    return dp[currentIndex][sum1];
  }

  return canPartitionRecursive(nums, 0, 0, 0);
}

console.log(`Can partitioning be done: ---> ${canPartition([1, 2, 3, 9])}`);
//3
//We can partition the given set into two subsets where minimum absolute difference between the sum of numbers is '3'. Following are the two subsets: {1, 2, 3} & {9}.

console.log(`Can partitioning be done: ---> ${canPartition([1, 2, 7, 1, 5])}`);
//0
//We can partition the given set into two subsets where minimum absolute difference between the sum of number is '0'. Following are the two subsets: {1, 2, 5} & {7, 1}.

console.log(`Can partitioning be done: ---> ${canPartition([1, 3, 100, 4])}`);
//92
//We can partition the given set into two subsets where minimum absolute difference between the sum of numbers is '92'. Here are the two subsets: {1, 3, 4} & {100}.
````
### Bottom-up Dynamic Programming
Let’s assume `S` represents the total sum of all the numbers. So, in this problem, we are trying to find a subset whose sum is as close to `S/2` as possible, because if we can partition the given set into two subsets of an equal sum, we get the minimum difference, i.e. zero. This transforms our problem to <b>Subset Sum</b>, where we try to find a subset whose sum is equal to a given number-- `S/2` in our case. If we can’t find such a subset, then we will take the subset which has the sum closest to `S/2`. This is easily possible, as we will be calculating all possible sums with every subset.

Essentially, we need to calculate all the possible sums up to `S/2` for all numbers. So how can we populate the array `db[TotalNumbers][S/2+1]` in the bottom-up fashion?

For every possible sum `s` (where `0 <= s <= S/2`), we have two options:

1. Exclude the number. In this case, we will see if we can get the sum `s` from the subset excluding this `number => dp[index-1][s]`
2. Include the number if its value is not more than `s`. In this case, we will see if we can find a subset to get the remaining `sum => dp[index-1][s-num[index]]`

If either of the two above scenarios is `true`, we can find a subset with a sum equal to `s`. We should dig into this before we can learn how to find the closest subset.

Let’s draw this visually, with the example input `{1, 2, 3, 9}`. Since the total sum is `15`, we will try to find a subset whose sum is equal to the half of it, i.e. `7`.
`[](./subsetsum.jpg)

The above visualization tells us that it is not possible to find a subset whose sum is equal to `7`. So what is the closest subset we can find? We can find the subset if we start moving backwards in the last row from the bottom right corner to find the first `T`. The first `T` in the diagram above is the sum `6`, which means that we can find a subset whose sum is equal to `6`. This means the other set will have a sum of `9` and the minimum difference will be `3`.

Here is the code for our <b>bottom-up dynamic programming</b> approach:
````js
function canPartition(nums) {
  //bottom-up dynamic programming
  let n = nums.length;
  let sum = 0;
  for (let i = 0; i < n; i++) sum += nums[i];

  const requiredSum = Math.floor(sum / 2);
  const dp = Array(n)
    .fill(false)
    .map(() => Array(requiredSum + 1).fill(false));

  //populage the sum=0 columns, as we can always form 0 sum with empty set
  for (let i = 0; i < n; i++) dp[i][0] = true;

  //with only only number, we can form a subset only when the reuired sum is eual to that number
  for (let s = 1; s <= requiredSum; s++) {
    dp[0][s] = nums[0] == s;
  }

  //process all subsets for all sums
  for (let i = 1; i < n; i++) {
    for (let s = 1; s <= requiredSum; s++) {
      // if we can get the sum 's' without the number at index 'i'
      if (dp[i - 1][s]) {
        dp[i][s] = dp[i - 1][s];
      } else if (s >= nums[i]) {
        // else include the number and see if we can find a subset to get the remaining sum
        dp[i][s] = dp[i - 1][s - nums[i]];
      }
    }
  }

  let sum1 = 0;
  // Find the largest index in the last row which is true
  for (let i = requiredSum; i >= 0; i--) {
    if (dp[n - 1][i] === true) {
      sum1 = i;
      break;
    }
  }

  const sum2 = sum - sum1;
  return Math.abs(sum2 - sum1);
}

console.log(`Can partitioning be done: ---> ${canPartition([1, 2, 3, 9])}`);
//3
//We can partition the given set into two subsets where minimum absolute difference between the sum of numbers is '3'. Following are the two subsets: {1, 2, 3} & {9}.

console.log(`Can partitioning be done: ---> ${canPartition([1, 2, 7, 1, 5])}`);
//0
//We can partition the given set into two subsets where minimum absolute difference between the sum of number is '0'. Following are the two subsets: {1, 2, 5} & {7, 1}.

console.log(`Can partitioning be done: ---> ${canPartition([1, 3, 100, 4])}`);
//92
//We can partition the given set into two subsets where minimum absolute difference between the sum of numbers is '92'. Here are the two subsets: {1, 3, 4} & {100}.
````
- The above solution has the time and space complexity of `O(N*S)`, where `N` represents total numbers and `S` is the total sum of all the numbers.

## 🌟Count of Subset Sum (hard)
https://leetcode.com/problems/combination-sum/
> Given a set of positive numbers, find the total number of subsets whose sum is equal to a given number `S`.

This problem follows the <b>0/1 Knapsack pattern</b> and is quite similar to <b>Subset Sum</b>. The only difference in this problem is that we need to count the number of subsets, whereas in <b>Subset Sum</b> we only wanted to know if a subset with the given sum existed.

A basic <b>brute-force</b> solution could be to try all subsets of the given numbers to count the subsets that have a sum equal to `S`. So our <b>brute-force</b> algorithm will look like:
````js
for each number 'i' 
  create a new set which includes number 'i' if it does not exceed 'S', and recursively   
      process the remaining numbers and sum
  create a new set without number 'i', and recursively process the remaining numbers 
return the count of subsets who has a sum equal to 'S'
````
Here is the code for the <b>brute-force</b> solution:
````js
function countSubsets (num, sum) {
  
  function countSubsetsRecursive (num, sum, currentIndex){
    //recursive base case check
    if(sum === 0) return 1
    
    if(num.length === 0 || currentIndex >= num.length) return 0
    
    //recursive call after selecting the number at the currentIndex
    //if the number at currentIndex exceeds the sum, we shouldn't process this
    let sum1 = 0
    if(num[currentIndex] <= sum) {
      sum1 = countSubsetsRecursive (num, sum-num[currentIndex], currentIndex+1)
    }
    
    //recursive call after excluding the number at currentIndex
    const sum2 = countSubsetsRecursive (num, sum, currentIndex+1)
    return sum1 + sum2
    
  }

  return countSubsetsRecursive (num, sum, 0);
};

console.log(`Count of subset sum is: ---> ${countSubsets([1, 1, 2, 3], 4)}`);
// 3
//The given set has '3' subsets whose sum is '4': {1, 1, 2}, {1, 3}, {1, 3}
//Note that we have two similar sets {1, 3}, because we have two '1' in our input.
console.log(`Count of subset sum is: ---> ${countSubsets([1, 2, 7, 1, 5], 9)}`);
//3
//The given set has '3' subsets whose sum is '9': {2, 7}, {1, 7, 1}, {1, 2, 1, 5}
````
- The time complexity of the above algorithm is exponential `O(2ⁿ)`, where `n` represents the total number. 
- The space complexity is `O(n)` which is used to store the recursion stack.
### Top-down Dynamic Programming with Memoization 
We can use <b>memoization</b> to overcome the overlapping sub-problems. We will be using a two-dimensional array to store the results of solved sub-problems. As mentioned above, we need to store results for every subset and for every possible sum.
````js
function countSubsets(num, sum) {
  const dp = [];

  function countSubsetsRecursive(num, sum, currentIndex) {
    //recursive base case check
    if (sum === 0) return 1;

    if (num.length === 0 || currentIndex >= num.length) return 0;

    dp[currentIndex] = dp[currentIndex] || [];

    //check if we have not already processed a similar problem
    if (typeof dp[currentIndex][sum] === 'undefined') {
      //recursive call after selecting the number at the currentIndex
      //if the number at currentIndex exceeds the sum, we shouldn't process this
      let sum1 = 0;
      if (num[currentIndex] <= sum) {
        sum1 = countSubsetsRecursive(
          num,
          sum - num[currentIndex],
          currentIndex + 1
        );
      }
      //recursive call after excluding the number at currentIndex
      const sum2 = countSubsetsRecursive(num, sum, currentIndex + 1);
      dp[currentIndex][sum] = sum1 + sum2;
    }

    return dp[currentIndex][sum];
  }

  return countSubsetsRecursive(num, sum, 0);
}

console.log(`Count of subset sum is: ---> ${countSubsets([1, 1, 2, 3], 4)}`);
// 3
//The given set has '3' subsets whose sum is '4': {1, 1, 2}, {1, 3}, {1, 3}
//Note that we have two similar sets {1, 3}, because we have two '1' in our input.
console.log(`Count of subset sum is: ---> ${countSubsets([1, 2, 7, 1, 5], 9)}`);
//3
//The given set has '3' subsets whose sum is '9': {2, 7}, {1, 7, 1}, {1, 2, 1, 5}
````
### Bottom-up Dynamic Programming
We will try to find if we can make all possible sums with every subset to populate the array `db[TotalNumbers][S+1]`.

So, at every step we have two options:
1. Exclude the number. Count all the subsets without the given number up to the given `sum => dp[index-1][sum]`
2. Include the number if its value is not more than the `sum`. In this case, we will count all the subsets to get the remaining `sum => dp[index-1][sum-num[index]]`

To find the total sets, we will add both of the above two values:
````
    dp[index][sum] = dp[index-1][sum] + dp[index-1][sum-num[index]])
````
    
Here is the code for our <b>bottom-up dynamic programming</b> approach:
````js
function countSubsets(num, sum) {
  //bottom-up dynamic programming approach
  const n = num.length;
  const dp = Array(n)
    .fill(0)
    .map(() => Array(sum + 1).fill(0));

  //populate the sum=0 columns, as we will always have an empty set for zero sum
  for (let i = 0; i < n; i++) {
    dp[i][0] = 1;
  }

  //with only one number, we can form a subset only when the required sum is equal to its value
  for (let s = 1; s <= sum; s++) {
    dp[0][s] = num[0] == s ? 1 : 0;
  }

  //process all subsets for all sums
  for (let i = 1; i < num.length; i++) {
    for (let s = 1; s <= sum; s++) {
      //exclude the number
      dp[i][s] = dp[i - 1][s];
      //include the number, if it does not exceed the sum
      if (s >= num[i]) {
        dp[i][s] += dp[i - 1][s - num[i]];
      }
    }
  }

  //the bottom-right corner will have our answer
  return dp[num.length - 1][sum];
}

console.log(`Count of subset sum is: ---> ${countSubsets([1, 1, 2, 3], 4)}`);
// 3
//The given set has '3' subsets whose sum is '4': {1, 1, 2}, {1, 3}, {1, 3}
//Note that we have two similar sets {1, 3}, because we have two '1' in our input.
console.log(`Count of subset sum is: ---> ${countSubsets([1, 2, 7, 1, 5], 9)}`);
//3
//The given set has '3' subsets whose sum is '9': {2, 7}, {1, 7, 1}, {1, 2, 1, 5}
````
- The above solution has the time and space complexity of `O(N*S)`, where `N` represents total numbers and `S` is the desired sum.

### Challenge
- [ ] Can we improve our <b>bottom-up DP</b> solution even further? Can you find an algorithm that has `O(S)` space complexity?
````js
function countSubsets(num, sum) {
  //O(S) bottom-up dynamic programming approach
  const n = num.length;
  const dp = Array(sum + 1).fill(0);
  dp[0] = 1;

  // with only one number, we can form a subset only when the required sum is equal to its value
  for (let s = 1; s <= sum; s++) {
    dp[s] = num[0] == s ? 1 : 0;
  }

  // process all subsets for all sums
  for (let i = 1; i < num.length; i++) {
    for (let s = sum; s >= 0; s--) {
      if (s >= num[i]) {
        dp[s] += dp[s - num[i]];
      }
    }
  }

  return dp[sum];
}

console.log(`Count of subset sum is: ---> ${countSubsets([1, 1, 2, 3], 4)}`);
// 3
//The given set has '3' subsets whose sum is '4': {1, 1, 2}, {1, 3}, {1, 3}
//Note that we have two similar sets {1, 3}, because we have two '1' in our input.
console.log(`Count of subset sum is: ---> ${countSubsets([1, 2, 7, 1, 5], 9)}`);
//3
//The given set has '3' subsets whose sum is '9': {2, 7}, {1, 7, 1}, {1, 2, 1, 5}
````
## 🌟 Target Sum (hard)
https://leetcode.com/problems/target-sum/
> You are given a set of positive numbers and a target sum ‘S’. Each number should be assigned either a ‘+’ or ‘-’ sign. We need to find the total ways to assign symbols to make the sum of the numbers equal to the target ‘S’.

