---
title: Day Four - Trimming a Binary Search Tree
category: "BST"
cover: day4.png
author: Daniel Jiang
---

Today's question is:

<code>Given a binary search tree and the lowest and highest boundaries as L and R, trim the tree so that all its elements lies in [L, R] (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.</code>

<br />
<br />

Let's talk about binary search trees a bit before we get into the question. The basic search algorithm looks some like:
<br />
<br />

```javascript
function search(root, key) {
//Base Case:
//We check if the root is null or if the key is our current root
    if (root == null || root.key == key) {
        return root;
    }
    //Then we check the left and right sides of the current root
    if (root.key > key) {
        return search(root.left,key);
    }
    else {
        return search(root.right,key);    
    }
}
```
<br />
<br />

So, this is the basic binary tree seach that a lot of the more advanced algorithms are built upon. To represent a bst as an array we simply go from left to right from the root down. Imagine we had a bst with the values [4,2,5,1,3,null,7]. Let's apply the basic search formula to get a good sense of the recursion. We start with 4 as the root and let's say 3 as the key. 3 is less than 4 so we pass in search(root.left, 3). Now we compare again where root.left is equal to 2. Now our root is smaller than our key. We pass in search(root.right,3) and see that they are equal. Once we get into some harder bst questions, the recursions will also get more confusing so it is nice to have a strong foundation in how bst works. 

Going back to the question, we can apply a similar thought process. The function takes in a root, a left, and a right key. We want to trim out the numbers that are less than L and more than R. 

```javascript
var trimBST = function(root, L, R) {
    if (root == null) {
        return root;
    }
};
```
<br />
<br />

This is the case where either the whole tree is null or the recursion hits a null in the array. If there is a null in the array, it would mean that there is no tree going down. In both those cases, we should just return back null because what we ultimately want to do is to trim the list not add new values. 
<br />
<br />

```javascript
var trimBST = function(root, L, R) {
    if (root == null) {
        return root;
    }

    if (root.val < L) {
        return trimBST(root.right,L,R);
    }
    if (root.val > R ) {
        return trimBST(root.left,L,R);
    }
};
```
<br />
<br />

Let's look at the edge cases. When the current root value is less than L, this would mean that all nodes going down the left subtree is also less than L. We can effectively trim all those values out. Instead, we will look at the the nodes going down the right subtree. We also do the same with the values greater than R. 
<br />
<br />

```javascript
var trimBST = function(root, L, R) {
    if (root == null) {
        return root;
    }

    if (root.val < L) {
        return trimBST(root.right,L,R);
    }
    if (root.val > R ) {
        return trimBST(root.left,L,R);
    }

    root.left = trimBST(root.left,L,R);
    root.right = trimBST(root.right,L,R);

    return root;
};
```

<br />
<br />

Now we need to consider the scenario where the values are inside the boundries [L,R]. In this case, we can just leave the values there as they are in their correct place. So, what we want is to go through the rest of the tree. 

For example, let a tree be [5,3,7,1,4,6,8] and an L = 1 and R = 8. This case does not have anything to trim. We start at the root 5, and then we set the root.left = trimBST(3,L,R), root.right = trimBST(7,L,R), and then we return 5. We return 5 at the end of all the recursive functions going down all the left and right subtrees. root.left would return 3 and would have a trimBST(1,L,R) and trimBST(4,L,R) for root.left and root.right. Going down the left subtree, we would then have trimBST(null,L,R) for the right and left keys. This is where the tree ends and we would return null for both subtrees. Since none of the values are outside the boundary, we are just returning the original value for each node. 

Let's create a template for BST functions that have multiple specific edge cases then one for the rest:
<br />
<br />

```javascript
function search(root, x, y) {
//Base Case:
//We check if the root is null
    if (root == null) {
        return root;
    }
    //Then we check the edge cases
    if (something with x as an edge case) {
        return //do something recursively that replaces the current value
    }
    if (something with y as an edge case) {
        return //do something recursively that replaces the current value
    }

    //Consider the case where the current root falls within the rules.
    //We then want to pass the value back in as the original value
    //and check the left and right nodes.

    root.left = //recursion for the left value
    root.right = //recursion for the right value

    return root; // Returns the original value

}
```
<br />
<br />

Side note is that the searching in BST takes O(log n) time and so should this algorithm.

There is pretty much one way to do this question so we are skipping the solution. Anyway, this is the intro question to BST with more to come.