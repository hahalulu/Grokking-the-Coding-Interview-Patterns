# Pattern 8: Tree Depth First Search (DFS)

This pattern is based on the Depth First Search (DFS) technique to traverse a tree.

We will be using recursion (or we can also use a stack for the iterative approach) to keep track of all the previous (parent) nodes while traversing. This also means that the space complexity of the algorithm will be `O(H)`, where `‘H’` is the maximum height of the tree.

## Binary Tree Path Sum (easy)

````
class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null
  }
}

const has_path = function(root, sum) {
  if(root == null) return false
  //start DFS with the root of the tree
  //if the current node is not a leaf node
    //1. Subtract the value of the current node from the given number to get a new sum => S = S - node.value
  //if the current node is a leaf node and it's value 
  //is equal to the sum, we've found a path
  if(root.value === sum && root.left === null && root.right == null) return true
    //2. Make two recursive calls for both the children of the current node with the new number calculated in the previous step.
    //recursively call to travese the left and right sub-tree
  //return true if any of the two recursive calls return true
  return has_path(root.left, sum - root.value) || has_path(root.right, sum - root.value)
  //At every step, see if the current node being visited is a leaf node and if its value is equal to the given number ‘S’. If both these conditions are true, we have found the required root-to-leaf path, therefore return true.
  //If the current node is a leaf but its value is not equal to the given number ‘S’, return false.
};

var root = new TreeNode(12)
root.left = new TreeNode(7)
root.right = new TreeNode(1)
root.left.left = new TreeNode(9)
root.right.left = new TreeNode(10)
root.right.right = new TreeNode(5)
console.log(`Tree has path: ${has_path(root, 23)}`)
console.log(`Tree has path: ${has_path(root, 16)}`)
````
- The time complexity of the above algorithm is `O(N)`, where `‘N’` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the recursion stack. The worst case will happen when the given tree is a linked list (i.e., every node has only one child).

## All Paths for a Sum (medium)

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


function find_paths(root, sum) {
  //Every time we find a root-to-leaf path, we will store it in a list.
  allPaths = [];
//We will traverse all paths and will not stop processing after finding the first path.
 findPathsRecursive(root, sum, new Deque(), allPaths)
  return allPaths;
};

function findPathsRecursive(currentNode, sum, currentPath, allPaths){
  if(currentNode === null) return
  
  //add the current node to the path
  currentPath.push(currentNode.value)
  
  //if the current node is a leaf and it's value is equal to sum, save the current path
  if(currentNode.val === sum && currentNode.left === null & currentNode.right === null) {
    allPaths.push(currentPath.toArray())
  } else {
    //traverse the left sub-tree
    findPathsRecursive(currentNode.left, sum - currentNode.value, currentPath, allPaths)
    //traverse the right sub-tree
    findPathsRecursive(currentNode.right, sum - currentNode.value, currentPath, allPaths)
  }
  //remove the current node from the path to backtrack,
  //we need to remove the current node while we are going up the recursive call stack
  currentPath.pop()
}



var root = new TreeNode(12)
root.left = new TreeNode(7)
root.right = new TreeNode(1)
root.left.left = new TreeNode(4)
root.right.left = new TreeNode(10)
root.right.right = new TreeNode(5)
let sum = 23,
  result = find_paths(root, sum);

process.stdout.write(`Tree paths with sum ${sum}: `);
for (i = 0; i < result.length; i++) {
  process.stdout.write(`[${result[i]}] `);
}
````
- The time complexity of the above algorithm is `O(N^2)`, where `‘N’` is the total number of nodes in the tree. This is due to the fact that we traverse each node once (which will take `O(N)`), and for every leaf node, we might have to store its path (by making a copy of the current path) which will take `O(N)`.
  - We can calculate a tighter time complexity of `O(NlogN)` from the space complexity discussion below.
- If we ignore the space required for the `allPaths` list, the space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the recursion stack. The worst-case will happen when the given tree is a linked list (i.e., every node has only one child).

## Sum of Path Numbers (medium)
## Path With Given Sequence (medium)
## Count Paths for a Sum (medium)
## 🌟 Tree Diameter (medium)
## 🌟 Path with Maximum Sum (hard)
