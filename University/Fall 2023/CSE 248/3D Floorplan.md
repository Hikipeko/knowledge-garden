>Zhiyuan (Joel) Chen

![[Pasted image 20231012225336.png|650]]

##### Notations and Clarifications

* Each shape that I draw represent a set of 3D floorplans that can be obtained by rotating or flipping this basic shape.
* ![[Pasted image 20231012225901.png|300]]
* For n = 1 to 4, each shape is given a label. E.g. the above three shapes are referred to as 4-1, 4-2, and 4-3.
* ![[Pasted image 20231012230343.png|100]]
* 48 correspond to number of floorplans obtained from the basis shape (question 6). 2 correspond to number of floorplans assuming symmetry by rotation (question 7). 6 corresponds to number of floorplans assuming symmetry by flipping (question 9).
* When assuming that the floorplan is symmetric by flipping and rotation, each such floorplan correspond to one of the shape that I draw. Thus the number of shapes is the number of floorplans in question 9.

##### Explanation of how These Shapes are Obtained

To avoid duplication and omissions, I use the idea of recursion to draw all these shapes. I will explain how I obtain result for $n = 5$. The situations where $n <5$ is done in a similar way.

There are following ways that can generate 5 partition, which is 2D-extention, 1+2+2, 2+1+2, 1+1+3, 1+3+1, 1+4, 5. I enumerate all the situations in the above order. E.g. in the above image, 1+4-7 means a 1-partition floorplan stack with floorplan 4-7.

For example when doing 1+4, I make sure that I (1) exhaust all possible ways of staking and (2) the 1+4 cannot degrade to previous enumerations such as 1+1+3 and 1+2+2.

##### Result

The result is shown in the following table

| Problem num \ n | 1   | 2   | 3   | 4   | 5   | OEIS                 |
| --------------- | --- | --- | --- | --- | --- | -------------------- |
| 6               | 1   | 3   | 15  | 93  | 683 | Not found            |
| 7               | 1   | 1   | 2   | 9   | 47  | Not found            |
| 8               | 1   | 3   | 8   | 41  | 213 | Not found            |
| 9               | 1   | 1   | 2   | 8   | 38  | A191016 (not likely) |

![[Pasted image 20231012232249.png]]

##### Conclusion

For all the 3D situations, we didn't find any promising series corresponding to the series we get. Perhaps a brand-new series is needed for the 3D Floorplanning problem.