## Pattern 3: Fast & Slow pointers

The <b>Fast & Slow</b> pointer approach, also known as the <b>Hare & Tortoise algorithm</b>, is a pointer algorithm that uses two pointers which move through the array (or sequence/LinkedList) at different speeds. This approach is quite useful when dealing with cyclic LinkedLists or arrays.

By moving at different speeds (say, in a cyclic LinkedList), the algorithm proves that the two pointers are bound to meet. The fast pointer should catch the slow pointer once both the pointers are in a cyclic loop.

One of the famous problems solved using this technique was <b>Finding a cycle in a LinkedList</b>. Let’s jump onto this problem to understand the <b>Fast & Slow</b> pattern.

## LinkedList Cycle (easy)
https://leetcode.com/problems/linked-list-cycle/

````
class Node {
  constructor(value, next = null) {
    this.value = value;
    this.next = next
  }
}

function hasCycle(head) {
  let slow = head
  let fast = head
  while(fast !== null && fast.next !== null) {
    fast = fast.next.next;
    slow = slow.next
    
    if(slow === fast) {
      //found the cycle
      return true
    }
  }
  return false
}

head = new Node(1)
head.next = new Node(2)
head.next.next = new Node(3)
head.next.next.next = new Node(4)
head.next.next.next.next = new Node(5)
head.next.next.next.next.next = new Node(6)
console.log(`LinkedList has cycle: ${hasCycle(head)}`)

head.next.next.next.next.next.next = head.next.next
console.log(`LinkedList has cycle: ${hasCycle(head)}`)

head.next.next.next.next.next.next = head.next.next.next
console.log(`LinkedList has cycle: ${hasCycle(head)}`)
````

- Once the slow pointer enters the cycle, the fast pointer will meet the slow pointer in the same loop. Therefore, the time complexity of our algorithm will be `O(N)` where `‘N’` is the total number of nodes in the LinkedList.
- The algorithm runs in constant space `O(1)`.

### Given the head of a LinkedList with a cycle, find the length of the cycle.

Once the fast and slow pointers meet, we can save the slow pointer and iterate the whole cycle with another pointer until we see the slow pointer again to find the length of the cycle.

````
class Node {
  constructor(value, next = null) {
    this.value = value;
    this.next = next
  }
}

function findCycleLength(head) {
  let slow = head
  let fast = head
  
  while(fast !== null && fast.next !== null) {
    fast = fast.next.next;
    slow = slow.next
    
    if(slow === fast) {
      //found the cycle
      return calculateCycleLength(slow)
    }
  }
  return 0
}

function calculateCycleLength(slow) {
  let current = slow
  let cycleLength = 0
  
  while(true) {
    current = current.next
    cycleLength++
    if(current === slow) {
      break
    }
  }
  return cycleLength
}

head = new Node(1)
head.next = new Node(2)
head.next.next = new Node(3)
head.next.next.next = new Node(4)
head.next.next.next.next = new Node(5)
head.next.next.next.next.next = new Node(6)
console.log(`LinkedList has cycle length of: ${findCycleLength(head)}`)

head.next.next.next.next.next.next = head.next.next
console.log(`LinkedList has cycle length of: ${findCycleLength(head)}`)

head.next.next.next.next.next.next = head.next.next.next
console.log(`LinkedList has cycle length of: ${findCycleLength(head)}`)
````

- The above algorithm runs in `O(N)` time complexity and `O(1)` space complexity.
