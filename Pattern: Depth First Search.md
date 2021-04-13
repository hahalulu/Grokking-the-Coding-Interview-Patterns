# Depth First Search (DFS)

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
