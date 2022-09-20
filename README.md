# CuculiTask

[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/tonightFury1/CuculiTask)

A README file for CuCuli Task Solution.

This README file is for details of Frog Cross River Solution.

This repository contains:

[Frog.html ](Frog.html) for solution of Frog Cross River problm.


## Table of Contents

- [Problem](#Problem)
- [Solution](#Solution)
- [Usage](#Usage)
- [Autor](#Autor)
- [License](#License)

## Problem

A frog is crossing a river. The river is divided into some number of units, and at each unit, there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions (in units) in sorted ascending order, determine if the frog can cross the river by landing on the last stone. Initially, the frog is on the first stone and assumes the first jump must be 1 unit.

If the frog's last jump was k units, its next jump must be either k - 1, k, or k + 1 units. The frog can only jump in the forward direction.

 

### Example 1:

Input: stones = [0,1,3,5,6,8,12,17]
Output: true
Explanation: The frog can jump to the last stone by jumping 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 2 units to the 4th stone, then 3 units to the 6th stone, 4 units to the 7th stone, and 5 units to the 8th stone.
### Example 2:

Input: stones = [0,1,2,3,4,8,9,11]
Output: false
Explanation: There is no way to jump to the last stone as the gap between the 5th and 6th stone is too large.

### Constraints:

2 <= stones.length <= 2000
0 <= stones[i] <= 231 - 1
stones[0] == 0
stones is sorted in a strictly increasing order.


## Solution

This problem can be solved with recursive function.

But the time complexity of recursive solution is O(3^n) which means too slow as the array's length is increasing, while it's only O(n^2) in dynamic solution.

Dynamic programming solution is preferred for this problem.

### Recursitive Solution:

Here is recursitive solution.

Recursive function has to stop if it reaches to the last stone.

If not, it starts to follow the logic.

First of all, it loops through possible units that frog can increase or decrease upon previous jump.

While it loops, it also loop through possible position from the next stone to the last one to check if the frog can jump or not.

If the indecated position is too far to jump according to the previous jump, it stops loop.

Here, "Too near" factor will not be considered since there's procedure that checks if the possible next jump is equal to the indicated position.

If the next jump matches to the indecated position, it jumps and go on checking & jumping.


```sh
        /**
         *  Determine whether it is possible to jump from the current curPosition to the last stone or not.
         *  @param stones: array of stones' position.
         *  @param curPos: frog's current position.
         *  @param prevJump: frog's previous jump unit.
         *  @return true if frog can jump to the last stone, false if otherwise.
         */
         function canCrossRiver(stones, curPos, prevJump) {

            # if the current curPosition is the last stone, return true.
            if (curPos == stones.length - 1) return true;

            # loop through curPossible unit which can be applied to the previous prevJump.
            for (var jumpUnit = -1; jumpUnit < 2; jumpUnit ++) {

                # loop from the next stone to the last stone.
                for (var posDist = 1; curPos + posDist < stones.length; posDist ++) {

                    # the distance between the current position and the indecated position.
                    var dist = stones[curPos + posDist] - stones[curPos];

                    # if the distance is too long to jump, break.
                    if (dist > prevJump + 1 ) break;

                    # determine the next jump.
                    var nextJump = prevJump + jumpUnit;

                    # if the next jump matches to the distance of the next stone, continue recursive procedure.
                    if (nextJump == dist) {

                        # go on jumping.
                        if (canCrossRiver(stones, curPos + posDist, nextJump) == true){
                            
                            # if frog can go on, return true.
                            return true;                            
                        }

                    }

                }
            }

            # if there's not any possibility, return false.
            return false;
        }

```

### Dynamic solution:
And here is dynamic programming solution.

First, it checks if the given stones' array is not empty.

If not empty. it creates a new Object which it's key is stone's position and the value is the array of possible previous jumps to get this stone.

Then, it loops through the given stones.

while it loops, it checks if there is any possible previous jumps for the current stone, and if exists, it checks if the next jump can exceed the last stone.

if the next jump don't exceed the last stone, it stoes an array with possible jumps to get on this stone.

Finaly. when the loop finishes, there's jump history for almost stones.

If the last stone has a jump log, it means frog can jump to the last one though we can't track the full jump log to get to the last one.

Function returns the result.


```sh
       /**
         *  Determine whether it is possible to jump from the current curPosition to the last stone or not.
         *  @param stones: array of stones' position.
         *  @return true if frog can jump to the last stone, false if otherwise.
         */
        /**
         *  Determine whether it is possible to jump from the current curPosition to the last stone or not.
         *  @param stones: array of stones' position.
         *  @return true if frog can prevJump to the last stone, false if otherwise.
         */
        function canCrossRiver(stones) {

            if(stones.length > 0){
                 // create a new object for jump history and initialize it with first position.
                var jumpLogs = {"0":[0]};

                // loop through stones to create a history.
                stones.forEach( stone => {

                    // check if the history for current stone exists.
                    if(jumpLogs[stone]){

                        // if the history exists, loop through history's jump.
                        jumpLogs[stone].forEach(prevJump => {

                            // loop through possible jump, frog can.
                            for(let jump = prevJump - 1; jump <= prevJump + 1; prevJump++){
                                
                                // caculate the next jump.
                                let nextJump = parseInt(stone) + jump

                                // check if the jump is jumpable and not exceed the last stone.
                                if(jump > 0 && nextJump < stones.length){

                                    // if condition is true, create a new jump history and push prev jumps to current stone.
                                    jumpLogs[nextJump] = []
                                    jumpLogs[nextJump].push(jump)
                                }
                            }
                        })
                    }
                })

                // finally check if the last stone has got any possible jump and if exists, return true. Otherwise false.
                return jumpLogs[stones.slice(-1)].length() > 0
            }
           
        }
```



This part is for showing result on browser.
```sh
        # Result for the first input.
        document.write( "<p>Stones: " + stones1 + "</p>" );
        document.write( "<p>Result: " + isCrossRiver(stones1 /*stones*/, 0 /*curPos*/, 0 /*prevJump*/) + "</p>" );

        # Result for the second input.
        document.write( "<p>Stones: " + stones2 + "</p>" );
        document.write( "<p>Result: " + isCrossRiver(stones2 /*stones*/, 0 /*curPos*/, 0 /*prevJump*/) + "</p>" );
```
## Usage

Simply open Frog.html with browser and you can see the result.

```sh
        var stones1 = [0,1,3,5,6,8,12,17];
        var stones2 = [0,1,2,3,4,8,9,11];
        # Can replace this lines with the other input and refresh browser.
```
## Autor

[@DavidStrager](https://github.com/tonightFury1).


## License

[MIT](LICENSE) © David Strager
