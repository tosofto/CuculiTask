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

Recursive function have to stop if it reaches to the last stone.

If not, it starts to follow the logic.

First of all, it loops through possible units that frog can increase or decrease upon previous jump.

While it loops, it also loop through possible position between the last stone to check if the frog can jump or not.

If the indecated position is too far to jump according to the previous jump, it stops loop.

Here, Too near factor is not be considered since it checks if the possible next jump is equal to the indicated position.

If the next jump matches to the indecated position, it jumps and go on checking & jumping.


```sh
 function isCrossRiver(stones, curPos, prevJump) {

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
                        if (isCrossRiver(stones, curPos + posDist, nextJump) == true){
                            
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
