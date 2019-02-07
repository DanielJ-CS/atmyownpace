---
title: Day Seven - Maximum Depth of a Binary Tree
category: "BST"
cover: day7.png
author: Daniel Jiang
---

Today's question is our second binary search tree question:

<code>Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.</code>

Our example is:

<code> Given binary tree: [3,9,20,null,null,15,7]
<br> Return 3 </code>

Another binary question! But this time we are returning a number instead of a whole tree. When we return a whole tree, it is in an array, but now we want to work in integers.

<br>
<br>

```javascript
var maxDepth = function(root) {
    if (root == null) {
        return 0;
    } 
};
```

<br>
<br>

From last time, we created a template that focused on the general outline of creating a BST algorithm. The first thing we should think about is the base case. This is when root is null and the entire tree is null. Then we would return a depth of 0, as the tree does not exist. Now let's consider the other times when root is null. When we procede with our recursion and we go to the left and right subtree, the last case is a null. This is because there is no node there and we want to return a value. Since we are going down the tree but the next node doesn't exist, we still return 0 because for the algorithm to return 1, there must be a node there to increase the depth. 

So once the algorithm starts and we know that the tree itself is not null, then this base case checks when we have reached the end of the tree. 

Now, unlike the previous question, we do not actually have to check for any edge cases like boundaries. We can simply continue to the right and left subtree and if there is anything there we want to increment the depth.

<br>
<br>

``` javascript
var maxDepth = function(root) {
    if (root == null) {
        return 0;
    }
    
    root.left = maxDepth(root.left) + 1;
    root.right = maxDepth(root.right) + 1;
};
```

<br>
<br>

What we are doing now is recursively going through the tree and going to all the left and right subtrees of every node. Consider the tree : [3]. We have root.left and root.right both be null. This means when we go through the recursion, we simply return 0 and root.right and root.left are both 1. 

Now to figure out what to return after the left and right statements, we consider a tree [3,2,null], where there is only a left subtree. At first, we have root=3, then we call the recursion on 2. Then, since we just did the case where 2 is root and we have two nulls under it, we know that its root.left and root.right are both 1. The question now is: What do we return to the first case where root = 3. What should our root.left be?

Let's look at the root.right case. We can immediately see that its 1. Well, since in the case for root = 2, root.left = 1 and root.right = 1, we always just return 1, right? But what if the root on the left is longer, just like the case in root=3. Then we just return the larger one between root.left and root.right. 
<br>
<br>

```javascript
var maxDepth = function(root) {
    if (root == null) {
        return 0;
    }
    
    root.left = maxDepth(root.left) + 1;
    root.right = maxDepth(root.right) + 1;
    
    return Math.max(root.left,root.right);
};
```
<br>
<br>

Then in our very basic example above where the tree is [3,2,null]. We return 1 from the case where root=2. Then for root=3, we have root.left = 2 and root.right = 1. Then we return 2 as the depth. 

So all in all, this question was okay. I have done it before so I already knew the direction to go for this. The same cannot be said for the harder BST questions, so I am trying to pick apart every single recursion and see why everything happens. When I reach a question with BST that I am not familiar with, I hope to apply the same pattern and recursion to it that I have used on more simple questions like these. 

**<em>What did we learn from this?</em>**

Well, we generalized a pattern to use when dealing with searching through a binary tree and returning an integer. The first part is our base case. We need to know what happens when there is an empty tree. We also need to know what happens when we hit nothing because we are already at the bottom of a subtree. In the first case, we can simply give the person a final answer. In the second case, we need to return a value to the previous recursion. 

Now for the case where we are returning a value. We have a root.left and a root.right which goes one node deeper into the tree and does something with the return value. Consider the case where both root.left and root.right finally have a return value. This is the deepest part of the recursion that includes root.left and root.right as the case where root == null doesn't get this far. 

The last case we consider is when we have both root.left and root.right, what do we want to pass up to the case above the current case. In this question, we are looking for the largest depth, so root.left having a larger value than root.right would simply mean that the subtree on the left is deeper. How much deeper? root.left - root.right deeper. So we return root.left because it is the deeper one and neglect root.right since it is now irrelevant. 

The algorithm just does its natural recursion and we arrive at the correct answer.