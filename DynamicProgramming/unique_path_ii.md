# Unique Path II

### Question:

Follow up for [Unique Path](unique_path.html).

Now consider if some obstacles are added to the grids and the robot cannot bypass those obstacles. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

| S | 0 | 0 | 0 | 0 |
|---|---|---|---|---|
| 1 | 0 | 0 | 0 | 1 |
| 0 | 0 | 0 | 1 | 0 |
| 1 | 0 | 0 | 0 | D |


---

### Think Out Loud:

This is a typical dynamic programming problem. Let us assume the robot is standing at `(i, j)`. How did the robot arrive at `(i, j)`? The robot could move down from `(i - 1, j)` or move right from `(i, j - 1)`. So the path to `(i, j)` is equal to the sum of path to `(i - 1, j)` and path to  `(i, j - 1)`. But remember if there is an obstacle in `(i, j)`, the path to `(i, j)` will be zero since it unreachable. We can use another 2D array to store the path to all node and use the equation below:

```
if obstacle(i, j) == 1
   paths(i, j) = 0
else
    paths(i, j) = paths(i - 1, j) + paths(i, j - 1) // i != 0 and j != 0
    paths(i, j) = paths(i - 1, j) // j == 0 and i != 0
    paths(i, j) = paths(i, j - 1) // i == 0 and j != 0
```

However, given more thoughts, you will find out that you don't actually need a 2D array to record all the values since when the robot is at row `i`, you only need the paths at `(i - 1)`.  So the equation is:

```
// initialize paths with default value as 0
if obstacle(i, j) == 1
   paths(j) = 0
else if j > 0
    res[j] = res[j] + res[j - 1];
```



### Show Me Your Code:

##### cpp

```cpp
int uniquePathsWithObstacles(vector<vector<int> > &obstacleGrid) {
    int m = obstacleGrid.size();
    if(m == 0)
        return 0;
    int n = obstacleGrid.begin()->size();
    if(n == 0)
        return 0;
    
    vector<int> paths(n, 0);
    
    paths[0] = 1;
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(obstacleGrid[i][j]) { //this will set paths[0] to 0 if obstacleGrid(0, 0) is 1
                paths[j] = 0;
            } else if (j > 0) {
                paths[j] = paths[j] + paths[j - 1];
            }
        }
    }
    return paths[n - 1];
}
```



##### javascript

```javascript
/**
 * @param {number[][]} obstacleGrid
 * @return {number}
 */
var uniquePathsWithObstacles = function(obstacleGrid) {
    const cols = obstacleGrid[0].length;
    
    const paths = Array.apply(null, Array(cols)).map(() => 0);
    paths[0] = 1;
        
    obstacleGrid.forEach((currentRow) => {
        currentRow.forEach((element, colIndex) => {
            if (element == 1) {
                paths[colIndex] = 0;
            } else if (colIndex > 0) {
                paths[colIndex] = paths[colIndex] + paths[colIndex - 1];
            }
        });
    });
    
    return paths[cols - 1];
};
```

