# Unique Path

### Question:

A robot is located at the top-left corner of a m x n grid.

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid.

| S    | n    | n    | n    | n     |
| ---- | ---- | ---- | ---- | ----- |
| n    | n    | n    | n    | n     |
| n    | n    | n    | n    | n     |
| n    | n    | n    | n    | **D** |


How many possible unique paths are there?

---

### Think Out Loud:

**Method 1:** We can solve this problem with simple math skill. The requirement is that you can only move either down or right at any point. So it requires exact **(m + n)** steps from **S** to **D** and **n** out of **(m + n)** steps go down. Thus, the answer is **C(m + n, n)**. 

**Method 2: ** Let us solve the issue in computer science way. This is a typical dynamic programming problem. Let us assume the robot is standing at `(i, j)`. How did the robot arrive at `(i, j)`? The robot could move down from `(i - 1, j)` or move right from `(i, j - 1)`. So the path to `(i, j)` is equal to the sum of path to `(i - 1, j)` and path to  `(i, j - 1)`. We can use another array to store the path to all node and use the equation below:

```
paths(i, j) = 1 // i == 0 or j == 0
paths(i, j) = paths(i - 1, j) + paths(i, j - 1) // i != 0 and j != 0
```

However, given more thoughts, you will find out that you don't actually need a 2D array to record all the values since when the robot is at row `i`, you only need the paths at `(i - 1)`.  So the equation is:

```
paths(j) = 1 //j == 0 for any i
paths(j) = paths(j - 1) + paths(j) // j != 0 for any i
```



### Show Me Your Code:

##### cpp

```cpp
//with 2D array
int uniquePaths(int m, int n) {
  if(m == 0 || n == 0) {
    return 0;
  }

  vector<vector<int>> res(m, vector<int>(n, 1));  
  for(int i = 1; i < m; i++) {
    for(int j = 1; j < n; j++) {
      res[i][j] = res[i - 1][j] + res[i][j - 1];
    }
  }
  return res[m - 1][n - 1];
}

//with 1D array
int uniquePaths(int m, int n) {
    if(m == 0 || n == 0)
        return 0;
        
    vector<int> res(n, 1);
    
    for(int i = 1; i < m; i++) {
        for(int j = 1; j < n; j++) {
            res[j] = res[j - 1] + res[j];
        }
        
    }
    return res[n - 1];
}
```



##### javascript

```javascript
const createTwoDimenssionsArray = function createTwoDimenssionsArray(row, col, default_val) {
	return Array.apply(null, Array(row)).map(() => { 
		return Array.apply(null, Array(col)).map(() => default_val); 
	});
};
// with 2D array
var uniquePaths = function(m, n) {
	if (m == 0 || n == 0) {
		return 0;
	}

	const res = createTwoDimenssionsArray(m, n, 1);
	for(var i = 1; i < m; i++) {
		for(var j = 1; j < n; j++) {
			res[i][j] = res[i - 1][j] + res[i][j - 1];
		}
	}
	return res[m - 1][n - 1];
};

// with 1D array
var uniquePaths = function(m, n) {
	if (m == 0 || n == 0) {
		return 0;
	}
	const res = Array.apply(null, Array(n)).map(() => 1);
	
	for(var i = 1; i < m; i++) {
		for(var j = 1; j < n; j++) {
			res[j] = res[j - 1] + res[j];
		}
	}
	return res[n - 1];
};
```

