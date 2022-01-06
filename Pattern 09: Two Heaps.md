# Pattern 9: Two Heaps

In many problems, where we are given a set of elements such that we can divide them into two parts. To solve the problem, we are interested in knowing the smallest element in one part and the biggest element in the other part. This pattern is an efficient approach to solve such problems.

This pattern uses two <b>Heaps</b> to solve these problems; A <b>Min Heap</b> to find the smallest element and a <b>Max Heap</b> to find the biggest element.

## Find the Median of a Number Stream (medium)
https://leetcode.com/problems/find-median-from-data-stream/
> Design a class to calculate the median of a number stream. The class should have the following two methods:
> 1. `insertNum(int num)`: stores the number in the class
> 2. `findMedian()`: returns the median of all numbers inserted in the class
> If the count of numbers inserted in the class is even, the median will be the average of the middle two numbers.

As we know, the median is the middle value in an ordered integer list. So a brute force solution could be to maintain a sorted list of all numbers inserted in the class so that we can efficiently return the median whenever required. Inserting a number in a sorted list will take `O(N)` time if there are `‘N’` numbers in the list. This insertion will be similar to the <b>Insertion sort</b>. Can we do better than this? Can we utilize the fact that we don’t need the fully sorted list - we are only interested in finding the middle element?

Assume ‘x’ is the median of a list. This means that half of the numbers in the list will be smaller than (or equal to) ‘x’ and half will be greater than (or equal to) ‘x’. This leads us to an approach where we can divide the list into two halves: one half to store all the smaller numbers (let’s call it `smallNumList`) and one half to store the larger numbers (let’s call it `largNumList`). The median of all the numbers will either be the largest number in the `smallNumList` or the smallest number in the `largNumList`. If the total number of elements is even, the median will be the average of these two numbers.

The best data structure that comes to mind to find the smallest or largest number among a list of numbers is a <b>Heap</b>. Let’s see how we can use a heap to find a better algorithm.

1. We can store the first half of numbers (i.e., `smallNumList`) in a <b>Max Heap</b>. We should use a Max Heap as we are interested in knowing the largest number in the first half.
2. We can store the second half of numbers (i.e., `largeNumList`) in a <b>Min Heap</b>, as we are interested in knowing the smallest number in the second half.
3. Inserting a number in a heap will take `O(logN)`, which is better than the brute force approach.
4. At any time, the median of the current list of numbers can be calculated from the top element of the two heaps.

Let’s take the Example-1 mentioned above to go through each step of our algorithm:
1. `insertNum(3)`: We can insert a number in the <b>Max Heap</b> (i.e. first half) if the number is smaller than the top (largest) number of the heap. After every insertion, we will balance the number of elements in both heaps, so that they have an equal number of elements. If the count of numbers is odd, let’s decide to have more numbers in max-heap than the Min Heap.
2. `insertNum(1)`: As ‘1’ is smaller than ‘3’, let’s insert it into the <b>Max Heap</b>.

Now, we have two elements in the <b>Max Heap</b> and no elements in <b>Min Heap</b>. Let’s take the largest element from the Max Heap and insert it into the <b>Min Heap</b>, to balance the number of elements in both heaps.

3. `findMedian()`: As we have an even number of elements, the median will be the average of the top element of both the heaps ➡️ `(1+3)/2 = 2.0(1+3)/2=2.0`
4. `insertNum(5)`: As ‘5’ is greater than the top element of the <b>Max Heap</b>, we can insert it into the <b>Min Heap</b>. After the insertion, the total count of elements will be odd. As we had decided to have more numbers in the <b>Max Heap</b> than the <b>Min Heap</b>, we can take the top (smallest) number from the <b>Min Heap</b> and insert it into the <b>Max Heap</b>.
5. `findMedian()`: Since we have an odd number of elements, the median will be the top element of <b>Max Heap</b> ➡️ `3`. An odd number of elements also means that the <b>Max Heap</b> will have one extra element than the <b>Min Heap</b>.
6. `insertNum(4)`: Insert ‘4’ into <b>Min Heap</b>.
7. `findMedian()`: As we have an even number of elements, the median will be the average of the top element of both the heaps ➡️ `(3+4)/2 = 3.5(3+4)/2=3.5`

### JavaScript Custom Heap Class
````
/** 
 *  custom Heap class
 */

class Heap {
	constructor(comparator) {
		this.size = 0;
		this.values = [];
		this.comparator = comparator || Heap.minComparator;
	}

	add(val) {
		this.values.push(val);
		this.size ++;
		this.bubbleUp();
	}

	peek() {
		return this.values[0] || null;
	}

	poll() {
		const max = this.values[0];
		const end = this.values.pop();
		this.size --;
		if (this.values.length) {
			this.values[0] = end;
			this.bubbleDown();
		}
		return max;
	}

	bubbleUp() {
		let index = this.values.length - 1;
		let parent = Math.floor((index - 1) / 2);

		while (this.comparator(this.values[index], this.values[parent]) < 0) {
			[this.values[parent], this.values[index]] = [this.values[index], this.values[parent]];
			index = parent;
			parent = Math.floor((index - 1) / 2);
		}
	}

