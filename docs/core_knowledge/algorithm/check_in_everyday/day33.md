---
comments: true
---

## [62.不同路径](https://leetcode.cn/problems/unique-paths/)

转为求二叉树叶子节点的个数, 利用dfs ([如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201209113602700.png))

- TLE 38/63 cases passed(N/A)

```cpp linenums="1"
int dfs(int i, int j, int m, int n) {
    if(i > m || j > n) return 0; // 越界
    if(i == m && j == n) return 1; //找到了叶子节点
    return dfs(i + 1, j, m, n) + dfs(i, j + 1, m, n);

}
int uniquePaths(int m, int n) {
    return dfs(1, 1, m, n);
}
```

利用DP --> 机器人从`(0,0)` 位置出发，到`(m - 1,n - 1)`终点

1. `dp[i][j]`：表示从`(0,0)`出发，到`(i, j)`有dp[i][j]条不同的路径
2. `dp[i][j]` 只能从两个方向来,即`dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`
3. 初始化: `dp[i][0] = 1`，因为从(0, 0)的位置到(i, 0)的路径只有一条, `dp[0][j]`也同理
4. 从左到右一层一层遍历
5. 举例: [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201209113631392.png)

```cpp linenums="1" hl_lines="2"
int uniquePaths(int m, int n) {
    vector<vector<int>> dp(m, vector<int>(n));
    for(int i = 0; i < m; i ++) dp[i][0] = 1;
    for(int j = 0; j < n; j ++) dp[0][j] = 1;
    for(int i = 1; i < m; i ++)
        for(int j = 1; j < n; j ++)
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
    return dp[m - 1][n - 1];
}
```

## [63.不同路径II](https://leetcode.cn/problems/unique-paths-ii/)

1. `dp[i][j]`：表示从`(0,0)`出发，到`(i, j)`有dp[i][j]条不同的路径
2. `dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`
   
    - `(i, j)`若是障碍的话应该就保持初始状态（初始状态为0） 
  
3. 如果`(i, 0)`这条边有了障碍，障碍之后的`dp[i][0]`为初始值0, (0,j)同理
4. 从左到右一层一层遍历
5. 举例--> [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210104114610256.png)

```cpp linenums="1" hl_lines="3"
int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
    int m = obstacleGrid.size(), n = obstacleGrid[0].size();
    vector<vector<int>> dp(m, vector<int>(n, 0));
    // 若起点或终点有障碍,直接返回
    if(obstacleGrid[m - 1][n - 1] == 1 || obstacleGrid[0][0] == 1) return 0;
    // dp数组初始化
    for(int i = 0; i < m && obstacleGrid[i][0] == 0; i ++) dp[i][0] = 1;
    for(int j = 0; j < n && obstacleGrid[0][j] == 0; j ++) dp[0][j] = 1;
    // 遍历求dp数组
    for(int i = 1; i < m; i ++) 
        for(int j = 1; j < n; j ++) {
            if(obstacleGrid[i][j] == 1) continue; // 又障碍,跳过
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    return dp[m - 1][n - 1];
}
```