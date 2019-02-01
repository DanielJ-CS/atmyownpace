---
title: Day Three - Island Perimeter
category: "Stacks and Queues"
cover: day3.png
author: Daniel Jiang
---

The question for today is:

<code>

You're now a baseball game point recorder.
Given a list of strings, each string can be one of the 4 following types:<br>

1.)Integer (one round's score): Directly represents the number of points you get in this round.

2.) "+" (one round's score): Represents that the points you get in this round are the sum of the last two valid round's points.

3.) "D" (one round's score): Represents that the points you get in this round are the doubled data of the last valid round's points.

4.) "C" (an operation, which isn't a round's score): Represents the last valid round's points you get were invalid and should be removed.

Each round's operation is permanent and could have an impact on the round before and the round after.
You need to return the sum of the points you could get in all the rounds.
</code>
<br />
<br />
With examples:
<br />
<br />
<code>

Input: ["5","2","C","D","+"]<br />
Output: 30<br />
Explanation: <br />
Round 1: You could get 5 points. The sum is: 5.<br />
Round 2: You could get 2 points. The sum is: 7.<br />
Operation 1: The round 2's data was invalid. The sum is: 5. <br /> 
Round 3: You could get 10 points (the round 2's data has been removed). The sum is: 15.<br />
Round 4: You could get 5 + 10 = 15 points. The sum is: 30.
-----------------------------------------------------------------------------------------
Input: ["5","-2","4","C","D","9","+","+"] <br />
Output: 27<br />
Explanation: <br />
Round 1: You could get 5 points. The sum is: 5. <br />
Round 2: You could get -2 points. The sum is: 3.<br />
Round 3: You could get 4 points. The sum is: 7.<br />
Operation 1: The round 3's data is invalid. The sum is: 3.  <br />
Round 4: You could get -4 points (the round 3's data has been removed). The sum is: -1.<br />
Round 5: You could get 9 points. The sum is: 8.<br />
Round 6: You could get -4 + 9 = 5 points. The sum is 13.<br />
Round 7: You could get 9 + 5 = 14 points. The sum is 27.<br />
</code>

<br />
<br />
First thoughts are to make sense of the question. Basically, there are bunch of rounds and, for each round, you can points. Aside from a numeric point value, you can also get three different signs. So C is the bad one. You remove the points gained from the round before and makes it seem like the round never happened. When you get the good ones, and the previous are C'd, you don't count them. D is 2*round before and + is fibonacci. I am switching to javascript for coding from now on.
<br />
<br />

```javascript
var calPoints = function(ops) {
    var stack = [];
    ops.forEach(op => {
        console.log(op);
    });
};
```
<br />
<br />

So the first thing that I tried was to reduce the whole thing using ops.reduce(). Turned out pretty bad. Not the right thing to do in this instance. Looked around and figured out that you were supposed to use a stack for this. 

<em>How am I supposed to know when to use a stack?</em>

Since I am starting on this series of questions, I might as well find the common occurances of stacks so I have an idea next time. Here is a <a href="https://www.geeksforgeeks.org/stack-set-2-infix-to-postfix/">link </a> to the first of many stack applications that I am going to summarize. <br />

Infix Expressions are expressions in the form of "a" (operation) "b" where there is an operation between the operands. Postfix has the operation taking place after both operands. So looking at this type of question, we can begin to see the similarities with our baseball question. We are a + b and 2*a operation. 

The second application is simply reversing a string. Push all chars into the stack and pop them back out. I guess this is similar to the C key in our question, where you want to pop out the last event.

The third application is implementing two stacks in an array. Basically, you create two stacks but only use one array to support it. You need a functional push1() and push2() which pushes to the said stack and pop1() and pop2(). The algorithm is just to start at the start and ends of the array and work into each other.

The last application is checking for balanced parenthesis. You basically push all starting parenthesis and pop all ending. The ending should match the starting parenthesis that popped. At the end, if there is still something left then it is not balanced.

<em>Back to the main problem</em>

Anyways, so far I created a stack and a forEach loop that goes through all the items in ops. Console.log returns each item's name. For each instance, we want to do something to the stack. The integers can be pushed on the stack, since we will just go through the stack array and add them all up after. For 'C', we can just pop the last event. For 'D', we can just push 2 times the value of the top value. Since javascript doesn't have a peek function for stacks, we just simply use array syntax. For '+', we somehow need to get the top two and add them together.

```javascript
var calPoints = function(ops) {
    var stack = [];
    ops.forEach(op => {
        if( !isNaN(op)) {
            stack.push(parseInt(op));
        } else if (op == "C"){
            stack.pop();
        } else if (op == "D") {
            stack.push(2*stack[stack.length - 1]);
        } else {
            stack.push(stack[stack.length - 1] + stack[stack.length - 2]);
        }
        console.log(stack);
    });
};
```
<br />
<br />

So we basically coded out what we do in each scenario of the forEach loop. This in total takes O(n) time as we go through every element in ops and apply a constant time action for each. We console.log the stack to verify that the numbers in the stack coincide with the example. Now all we have to do is take everything in the stack array and add them up. We could do another forEach loop but reduce is probably what we are looking for.

<br />
<br />

```javascript
var calPoints = function(ops) {
    var stack = [];
    ops.forEach(op => {
        if( !isNaN(op)) {
            stack.push(parseInt(op));
        } else if (op == "C"){
            stack.pop();
        } else if (op == "D") {
            stack.push(2*stack[stack.length - 1]);
        } else {
            stack.push(stack[stack.length - 1] + stack[stack.length - 2]);
        }
    });
    
    return stack.reduce((total,value) => {
        return total + value;
    });
};
```

<br />
<br />

The reduce() function takes in the paremeters total which is the accumulator and a value. For each item in the array, it adds the value to the total and returns the sum. So, all in all, once you figure out that you were supposed to use a stack, this question becomes immensely easier. 

<em>The solution</em>

<br />
<br />

```javascript
var calPoints = function(ops) {
    let total = 0;
    let stack = [];
    let size = 0;
    for (i=0; i<ops.length; i++) {
        if (isNaN(ops[i])) {
            if (ops[i] === '+') {
                total+=(stack[size-1]+ stack[size-2]);
                stack.push(stack[size-1] + stack[size-2]);
                size+=1;
            } else if (ops[i] === 'D') {
                total+=(stack[size-1]*2);
                stack.push(stack[size-1]*2);
                size+=1;
            } else if (ops[i] === 'C') {
                let numRemoval = stack.pop();
                total-=numRemoval; 
                size-=1;
            }
        } else {
            let score = parseInt(ops[i]);
            stack.push(score);
            total+= parseInt(score);
            size+=1;
        }    
    }
    return total;
    
};
```
<br />
<br />

This one runs a bit faster than mine. Let's find out why. So we begin with initializing three variables. We then create a for loop and check if the current element is not a number. If it is not a number we have the 3 cases. We add the number to stack, total and we increment the size by one. For 'C', we create a variable for stack.pop() and subtract i from total. Then we decrement size by one. If it is a number, we add it to total and push to stack. Then we just push to stack.

I do not like a couple of things about this solution. It seems like size is a little bit unnecessary as we can just use stack.length. There is also no use for numRemoval. Actually, I just ran the solution and the runtime is the same as mine and uses more memory. The other javascript solutions are similar to mine. 

Well, all in all, I learned more about using stacks. That's all for today!



