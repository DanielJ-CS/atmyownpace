---
title: Day Two - Island Perimeter
category: "Iterative"
cover: day2.png
author: Daniel Jiang
---

Today's question is:

<code>You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.</code>

with examples:

<code>
Input:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

Output: 16
</code>

Let's analyze the example. So the first thing that we see is that the input is an array of integer arrays. The first element of the array has the coordinates [0,0] and is a 0 which means that it is water. The second element is a 1 which means that it is land and has the coordinate [1,0]. This means that if you go up grid by one, you are out of bounds, so the top counts as water. We see that the left and right of [1,0] are both 0's which means that they are part of the perimeter. The bottom of [1,0] is a 1, therefore we do not add it to the perimeter. Thus, [1,0] adds three to the perimeter in total.

<em>What does this tell us?</em>

Well, if we think of the whole input as a grid and each cell of the grid as having an x and y coordinate, then to find the whole perimeter, we should first find the cells to the top/bottom/right/left direction of the current cell. If the cell in one of the directs is out of bounds or an 0, we can increase the perimeter by 1. 

```javascript
public int islandPerimeter(int[][] grid) {
        int colLen = grid.length;
        int rowLen = grid[0].length;
    }
```

<br />
So to start it all off, I think that first and foremost, we should know how to get the column and row length of a 2D array or int[][] in java. To do this, the length of the variable returns the column length. For the row length we have to use grid[1].length which returns the length of the first row. If the rows are not equivalent and we want the length of the second row, we would use grid[2] and so on. 
<br />
<br />

```javascript 
    public int islandPerimeter(int[][] grid) {
        int colLen = grid.length;
        int rowLen = grid[0].length;
        for (int i=0; i < colLen;i++) {
            for (int j=0; j< rowLen; j++) {
                System.out.println(grid[i][j]);
            } 
        }
        return 0;
    }
```

<br />
Here, we are simplying going through all the cells in each row then moving down a column. In a int[][], this would traverse all the elements in order. Now I want to somehow check if each direction of a cell is either out of bounds or a 0.
<br />
<br />

```javascript 

class Solution {
    public int islandPerimeter(int[][] grid) {
        int peri = 0;
        int colLen = grid.length;
        int rowLen = grid[0].length;
        for (int i=0; i < colLen;i++) {
            for (int j=0; j< rowLen; j++) {
                if (grid[i][j] == 1) {
                    peri += checkZeroes(grid,i,j);
                }
            } 
        }
        return peri;
    }
    
    public int checkZeroes(int[][] grid, int x, int y) {
        int count = 0;
        if (x == 0 || grid[x-1][y] == 0) count++;
        if (x == grid.length - 1 || grid[x+1][y] == 0) count++;
        if (y == 0 || grid[x][y-1] == 0) count++;
        if (y == grid[0].length - 1 || grid[x][y+1] == 0) count++;
        return count;
    }
}
```
<br />
<br />

Okay, so I ran into quite a few problems while working on the solution from the last point. First of all, I created a local variable called peri that would increase everytime a zero or out of bounds was found. Then I created a helper function that took in the grid and the current coordinates. The helper function would return an integer value that is at greatest four. So, the main issue I had is a bunch of out of bounds errors. The first one was because x could at most be the length - 1, instead of just the length. Simple enough, arrays start at 0 not 1, my mistake. 

The second was that I mixed up the second and fourth conditional, in the sense that I used grid[0].length for the second and grid.length for the fourth. Why did I do this in the beginning? We see that the second conditional is the x coordinate so the out of bounds would be when the x value is at the most right. After a bit of search on stackoverflow, I find out that my understanding of the column and row lengths are fundamentally wrong. Instead, it is not column length that can be found by going straight down, but the number of columns. For grid[0].length, we take the first row and count the length going across. So I flipped the two, whoops.
<br />
<br />
#<em>The Solution</em>
<br />
<br />

``` javascript

public class Solution {
    public int islandPerimeter(int[][] grid) {
        int islands = 0, neighbours = 0;

        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[i].length; j++) {
                if (grid[i][j] == 1) {
                    islands++; // count islands
                    if (i < grid.length - 1 && grid[i + 1][j] == 1) neighbours++; // count down neighbours
                    if (j < grid[i].length - 1 && grid[i][j + 1] == 1) neighbours++; // count right neighbours
                }
            }
        }

        return islands * 4 - neighbours * 2;
    }
}
```

<br />
<br />

Probably the hardest solution to understand from the java discussion of the solutions. Let's try to make sense of this. So the first thing that I see is that he saves memory space by not creating the row and column length variables. I should start picking up that habit. He has two variables, one for islands and another for neighbours. If the cell is a 1, the islands increase by 1. Then he creates two if statements, the first to check if the x and y coordinate are at the edge and the second to see if the cell to the right and down direction is an island. If it is, neightbours go up. Then he simply does some math. 

<em>Why does this work?</em>

Well, my outtake on this is that it is better to imagine an example. Imagine two cells next to each other with water everywhere else. We get to the first cell in the for loop and we now have 1 island. We check down, nothing is there. We check right, there is the second cell. We increase neighbours by one. Move on to the second cell, there is nothing to the right or down direction, but we didn't account for the left and up direction. Therefore, we need to double neighbours as this neighbour takes out 1 from the total four sides from both cells. 

A couple of the other solutions are similar to mine but they start with an integer value at 4 and decrease everytime a direction is not 0. Basically the opposite approach that I took. Here is portion of the code used:
<br />
<br />

```javascript

if (grid[i][j] == 1) {
    sum += 4;
    for (int[] dir : dirs) { 
        int x = i + dir[0], y = j + dir[1];
        if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) sum--;
    }
}
```
<br />
<br />

Actually sum is declared outside the for statements with a value of zero, so it keeps updating as the loop continues. Unlike my solution, we do not need to generate a helper function with an extra variable each time. This saves memory. Very useful and much better to do it this way. Well, that's it for today!

