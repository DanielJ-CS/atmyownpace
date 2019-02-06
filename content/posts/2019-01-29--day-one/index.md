---
title: Day One - Uncommon Words from Two Sentences
category: "Strings"
cover: day1.png
author: Daniel Jiang
---
The question for today comes from leetcode:

<code>We are given two sentences A and B. 
A word is uncommon if it appears exactly once in one of the sentences, and does not appear in the other sentence.
Return a list of all uncommon words. 
You may return the list in any order.</code>
<br />
<br />
Well, let's look at some examples to make sense of what we are inputting and outputting.
<br />
<br />
<code>
Input: A = "this apple is sweet", B = "this apple is sour"
<br />
Output: ["sweet","sour"]
<br />
</code>
<code>
Input: A = "apple apple", B = "banana"
<br />
Output: ["banana"]
</code>
<br />
<br />

So the first thing that I look at is the inputs and outputs. I am inputting two strings and returning a list of strings. It seems like the solution template that leetcode provides wants me to output an array of strings. Since the question is working in words, I need to find a way to divide up the strings into individual words. 

Take the first example, sentence A will contain "this", "apple", "is", "sweet" and sentence B will contain "this", "apple", "is", "sour". Each word is divided by spaces. For my first attempt, I can try creating a string array with each individual word. Let's try this:
<br />
<br />

```javascript
public String[] uncommonFromSentences(String A, String B) {
    String[] aArray = A.split("\\s+");
    String[] bArray = B.split("\\s+");
}
```
<br />
<br />

With a bit of searching stackoverflow, I came across a way to divide up the given strings by spaces. The function to do this is <code>String.split()</code>. The regex that is used is <code>\\\s+</code> which means that I am splitting on multiple spaces. A singular space can be represented by <code>\\\s</code>. Let's look more into some  <a href="http://www.vogella.com/tutorials/JavaRegularExpressions/article.html">regex</a> since I am quite new to this. Another regex example that came up while looking at stackoverflow is <code>\\\W</code> and <code>\\\W+</code> which in combination with the split function, would mean that I am splitting on a singular or multiple non-word character(s). When dealing with numbers, <code>\\d</code> can be used for a digit and <code>\\D</code> can be used for splitting on a non-digit.

Carrying on, I currently have two arrays that look like:

```javascript
["this","apple","is","sweet"]
["this","apple","is","sour"]
```
<br />
<br />

Now I want to take this and remove all the duplicates. The problem with this is that if I want to match up aArray[0] to all the elements in bArray, this will probably be too high of a runtime. What I am talking about is brute forcing it with two for loops. Runtime wise, it would be O(nm) where n is the length of aArray and m is the length of bArray. Another way that I thought of is to do it by dictionaries/hashmaps. Runtime wise, putting everything into the hashmap is O(n+m) and checking if each one has a count of two or more is also O(n+m), which is much better than O(nm).

```javascript
public String[] uncommonFromSentences(String A, String B) {
         String[] aArray = A.split("\\W+");
         String[] bArray = B.split("\\W+");
         
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i < aArray.length; i++) {
            if (!map.containsKey(aArray[i])) {
                map.put(aArray[i],1);
            } else {
                map.put(aArray[i],map.get(aArray[i]) + 1);
            }
        }
         for (int i = 0; i < bArray.length; i++) {
            if (!map.containsKey(bArray[i])) {
                map.put(bArray[i],1);
            } else {
                map.put(bArray[i],map.get(bArray[i]) + 1);
            }
        }
    }
```
<br />
<br />

I changed the regex to <code>\\\W+ </code> as it deals with all non words and not just spaces. Next I created a HashMap called map. I was thinking of not checking keys for aArray but a sentence can have more than one of the same word and would disqualify it from being an uncommon word. Therefore, I just check all keys. 

From here, I can just find all words that only appear once and put them in an array:

```javascript
public String[] uncommonFromSentences(String A, String B) {
         String[] aArray = A.split("\\W+");
         String[] bArray = B.split("\\W+");
         List<String> sol = new LinkedList<>(); 
         
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i < aArray.length; i++) {
            if (!map.containsKey(aArray[i])) {
                map.put(aArray[i],1);
            } else {
                map.put(aArray[i],map.get(aArray[i]) + 1);
            }
        }
         for (int i = 0; i < bArray.length; i++) {
            if (!map.containsKey(bArray[i])) {
                map.put(bArray[i],1);
            } else {
                map.put(bArray[i],map.get(bArray[i]) + 1);
            }
        }
        for (String key: map.keySet()) {
            if (map.get(key) == 1) {
                sol.add(key);
            }
        }
        return sol.toArray(new String[sol.size()]);
        
    }
```
<br />
<br />

So, some problems that I had in the last portion of this question is that to instantiate a string array in java, I need to declare the size. This contridicts the order of my code as I want to add the keys from the Hashmap to the solution directly but I do not know how many elements appear once in the Hashmap before hand. To fix this, I created a LinkedList instead of a string array and used the function toArray() after I iterated through all the keys in the Hashmap, when I knew the amount of keys in the solution. 

Aside from that I iterated through the Hashmap by the keys, using a for loop for the Hashmap's keyset. If I needed both key and value then I would have had to iterate through the entryset instead. 

<br />
<br />

#<em>The Solution</em>
<br />

```javascript
class Solution {
    public String[] uncommonFromSentences(String A, String B) {
        Map<String, Integer> count = new HashMap();
        for (String word: A.split(" "))
            count.put(word, count.getOrDefault(word, 0) + 1);
        for (String word: B.split(" "))
            count.put(word, count.getOrDefault(word, 0) + 1);

        List<String> ans = new LinkedList();
        for (String word: count.keySet())
            if (count.get(word) == 1)
                ans.add(word);

        return ans.toArray(new String[ans.size()]);
    }
}
```

<br />
<br />

Description:
<br />

<code> Every uncommon word occurs exactly once in total. We can count the number of occurrences of every word, then return ones that occur exactly once. </code>

Let's compare. The big differences is the getOrDefault() function. From the <a href="https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html">Java Docs</a>,
the definition of this function is:

<code> getOrDefault(Object key, V defaultValue):<br />
Returns the value to which the specified key is mapped, or defaultValue if this map contains no mapping for the key.</code>

So the first input is the key to get and the second input is the default value. If the key can be found then the function adds 1 to it, otherwise, it sets the default value as 0 and we add one to it. 

Another thing is that when we split the Strings, we can already iterate through the split words, so I didn't need to create a variable for the words. Interesting, I wonder if the concepts of mutable and immutable objects can effect the decision to do this.

Anyways, this is the first day of questions with many more to come.