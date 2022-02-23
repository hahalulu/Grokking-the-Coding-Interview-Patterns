# Pattern 13: Top 'K' Elements

Any problem that asks us to find the <b>top/smallest/frequent K</b> elements among a given set falls under this pattern.

The best data structure that comes to mind to keep track of <b>K</b> elements is Heap. This pattern will make use of the <b>Heap</b> to solve multiple problems dealing with <b>K</b> elements at a time from a set of given elements.

## Top 'K' Numbers (easy)
https://leetcode.com/problems/kth-largest-element-in-an-array/

> Given an unsorted array of numbers, find the ‘K’ largest numbers in it.

Note: For a detailed discussion about different approaches to solve this problem, take a look at Kth Smallest Number.

A brute force solution could be to sort the array and return the largest K numbers. The time complexity of such an algorithm will be O(N*logN)O(N∗logN) as we need to use a sorting algorithm like Timsort if we use Java’s Collection.sort(). Can we do better than that?

The best data structure that comes to mind to keep track of top ‘K’ elements is Heap. Let’s see if we can use a heap to find a better algorithm.

If we iterate through the array one element at a time and keep ‘K’ largest numbers in a heap such that each time we find a larger number than the smallest number in the heap, we do two things:

Take out the smallest number from the heap, and
Insert the larger number into the heap.
This will ensure that we always have ‘K’ largest numbers in the heap. The most efficient way to repeatedly find the smallest number among a set of numbers will be to use a min-heap. As we know, we can find the smallest number in a min-heap in constant time O(1)O(1), since the smallest number is always at the root of the heap. Extracting the smallest number from a min-heap will take O(logN)O(logN) (if the heap has ‘N’ elements) as the heap needs to readjust after the removal of an element.

Let’s take Example-1 to go through each step of our algorithm:

Given array: [3, 1, 5, 12, 2, 11], and K=3

First, let’s insert ‘K’ elements in the min-heap.
After the insertion, the heap will have three numbers [3, 1, 5] with ‘1’ being the root as it is the smallest element.
We’ll iterate through the remaining numbers and perform the above-mentioned two steps if we find a number larger than the root of the heap.
The 4th number is ‘12’ which is larger than the root (which is ‘1’), so let’s take out ‘1’ and insert ‘12’. Now the heap will have [3, 5, 12] with ‘3’ being the root as it is the smallest element.
The 5th number is ‘2’ which is not bigger than the root of the heap (‘3’), so we can skip this as we already have top three numbers in the heap.
The last number is ‘11’ which is bigger than the root (which is ‘3’), so let’s take out ‘3’ and insert ‘11’. Finally, the heap has the largest three numbers: [5, 12, 11]
As discussed above, it will take us O(logK)O(logK) to extract the minimum number from the min-heap. So the overall time complexity of our algorithm will be O(K*logK+(N-K)*logK)O(K∗logK+(N−K)∗logK) since, first, we insert ‘K’ numbers in the heap and then iterate through the remaining numbers and at every step, in the worst case, we need to extract the minimum number and insert a new number in the heap. This algorithm is better than O(N*logN)O(N∗logN).
## 
## 
## 
## 
## 
## 
## 
## 
## 
## 
## 🌟
## 🌟
## 🌟

