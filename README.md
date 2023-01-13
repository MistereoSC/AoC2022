
# Advent of Code 2022

[Advent of Code](https://adventofcode.com/2022) is an annual competition to solve 25 coding puzzles.
These are my solution for the 2022 puzzles, written in JavaScript and bundled as a small webpage.
The webpage is rudimentary and serves just to collect all my solutions in one place.
<br><br>
Puzzles solved currently: 12/25


# Hints
Some hints that may help find a solution to the puzzles, or where I got a bit stuck on for a bit.

### Day 1:
**Q1+Q2:** Not much to say, sum up all lines until an empty line, and push it to an array. Now you only have to find the (3) highest values in that array.

### Day 2:
**Q2+Q2:** The easiest solution is to manually calculate the result of each line (in a switch), for example "A X"=3 and sum all the results

### Day 3:
**Q1+Q2:** You have to split each line in two halves, and find all characters that are present in in both halves, no real hints I can give here. For the priorities, if you have the character of the duplicate Item, you can use its ASCII value. 'a' has an ASCII value of 97 but is Priority 1, 'A' is 65 but is priority 27. With some minor logic and some subtraction, it is quite easy to get the priority value. Q2 is basically the same process, but you have to compare 3 strings, instead 2 halves of a string now.

### Day 4:
**Q1+Q2:** You only have to compare the start and end values for each pair. If the first elv starts lower, and ends higher than the second, the right elv's task are completely contained in the left's and so on. This is also true for Q2, although the logic is slightly different. It's easy if you visualize the elv's tasks:
```sh
1-2-3-4-5-6-7-8-9
. # # # # # . . .
. . . # # # # # .
```

### Day 5:
**Q1:** The hard part is to parse the input, you can either use RegEx, or manually split the input in blocks (of 4 characters) and get the relevant characters that way. You can now read each column from top to bottom, and push each character to a push-pop-stack.<br>
Stepping through the moves now simply requires to pop an Item from stack X and pop it to stack Y, n times.
<br><br>

**Q2:** It's basically the same as Q1, but you have to reverse the order you push the blocks to stack Y. Instead of directly pushing the blocks to stack Y one by one, you can pop all blocks from stack X and push them to a separate stack, and then pop and push all to Stack Y.

### Day 6:
**Q1+Q2:** The simplest way is to loop through the entire input, character by character, and check if the current character, and the 3 (13) before that, are unique to each other. Not much I can say, it's just some comparison trickery. You don't have to compare **all** the characters though, it's enough if you compare 1-2, 1-3, 1-4,  2-3, 2-4,  3-4.

### Day 7:
**Q1:** This one was a bit tricky. The first step is to step through each program instructions, and figure out what to do (you can basically completely ignore '$ ls'). The most logical solution to me, was to store all the files in a binary tree. so each directory is a node, and each file a leaf. You also need a pointer, that points to the root node. After the '$ ls' instruction, create a child node at the current pointer. Each Node (including the root, files, and directories) should have a 'size' property, a pointer to its parent AKA the current pointer for your position in the tree, and an array of child nodes. Files do not have child nodes tho. 
<br>
On '$ cd X' you need to set the pointer to directory X(pointer = pointer.X). On '$ cd ..' you step up one level in the tree (pointer=pointer.parent).
Finally you simply have to (tail) recursively step through each child node and calculate its size, by adding up the size of all its children.<br>
JavaScript doesn't like loops within JSONs, so instead of saving a pointer to the parent, I kept track of the pointer location with a push-pop-stack
<br><br>

**Q2:** Now that you have calculated the size of all directories, including the root directory, you need to figure out how much space at the minimum you need to free up. You know how much free space you need, how much you used, and how much you can have at max. While calculating the size of each directory, you hopefully also have all their sizes stored in a separate List or Array. You don't need the paths for each directory, only their sizes. In that list, you simply have to find the smallest number, that is bigger than 30.000.000

### Day 8
**Q1+Q2:** Not many hints I can give here. For Q1 you have to check each field in each direction, if there is a tree larger than itself between its location and the edge of the array, for Q2 you have to instead find the distance to the first tree larger than itself.

### Day 9:
**Q1:** The first one is quite easy, you simply have to keep track of the head and tail's position, and also the head's previous position. After each move, if the head has a distance of greater than 1 from the tail, the tail's new position is the head's previous position. Don't move all the spaces at once, only move the Head 1 position at a time! The tricky part is that we don't know how large the area should be, or what the starting position is. I chose a generously large 400\*400 area, setting the center as the starting position, but you may have to tinker around a bit (or figure out another way how big the play area should be beforehand). For counting, you can keep track of the tails positions by pushing each position on a stack and filter out duplicates afterwards, or an array and summing up each position the tail has been in.
<br><br>

**Q2:** This one is a bit more tricky since you actually have to implement the logic of the tail 'physics', and you have 10 moving pieces instead of one. Just following the previous piece doesn't cut it this time. You basically have to figure where to move based on the previous piece's position
```sh
o o x o o
o - - - o
x - T - x
o - - - o
o o x o o
```
There are 16 positions where the tail (T) can be, relative to the head. The inner 8 (-) don't move the tail! When the head is straight across 2 positions (x), you only have to move the tail straight one position, otherwise (o) you have to move diagonally. After you have moved the head, check each following piece if and where it has to move, relative to it's previous piece.

### Day 10:
**Q1:** The tricky part here is to put the checks, cycle increases and addition in exactly the right spot. Keep track of cycles separately from your program counter, and loop through the program. You should check if a breakpoint is reached after **each** cycle increase, but **before** you make the addition.
<br><br>

**Q2:** The sprite you have to check against spans all 6 rows. You can build up the output string by adding characters one by one. You have to figure out where to put a '\n' which is **after** every 40th cycle increase. And you have to figure out whether to put an '.' or '#' **before** each cycle increase


### Day 11:
**Q1:** There aren't many hints I can give here. The hard'ish part is to get all the necessary operations from the input. Each monkey is its own object and has 7 attributes:
the items it is holding, the amount of items it has inspected, the sign of the operation (+ or \*), the value for the operation, the 'divisible by X', what monkey it throws to if the test is false, and true.<br>Checking if a number is divisible by X is easy with a modulo operation. X is divisible by 7 if X%7==0
<br><br>

**Q2:** The worry levels get incredibly large, larger than feasible to store in an integer, or even a long. The only thing that matters about the worry level tho, is if it is divisible by the monkeys 'divisible by x' test. For lets say x=10, it doesn't matter if the worry level is 12, 1002 or 9898982, in each case worryLevel%x is always 2, so if you safe it as worryLevel=worryLevel%Y no relevant information gets lost, and the numbers stay small, where Y is the product of all the values the monkeys check (in the example given it is 23\*19\*13\*17)

### Day 12:
**Q1:**  I used some sort of Dijkstra Algorithm for solving the maze. From the starting point, visit each node (recursively) and keep track of how many steps it took to reach a node. If a node was visited before, and was reached faster than the current recursion branch, terminate that branch. Repeat until the endpoint is reached.
<br><br>

**Q2:** You don't have to repeat the search for every node with elevation 'a', only those adjacent to nodes with elevation 'b', since otherwise they are unreachable, or only adjacent to other 'a's. If you reverse the Start and End points, you don't even have to run the algorithm more than once!


### Screenshots

![Screenshot](https://i.imgur.com/pRsPqk5.png)

  

### Project Setup

```sh

npm install

npm run dev

```