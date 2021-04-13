# Grokking-the-Coding-Interview-Patterns

## Pattern 1 : Sliding Window

## Pattern 2: Two Pointer

## Pattern: Tree Breadth First Search

### Binary Tree Level Order Traversal (easy) 😕
````
class Deque {
    constructor() {
        this.front = this.back = undefined;
    }
    addFront(value) {
        if (!this.front) this.front = this.back = { value };
        else this.front = this.front.next = { value, prev: this.front };
    }
    removeFront() {
        let value = this.peekFront();
        if (this.front === this.back) this.front = this.back = undefined;
        else (this.front = this.front.prev).next = undefined;
        return value;
    }
    peekFront() { 
        return this.front && this.front.value;
    }
    addBack(value) {
        if (!this.front) this.front = this.back = { value };
        else this.back = this.back.prev = { value, next: this.back };
    }
    removeBack() {
        let value = this.peekBack();
        if (this.front === this.back) this.front = this.back = undefined;
        else (this.back = this.back.next).back = undefined;
        return value;
    }
    peekBack() { 
        return this.back && this.back.value;
    }
}

class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null; 
  }
};


function traverse (root) {
  result = [];
  if(root === null ) {
    return result
  }
  
  const queue = new Deque()
  //Start by pushing the root node to the queue.
  queue.addFront(root)
  //Keep iterating until the queue is empty.
  let currentLevel = []
  while (queue.length > 0) {
    const levelSize = queue.length
    //In each iteration, first count the elements in the queue (let’s call it levelSize). We will have these many nodes in the current level.
     
    for(i = 0; i < levelSize; i++) {
      TreeNode = queue.removeFront()
      //add the node to the current level
      currentLevel.push(TreeNode.val)
      //insert the children of current node in the queue
      if(TreeNode.left !== null) {
        queue.addBack(TreeNode.left)
      }
    }
    if(TreeNode.right !== null) {
      queue.addBack(TreeNode.right)
    }
  }
  
  result.push(currentLevel)
  
  //Next, remove levelSize nodes from the queue and push their value in an array to represent the current level.
  //After removing each node from the queue, insert both of its children into the queue.
  //If the queue is not empty, repeat from step 3 for the next level.
  return result;
};



var root = new TreeNode(12);
root.left = new TreeNode(7);
root.right = new TreeNode(1);
root.left.left = new TreeNode(9);
root.right.left = new TreeNode(10);
root.right.right = new TreeNode(5);
console.log(`Level order traversal: ${traverse(root)}`);
````

- The time complexity of the above algorithm is `O(N)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` as we need to return a list containing the level order traversal. We will also need `O(N)` space for the queue. Since we can have a maximum of `N/2` nodes at any level (this could happen only at the lowest level), therefore we will need `O(N)` space to store them in the queue.

#### Easier to understand solution 
````
function TreeNode(val, left, right) {
  this.val = (val===undefined ? 0 : val)
  this.left = (left===undefined ? null : left)
  this.right = (right===undefined ? null : right)
 }
 
 const levelOrder = function  (root) {
  //If root is null return an empty array
  if(!root) return []
  
  const queue = [root] //initialize the queue with root
  const levels = [] //declare output array
  
  while(queue.length !== 0) {
    const queueLength = queue.length//get the length prior to deque
    const currLevel = []//declare this level
    //loop through to exahuast all options and only to include nodes at currLevel
    for(let i = 0; i < queueLength; i++) {
      //get next node
      const current = queue.shift()
      if(current.left) {
        queue.push(current.left)
      }
      if(current.right) {
        queue.push(current.right)
      }
      //after we add left and right for current, we add to currLevel
      currLevel.push(current.val)
    }
    //Level has been finished. Push into output array
    levels.push(currLevel) 
  }
  return levels
};



levelOrder([3,9,20,null,null,15,7])//[[3],[9,20],[15,7]]
levelOrder([1])//[[1]]
levelOrder([])//[]
````

### Reverse Level Order Traversal (easy)
````
function TreeNode(val, left, right) {
  this.val = (val===undefined ? 0 : val)
  this.left = (left===undefined ? null : left)
  this.right = (right===undefined ? null : right)
 }
 
 const traverse = function(root) {
  if(!root) return []
   const queue = [root]
   
   const levels = []
   
   while(queue.length !== 0) {
     const queueLength = queue.length
     const currLevel = []
     for (let i = 0; i < queueLength; i++) {
       const current = queue.push()
       if(current.left) {
         queue.push(current.left)
       }
       if(current.right) {
         queue.push(current.right)
       }
       currLevel.push(current.val)
     }
     levels.unshift(currLevel)
   }
   
   return levels
}
 
 traverse([[12], [7,1], [9, 10, null, 5]])
````
### Zigzag Traversal (medium) 🌴
````
function TreeNode(val, left, right) {
  this.val = (val === undefined ? 0 : val)
  this.left = (left === undefined ? null : left)
  this.right = (right === undefined ? nill : right)
}

function zigzagLevelOrder(root) {
  //if root is null return an empty array
  if(!root) return []
  
  //initialize the queue with root
  const queue = [root]
  //declare the output array
  const levels = []
  let leftToRight = true
  
  while(queue.length !== 0) {
    //get the length prior to deque?
    const queueLength = queue.length
    //declare the current level
    const currentLevel = []
    
    //loop through to exhaust all aoption and only to include nodes at current Level
    for (let i = 0; i < queueLength; i++) {
      //get the next node
      const currentNode = queue.shift()
      
      //add the node to the current level based on the traverse direction
      if(leftToRight) {
        currentLevel.push(currentNode.val)
      } else {
        currentLevel.unshift(currentNode.val)
      }
      
      //insert the children of current node in the queue
      if(currentNode.left !== null) {
        queue.push(currentNode.left) 
      }
      if(currentNode.right !== null) {
        queue.push(currentNode.right)
      }
    }
    //Level has been finished. push to the out put arra
    levels.push(currentLevel)
    
    //reverse the traversal direction 
    leftToRight = !leftToRight
  }
  return levels
}

zigzagLevelOrder([1, 2, 3, 4, 5, 6, 7])
zigzagLevelOrder([3,9,20,null,null,15,7])
zigzagLevelOrder([1])
zigzagLevelOrder([])
````
- The time complexity of the above algorithm is `O(N)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` as we need to return a list containing the level order traversal. We will also need `O(N)` space for the queue. Since we can have a maximum of `N/2` nodes at any level (this could happen only at the lowest level), therefore we will need `O(N)` space to store them in the queue.

# Depth First Search (DFS)

This pattern is based on the Depth First Search (DFS) technique to traverse a tree.

We will be using recursion (or we can also use a stack for the iterative approach) to keep track of all the previous (parent) nodes while traversing. This also means that the space complexity of the algorithm will be `O(H)`, where `‘H’` is the maximum height of the tree.
