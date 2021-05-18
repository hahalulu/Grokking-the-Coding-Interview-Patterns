# In-place Reversal of a LinkedList

In a lot of problems, we are asked to reverse the links between a set of nodes of a <b>LinkedList</b>. Often, the constraint is that we need to do this in-place, i.e., using the existing node objects and without using extra memory.

<b>In-place Reversal of a LinkedList pattern</b> describes an efficient way to solve the above problem.

## Reverse a LinkedList (easy)
https://leetcode.com/problems/reverse-linked-list/
> Given the head of a Singly LinkedList, reverse the LinkedList. Write a function to return the new head of the reversed LinkedList.

To reverse a <b>LinkedList</b>, we need to reverse one node at a time. We will start with a variable `curren`t which will initially point to the head of the LinkedList and a variable `previous` which will point to the previous node that we have processed; initially `previous` will point to `null`.

In a stepwise manner, we will reverse the `current` node by pointing it to the `previous` before moving on to the next node. Also, we will update the `previous` to always point to the previous node that we have processed. 

````
class Node {
  constructor(value, next=null) {
    this.value = value;
    this.next = next
  }
  
  printList() {
    let result = ""
    let temp = this
    while(temp !== null) {
      result += temp.value + " "
      temp = temp.next
    }
    return result
  }
}

function reverse(head) {
  let current = head
  let previous = null
  
  while(current !== null) {
    //temporarily store the next node
    next = current.next
    
    //reverse the current node
    current.next = previous
    
    //before we move to the next node, 
    //point previous to the current node
    previous = current
    
    //move on to the next node
    current = next
  }
  
  return previous
}

head = new Node(2)
head.next = new Node(4);
head.next.next = new Node(6);
head.next.next.next = new Node(8);
head.next.next.next.next = new Node(10);

console.log(`Nodes of original LinkedList are: ${head.printList()}`)
console.log(`Nodes of reversed LinkedList are: ${reverse(head).printList()}`)
````

- The time complexity of our algorithm will be `O(N)` where `‘N’` is the total number of nodes in the LinkedList.
- We only used constant space, therefore, the space complexity of our algorithm is `O(1)`.

## Reverse a Sub-list (medium)
## Reverse every K-element Sub-list (medium)
## 🌟 Reverse alternating K-element Sub-list (medium)
## 🌟 Rotate a LinkedList (medium)