	bubbleDown() {
		let index = 0, length = this.values.length;

		while (true) {
			let left = null,
				right = null,
				swap = null,
				leftIndex = index * 2 + 1,
				rightIndex = index * 2 + 2;

			if (leftIndex < length) {
				left = this.values[leftIndex];
				if (this.comparator(left, this.values[index]) < 0) swap = leftIndex;
			}

			if (rightIndex < length) {
				right = this.values[rightIndex];
				if ((swap !== null && this.comparator(right, left) < 0) || (swap === null && this.comparator(right, this.values[index]))) {
					swap = rightIndex;
				}
			}
			if (swap === null) break;

			[this.values[index], this.values[swap]] = [this.values[swap], this.values[index]];
			index = swap;
		}
	}
}

/** 
 *  Min Comparator
 */
Heap.minComparator = (a, b) => { return a - b; }

/** 
 *  Max Comparator
 */
Heap.maxComparator = (a, b) => { return b - a; }
````
### Solution 
````
class MedianOfAStream {
  constructor(){
     this.maxHeap = new Heap(Heap.maxComparator);
     this.minHeap = new Heap(Heap.minComparator);
  }
  
  insertNum(num) {
   if(this.maxHeap.size === 0 || this.maxHeap.peek() >= num){
     this.maxHeap.add(num)
   } else {
     this.minHeap.add(num)
   }
    
    //either both the heaps will have = numnber of elements
    //or maxHeap will have one or more elements than minHeap
    if(this.maxHeap.size > this.minHeap.size + 1){
      this.minHeap.add(this.maxHeap.poll())
    } else if(this.maxHeap.size < this.minHeap.size) {
      this.maxHeap.add(this.minHeap.poll())
    }
  }

  findMedian() {
    if(this.maxHeap.size > this.minHeap.size){
      //because maxHeap will have one more element 
      //than the minHeap
      return this.maxHeap.peek()
    } else if(this.maxHeap.size < this.minHeap.size){
      return this.minHeap.peek()
    } else {
      //we have an even number of elements
      //so take the average of the middle two elements
      return (this.maxHeap.peek() + this.minHeap.peek()) / 2.0
    } 
  }
};


const medianOfAStream = new MedianOfAStream()
medianOfAStream.insertNum(3)
medianOfAStream.insertNum(1)
console.log(`The median is: ${medianOfAStream.findMedian()}`)//2
medianOfAStream.insertNum(5)
console.log(`The median is: ${medianOfAStream.findMedian()}`)//3
medianOfAStream.insertNum(4)
console.log(`The median is: ${medianOfAStream.findMedian()}`)//3.5
````
- The time complexity of the `insertNum()` will be `O(logN)` due to the insertion in the heap. The time complexity of the `findMedian()` will be `O(1)` as we can find the median from the top elements of the heaps.
- The space complexity will be `O(N)` because, as at any time, we will be storing all the numbers.

## Sliding Window Median (hard)
https://leetcode.com/problems/sliding-window-median/

> Given an array of numbers and a number ‘k’, find the median of all the ‘k’ sized sub-arrays (or windows) of the array.

### Example 1:
#### Input: 
`nums=[1, 2, -1, 3, 5], k = 2`
#### Output: 
`[1.5, 0.5, 1.0, 4.0]`
#### Explanation: 
Lets consider all windows of size ‘2’:
<pre>
[<b>1, 2</b>, -1, 3, 5] -> median is 1.5
[1, <b>2, -1</b>, 3, 5] -> median is 0.5
[1, 2, <b>-1, 3</b>, 5] -> median is 1.0
[1, 2, -1, <b>3, 5</b>] -> median is 4.0
</pre>
### Example 2:
#### Input: `nums=[1, 2, -1, 3, 5], k = 3`
#### Output: `[1.0, 2.0, 3.0]`
#### Explanation: 
Lets consider all windows of size ‘3’:
<pre>
[<b>1, 2, -1</b>, 3, 5] -> median is 1.0
[1, <b>2, -1, 3</b>, 5] -> median is 2.0
[1, 2, <b>-1, 3, 5</b>] -> median is 3.0
</pre>

This problem follows the <b>Two Heaps</b> pattern and share similarities with <b>Find the Median of a Number Stream</b>. We can follow a similar approach of maintaining a <b>max-heap</b> and a <b>min-heap</b> for the list of numbers to find their median.

The only difference is that we need to keep track of a sliding window of ‘k’ numbers. This means, in each iteration, when we insert a new number in the heaps, we need to remove one number from the heaps which is going out of the sliding window. After the removal, we need to rebalance the heaps in the same way that we did while inserting.
### JavaScript Custom Heap Class
````
/** 
 *  custom Heap class
 */

class Heap {
	constructor(comparator) {
		this.size = 0;
		this.values = [];
		this.comparator = comparator || Heap.minComparator;
	}

	add(val) {
		this.values.push(val);
		this.size ++;
		this.bubbleUp();
	}

	peek() {
		return this.values[0] || null;
	}

