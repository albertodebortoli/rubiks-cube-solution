# Rubik's Cube solution
### The simplest, easiest and quickest way to learn how to solve the Rubik's Cube.


![rubik header][1]

by [Alberto De Bortoli][2]
[@albertodebo][3]
[github.com/albertodebortoli][4]

v0.2-beta

25th January 2013
 
# Introduction
 
Here I explain my technique to solve the Rubik's Cube. I consider it the simplest, easiest and quickest way to learn-how-to-solve a cube. Mastering this technique you'll be able to solve a cube in c.a. 1 minute and 40 seconds.
Here is a [video](https://www.youtube.com/watch?v=3YLt8ULE2jc) where I made it in 1:45.5.
Not too good but still decent to impress someone. Practice and you'll master it. Follow rigorously every single word in this guide, if you miss a sentence or a step you're not going to do it and you'll give up shortly.

```
              --- --- --- 
             | 1y| 2y| 3y| 
              --- --- --- 
             | 4y| 5y| 6y| 
              --- --- --- 
             | 7y| 8y| 9y| 
              --- --- --- 
 --- --- ---  --- --- ---  --- --- ---  --- --- --- 
| 1b| 2b| 3b|| 1r| 2r| 3r|| 1g| 2g| 3g|| 1o| 2o| 3o| 
 --- --- ---  --- --- ---  --- --- ---  --- --- --- 
| 4b| 5b| 6b|| 4r| 5r| 6r|| 4g| 5g| 6g|| 4o| 5o| 6o| 
 --- --- ---  --- --- ---  --- --- ---  --- --- --- 
| 7b| 8b| 9b|| 7r| 8r| 9r|| 7g| 8g| 9g|| 7o| 8o| 9o| 
 --- --- ---  --- --- ---  --- --- ---  --- --- --- 
              --- --- --- 
             | 1w| 2w| 3w| 
              --- --- --- 
             | 4w| 5w| 6w| 
              --- --- --- 
             | 7w| 8w| 9w| 
              --- --- --- 
```
 
*fig. 1*: This is the map of a cube. *y = yellow*, *r = red*, *b = blue*, *g = green*, *w = white*, *o = orange*.

We name *pivot face* the face of the cube to use as reference for the sequence of moves we are going to apply. You should keep this face always in front of your eyes while learning.

## Basic moves
 
Here are all the available moves that can be applied to the cube. Consider the red face as pivot.
 
1. **RIGHT_down**: move `3r-6r-9r` to `3w-6w-9w`
2. **RIGHT_up**: move `3r-6r-9r` to `3y-6y-9y`
3. **LEFT_down**: move `1r-4r-7r` to `1w-4w-7w` 
4. **LEFT_up**: move `1r-4r-7r` to `1y-4y-7y` 
5. **MID_down**: move `2r-5r-8r` to `2w-5w-8w`
6. **MID_up**: move `2r-5r-8r` to `2y-5y-8y`
7. **UP_left**: move `1r-2r-3r` to `1b-2b-3b` 
8. **UP_right**: move `1r-2r-3r` to `1g-2g-3g` 
9. **DOWN_left**: move `7r-8r-9r` to `7b-8b-9b` 
10. **DOWN_right**: move `7r-8r-9r` to `7g-8g-9g` 
11. **MID_left**: move `4r-5r-6r` to `4b-5b-6b`
12. **MID_right**: move `4r-5r-6r` to `4g-5g-6g` 
13. **ROT_right**: move `1r-2r-3r` to `3r-6r-9r`
14. **ROT_left**: move `1r-2r-3r` to `7r-4r-1r`


#Algorithm 
 
1. Complete a face (colors on the edges must be correct)
2. Fix centers on each face (max 2 moves)
3. Fix the angles of the face opposite to the completed one
4. Fix the colors of angles of the face opposite to the completed one
5. Fix the remaining edges

### 1. Complete a face

Easy, just a little practice. The obtained face is the pivot. 

### 2. Fix centers on each face

Trivial.

### 3. Fix the angles of the face opposite to the completed one

If the angles of the face opposite to the *pivot face* are not in the correct position regardless of the colors, swaps are needed between *7b-7w-9o*, *9w-9g-7o*, *3y-3g-1o*, *1b-1y-3o*. 

Moves:

* **comboAngles_right**: *RIGHT_down*, *DOWN_right*, *RIGHT_up*, *DOWN_right*, *ROT_right*, *DOWN_left*, *ROT_left* 

* **comboAngles_left**: *LEFT_down*, *DOWN_left*, *LEFT_up*, *DOWN_left*, 
*ROT_left*, *DOWN_right*, *ROT_right* 


Algorithm: 

* put the former *pivot face* face up (in the example *pivot face* goes from red to white): simply rotate the cube to change the *pivot face*, no moves are needed;

* repeat *comboAngles_right* (or the symmetric one *comboAngles_left*) as long as the angles are put in the correct places regardless of the colors.

N.B. These moves don't break the former *pivot face*. 

### 4. Fix the colors of angles of the face opposite to the completed one

Let's give weights to the angles of the face opposite to the completed one.

* **0**: if the angle has colors already in place
* **1left**: if the angle has colors shifted on the right by 1
* **1right**: if the angle has colors shifted on the left by 1

Moves: 

* **comboColors_right**: *LEFT_down*, *DOWN_right*, *LEFT_up*, *DOWN_right*, 
*LEFT_down*, *DOWN_left* (x2) (o *DOWN_right* x2), *LEFT_up*, 
*DOWN_left* (x2) (or *DOWN_right* x2);
 
* **comboColors_left**: *RIGHT_down*, *DOWN_left*, *RIGHT_up*, *DOWN_left*, 
*RIGHT_down*, *DOWN_right* (x2) (o *DOWN_left* x2), *RIGHT_up*, 
*DOWN_right* (x2) (or *DOWN_left* x2)

Examples given bottom right angle *7b-7w-9o* in the *pivot face*. The final state will be: 

```
angle     weight  after cc_right  after cc_left 
--------- ------- --------------- --------------
7b-7w-9o  0       0               1right
9w-9g-7o  1right  0               1left
3g-3y-1o  1right  0               1left
1b-1y-3o  1right  0               1right
```

```
angle     weight    after cc_right  after cc_left
--------- --------- --------------- --------------
7b-7w-9o  0         0               1right
9w-9g-7o  0         1left           1right
3g-3y-1o  1right    0               1left
1b-1y-3o  1left     1right          1left
```

In every case an angle remains unaltered.

Algorithm:

* put the former *pivot face* face up (in the example *pivot face* goes from red to white): simply rotate the cube to change the *pivot face*, no moves are needed;

* apply *comboColors_right* (or the symmetric one *comboColors_left*) as long as the angles have weights equal to 0.

*comboColors_right* and *comboColors_left* are symmetric, both keep an angle unaltered and increase or decrease the weight of the other 3 angles by 1 (from 1right to 1left or from 1left to 1right). 

This is tough, keep practicing it.
 
### 5. Fix the remaining edges

At this point only the edges need to be placed in the correct positions. We are going to do it moving the edges 3 by 3 clockwise or counterclockwise. We will use combinations of both *comboColors_right* and *comboColors_left*. 

Example:
Given a solved face, rotating 8b-4b-2b, the starting pivot will be as in *fig. 2*.

``` 
 --- --- ---       --- --- ---       --- --- --- 
| 1b| 2b| 3b|     | 7b| 4b| 1b|     | 7o| 4o| 1o| 
 --- --- ---       --- --- ---       --- --- --- 
| 4b| 5b| 6b| --> | 8b| 5b| 2b| --> | 8o| 5o| 2o| 
 --- --- ---       --- --- ---       --- --- --- 
| 7b| 8b| 9b|     | 9b| 6b| 3b|     | 9o| 6o| 3o| 
 --- --- ---       --- --- ---       --- --- --- 
```
 
*fig. 2*: Simply rotate the cube to change the *pivot face*, no moves are needed.

**Optimal state**: given 3 edges to fix, the application of the following algorithm fixes all of them

**Sub-optimal state**: given 3 edges to fix, the application of the following algorithm fixes 1 or 2 of them

Now we need to move edges clockwise.

Algorithm to move the edges clockwise:

1. apply all the moves in *comboColors_right* but the last 2;
2. rotate the cube on the left, no moves are necessary;
3. apply all the moves in *comboColors_left* but the last 2.

Algorithm to move the edges counterclockwise:

1. apply all the moves in *comboColors_left* but the last 2;
2. rotate the cube on the right, no moves are necessary;
3. apply all the moves in *comboColors_right* but the last 2.

Hard thing here is that it is likely not to have optimal state that allow to fix edges simply using the algorithm described above, so you might want to apply some pre-moves to reach an optimal or sub-optimal state required to apply the algorithm.

This is even more tough than the previous step.

## Little trick to fool newbies

Swapping centers and something more...
 
**Preconditions**:

* start with a solved cube
* pick up a move in: *MID_down*, *MID_up*, *MID_left* or
*MID_right*
* pick up a direction between left or right.
 
**Algorithm**:

1. apply the chosen move on the *pivot face* 
2. turn around the cube by 90 degrees in the chosen direction keeping the *pivot face* in front 
3. repeat 1) and 2) three times
4. *RIGHT_DOWN* (x2) e *DOWN_right* (x2) 
 
Solution: c'mon! Make that brain work!

[1]: rubik_header.png
[2]: http://albertodebortoli.com
[3]: http://twitter.com/albertodebo
[4]: http://github.com/albertodebortoli
