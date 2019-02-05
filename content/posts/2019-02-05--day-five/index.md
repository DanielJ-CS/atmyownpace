---
title: Day Five - Binary Gap
category: "Binary"
cover: day5.png
author: Daniel Jiang
---

Today's question is our first binary question:

<code>Given a positive integer N, find and return the longest distance between two consecutive 1's in the binary representation of N.

If there aren't two consecutive 1's, return 0. </code>

with an example of:

<code>

Input: 22 <br>
Output: 2 <br>
Explanation:  <br>
22 in binary is 0b10110.
In the binary representation of 22, there are three ones, and two consecutive pairs of 1's. <br>
The first consecutive pair of 1's have distance 2.<br>
The second consecutive pair of 1's have distance 1.<br>
The answer is the largest of these two distances, which is 2.
</code>

The first thing we want to do is to convert the number to its binary form. We can search stackoverflow for a suitable function.

```javascript 
var binaryGap = function(N) {
    var bin = N.toString(2);
    console.log(bin);
    console.log(typeof bin);
};
```

<br>
<br>

I googled how to change an integer to a binary in javascript and found some methods. The method that seemed the easiest to use is the toString(2) function. Some comments about this function is that you cannot use this for negative numbers, but for most binary questions, it should get the job done.

Now, the code above is simplified to see the outputs and the type of the output. The results is that we get the binary form of a number in a string. 
<br>
<br>

```javascript
var binaryGap = function(N) {
    var bin = N.toString(2);
    var count = 0;
    var max = 0;
    
    for (var i = 0; i < bin.length; i++) {
        if (bin[i] == '1') {
            max = Math.max(max, count);
            count = 0;
        } 
        count++;
    }
    return max;
};

```

<br>
<br>

Here, we have a fairly simple solution. Whenever we hit a '1' in the string we want to do something. The first thing is to compare count with max. Our max will be the solution we are returning. If our count is higher than max, we will change max to count.

For order, it would be best to increment count after everything. The first solution that I wrote checked for if the count was greater than max, then if it was, I had to change it. This added problems where if the first element was one and no other one appeared, then the max would still be updated as I had my count increment before the max/count check. To remedy that, I had to create a flag variable. 

Anyway, one thing I found was that using my bin variable to store the binary string actually saved some memory. In the future, I want to improve my understanding of saving space and in-place algorithms.

**<em>The solution</em>**

We will look at the solution which I find the most interesting which is the java one that takes O(log n) time. Okay, I continued to read the solution complexity analysis and it turns out that log N is the number of digits in the binary representation of N. This is good to know, as I thought it would just be O(N).
<br>
<br>

```javascript
class Solution {
    public int binaryGap(int N) {
        int[] A = new int[32];
        int t = 0;
        for (int i = 0; i < 32; ++i)
            if (((N >> i) & 1) != 0)
                A[t++] = i;

        int ans = 0;
        for (int i = 0; i < t - 1; ++i)
            ans = Math.max(ans, A[i+1] - A[i]);
        return ans;
    }
}
```

<br>
<br>

The first one is to take an approach that stores indices. We create a new array A. We create an integer t declared to be 0. Our array A has max length 32. Why 32? I'm not sure but I guess it is because it is sufficiently large. If all 32 indices are filled with 1's, changing back to integer, we would get 4294967295. So if there's a meaning behind 32, I am not getting it. 

The next line is a bit tricky. N >> i means that we are shifting N to the right by i. In short, if our example N is 22. Shifting it to the right when i=0 results in 22, when i=1 results in 11, i=2 results in 5 and so on. You can look at the binary form of the integer and move it i spaces to the right. 

The &1 simply checks if the last digit in the binary form is a 1. If it is, it returns a 1, else 0. And then, the if statement checks if each digit is a 1. If it is, push it into the A array with a value of the index it takes up in the binary form of the integer. 

Then we simply take the distance between the two ones by subtracting their index and return the largest distance. 

So the hardest part of this solution was understanding the syntax for shifting bits over to the right. But once we got over that, it was easier to understand. The second given solution is similar to ours, so I won't go over it.