	poll() {
		const max = this.values[0];
		const end = this.values.pop();
		this.size --;
		if (this.values.length) {
			this.values[0] = end;
			this.bubbleDown();
		}
		return max;
	}

	bubbleUp() {
		let index = this.values.length - 1;
		let parent = Math.floor((index - 1) / 2);

		while (this.comparator(this.values[index], this.values[parent]) < 0) {
			[this.values[parent], this.values[index]] = [this.values[index], this.values[parent]];
			index = parent;
			parent = Math.floor((index - 1) / 2);
		}
	}

	bubbleDown() {
		let index = 0, length = this.values.length;

		while (true) {
			let left = null,
				right = null,
				swap = null,
				leftIndex = index * 2 + 1,
				rightIndex = index * 2 + 2;

			if (leftIndex < length) {
				left = this.values[leftIndex];
				if (this.comparator(left, this.values[index]) < 0) swap = leftIndex;
			}

			if (rightIndex < length) {
				right = this.values[rightIndex];
				if ((swap !== null && this.comparator(right, left) < 0) || (swap === null && this.comparator(right, this.values[index]))) {
					swap = rightIndex;
				}
			}
			if (swap === null) break;

			[this.values[index], this.values[swap]] = [this.values[swap], this.values[index]];
			index = swap;
		}
	}
  // balance() {
  //   const diff = this.maxHeap.size() - this.minHeap.size()
  //   if (diff > 0) this.minHeap.add(this.maxHeap.values.pop());
  //   else if (diff < -1) this.maxHeap.add(this.minHeap.values.pop());
  // }
}

/** 
 *  Min Comparator
 */
Heap.minComparator = (a, b) => { return a - b; }

/** 
 *  Max Comparator
 */
Heap.maxComparator = (a, b) => { return b - a; }
````
### Solution 😕(review)
````

class SlidingWindowMedian {
  constructor(){
    this.maxHeap = new Heap(Heap.maxComparator)
    this.minHeap = new Heap(Heap.minComparator)
  }
  
    balance() {
    // either both the heaps will have equal number of elements or max-heap will have
    // one more element than the min-heap
    if (this.maxHeap.size > this.minHeap.size + 1) {
      this.minHeap.add(this.maxHeap.values.pop());
    } else if (this.maxHeap.size < this.minHeap.size) {
      this.maxHeap.add(this.minHeap.values.pop());
    }
  }

  findSlidingWindowMedian(nums, k) {
    const result = new Array(nums.length - k + 1).fill(0.0);
    console.log(result)
    
    let n = nums.length
    console.log(n)
    
    for(let i = 0; i < n; i++){
      console.log(nums[i])
      if(this.maxHeap.size === 0 || nums[i] <= this.maxHeap.peek()){
        this.maxHeap.add(nums[i])
        console.log(this.maxHeap.peek())
      } else {
        this.minHeap.add(nums[i])
        console.log(this.minHeap.peek())
      }
      
      this.balance()
      
      if(i - k + 1 >= 0){
         //if we have at least k elements in the sliding window
        //then add the median to the result array
        if(this.maxHeap.size() === this.minHeap.size()){
          //we have an enven number of elements
          //take the average of middle two elements
          result[i - k + 1] = (this.maxHeap.peek() + this.min.Heap.peek())/2
        } else {
          //because maxHeap will have one more element than the minHeap
          result[i - k + 1] = this.maxHeap.peek()
        }
        
        //remove the element going out of the sliding window
        const elementToBeRemoved = nums[i - k + 1]
        if(elementToBeRemoved <= this.maxHeap.peek()){
          //delete from heap
          this.maxHeap.remove(elementToBeRemoved)
        } else {
          //delete from heap
          this.minHeap.remove(elementToBeRemoved)
        }
        this.balance()
     }
    }
   return result
  }
};



const slidingWindowMedian = new SlidingWindowMedian()
let result = slidingWindowMedian.findSlidingWindowMedian(
  [1, 2, -1, 3, 5], 2)
console.log(`Sliding window medians are: ${result}`)//[1.5, 0.5, 1.0, 4.0]

slidingWindowMedian = new SlidingWindowMedian()
result = slidingWindowMedian.findSlidingWindowMedian(
  [1, 2, -1, 3, 5], 3)
console.log(`Sliding window medians are: ${result}`)//[1.0, 2.0, 3.0]
````

- The time complexity of our algorithm is `O(N*K)`where `N` is the total number of elements in the input array and `K` is the size of the sliding window. This is due to the fact that we are going through all the `N` numbers and, while doing so, we are doing two things:
	1. Inserting/removing numbers from heaps of size `K`. This will take `O(logK)`.
	2. Removing the element going out of the sliding window. This will take `O(K)` as we will be searching this element in an array of size `K` (i.e., a heap).
- Ignoring the space needed for the output array, the space complexity will be `O(K)` because, at any time, we will be storing all the numbers within the sliding window.

## Maximize Capital (hard)
https://leetcode.com/problems/ipo/

## 🌟 Next Interval (hard)
https://leetcode.com/problems/find-right-interval/
