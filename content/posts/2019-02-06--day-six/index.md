---
title: Day Six - Distribute Candies
category: "Arrays"
cover: day6.png
author: Daniel Jiang
---

Today's question is:

<code>Given an integer array with even length, where different numbers in this array represent different kinds of candies. Each number means one candy of the corresponding kind. You need to distribute these candies equally in number to brother and sister. Return the maximum number of kinds of candies the sister could gain.</code>


with example:

<code>
Input: candies = [1,1,2,2,3,3]<br>
Output: 3<br>
Explanation:<br>
There are three different kinds of candies (1, 2 and 3), and two candies for each kind.
Optimal distribution: The sister has candies [1,2,3] and the brother has candies [1,2,3], too. 
The sister has three different kinds of candies. 
</code>

<br>
<br>

First of all, it should be noted that candies minimum length is 2. Thus, we don't have to worry about a lot of unnecessary cases. Reading the question, we know that the integer array has to be an even length, so we can divide all the candies equally between the sister and brother. We want the maximum number of kinds of candies that the sister is getting. In short, whether it's the sister or brother does not matter, we just want to find the maximum amount of unique candies a person can get.

We see in the example that there are 6 candies in total. That means either sibling can get 3 candies . If there are 3 unique numbers in the array, we can just give them to the sister and leave the rest for the brother. This means that the only thing we actually need to care about is if the number of uniques is lower than half the total amount of candies or not, since it cannot be greater. 

We can do this a couple different ways using hashmaps or sets. 

<br>
<br>

```javascript
var distributeCandies = function(candies) {
    var candiesSet = new Set(candies);
    var candiesSet2 = [...new Set(candies)];
    console.log(candiesSet.length);
};
```

<br>
<br>

Let's talk about the differences between a new set and having the set values be in an array. If we do not surround the new set in square brackets and spread syntax, we have a simple set object. If we want to get the length of candiesSet, we would need to use size. If we want to get the length of candiesSet2, we would use length. If we time 100,000 iterations of both set and array and its size/length function, we see that the set is faster. So let's use that.

Now we have the number of uniques. We're pretty much done now. All we have to do is compare with candies.length / 2 and if the size of the set is smaller then we return that. If it is not smaller than it is the same, so its much easier to just return the min of both.

<br>
<br>

```javascript
var distributeCandies = function(candies) {
    return Math.min(candies.length/2,new Set(candies).size);
};
```

<br>
<br>

This can be simplified to one line. If we wanted to work with hashmaps/dictionaries. We would create a dictionary, where the keys are the different types of candies and the values is the number of occurances. We then add all the candies in. Then we would just iterate and count the number of keys. Here, we can see that the value of the keys are irrelevant so it is just much better to use a set. 

**<em>The Solution</em>**

There is one solution that I am interested in going over, not because it is confusing but because it uses permutations. Basically, I am more interested in the permutation process than the actual problem solving process. This is done in java.
<br>
<br>

```javascript
public class Solution {
    int max_kind = 0;
    public int distributeCandies(int[] nums) {
        permute(nums, 0);
        return max_kind;
    }
    public void permute(int[] nums, int l) {
        if (l == nums.length - 1) {
            HashSet < Integer > set = new HashSet < > ();
            for (int i = 0; i < nums.length / 2; i++) {
                set.add(nums[i]);
            }
            max_kind = Math.max(max_kind, set.size());
        }
        for (int i = l; i < nums.length; i++) {
            swap(nums, i, l);
            permute(nums, l + 1);
            swap(nums, i, l);
        }
    }
    public void swap(int[] nums, int x, int y) {
        int temp = nums[x];
        nums[x] = nums[y];
        nums[y] = temp;
    }
}
```
<br>
<br>

Since we have a sufficient understanding of the problem, let's just quickly go over it. There is a global variable maxkind and two helper functions. The main problem passes in the permute function that takes in an int[] and an int. The permute function checks to see if l is the last element in the array. If it is we add all the elements in the array into a set and change max_kind to be the max of itself and the set size. After that we have a for loop that is for the permutations. The swap helper function is just the traditional way of swapping two elements.

After reading stackoverflow and looking at illustrations of the permutation process, I then went into console and copied the permuation and swap functions over and simplified them. Then I console.log() every variable I could to understand the process. All in all, it's just a lot of recursion and understanding the process is a bit tedious. At the end of the day, I guess you should just keep the recursive permutation in your mind and use it whenever you need to. 

Anyway, I'll probably do more permutations in the future and gain a more solid understanding through applying the theory to questions. 