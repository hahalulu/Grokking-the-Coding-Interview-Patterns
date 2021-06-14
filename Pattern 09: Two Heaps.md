# Pattern 9: Two Heaps

In many problems, where we are given a set of elements such that we can divide them into two parts. To solve the problem, we are interested in knowing the smallest element in one part and the biggest element in the other part. This pattern is an efficient approach to solve such problems.

This pattern uses two <b>Heaps</b> to solve these problems; A <b>Min Heap</b> to find the smallest element and a <b>Max Heap</b> to find the biggest element.

## Find the Median of a Number Stream (medium)

Design a class to calculate the median of a number stream. The class should have the following two methods:
1. `insertNum(int num)`: stores the number in the class
2. `findMedian()`: returns the median of all numbers inserted in the class
If the count of numbers inserted in the class is even, the median will be the average of the middle two numbers.

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
