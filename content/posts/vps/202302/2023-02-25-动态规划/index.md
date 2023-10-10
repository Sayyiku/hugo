---
title: "动态规划"
date: "2023-02-25"
categories: 
  - "前端"
---

参考文章：[动态规划](https://zhuanlan.zhihu.com/p/91582909)

## 动态规划的三大步骤

动态规划，无非就是利用历史记录，来避免我们的重复计算。而这些历史记录，我们得需要一些变量来保存，一般是用一维数组或者二维数组来保存。

**步骤一：定义数组元素的含义。**前面提到一般用数组来保存历史记录，假设用一维数组 dp\[\]，这时候每个数组元素的含义非常重要。

**步骤二：找出数组元素之间的关系式。**动态规划类似于我们高中学习时的归纳法，可以利用 dp\[n-1\]、dp\[n-2\]...dp\[1\]，进而推出 dp\[n\]，也就是可以利用历史数据来推出新的元素值，所以我们要找出数组元素之间的关系式，例如 dp\[n\] = dp\[n-1\] + dp\[n-2\]，这个就是他们的关系式了。

**步骤三：找出初始值。**即便知道了数组元素之间的关系式，也需要根据初始值才能推导出 dp\[n\]。

## 青蛙跳台阶

> 一只青蛙一次可以跳上 1 级台阶，也可以跳上 2 级。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

1. 定义数组元素的含义：跳上一个 i 级的台阶总共有 dp\[i\] 种跳法，我们的目标是求 dp\[n\]。
2. 找出数组元素之间的关系式：对于这道题，由于情况可以选择跳一级，也可以选择跳两级，所以青蛙到达第 i 级的台阶有两种方式：一种是从第 i-1 级跳上来，一种是从第 i-2 级跳上来，所以所有可能的跳法的 dp\[i\] = dp\[i-1\] + dp\[i-2\]。
3. 找出初始值：dp\[0\] = 0; dp\[1\] = 1; dp\[3\] = 3;  
    

得出求解代码：

```
const f = (n) => {
  const dp = []
  dp[0] = 0
  dp[1] = 1
  dp[2] = 2
  for (let i = 3; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2]
  }
  return dp[n]
}

console.log(f(2)) // 2
console.log(f(3)) // 3
console.log(f(4)) // 5
console.log(f(5)) // 8
```

## 不同路径

题目来源：[不同路径](https://leetcode.cn/problems/unique-paths/)

> 一个机器人位于一个 m x n 网格的左上角。机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角。问总共有多少条不同的路径？

1. 定义数组元素的含义：我们把网格看作一个二维数组，起点坐标为 (0,0)，右下角坐标为 (m-1)(n-1)，当机器人从左上角走到 (i,j) 这个位置时，一共有 `dp[i][j]` 种路径，所以我们的目标是求 `dp[m-1][n-1]`。
2. 找出数组元素之间的关系式：达到 (i,j) 这个位置有两种方式：一种是从 (i-1,j) 这个位置到达，一种是从 (i,j-1) 这个位置到达，所以所有达到的方式 `dp[i][j] = dp[i-1][j] + dp[i][j-1]`。
3. 找出初始值：可以易知，当 i=0 或者 j=0 时，只能往右或者往下，方式只有一种。  
    

得出求解代码：

```
var uniquePaths = function (m, n) {
  if (m <= 0 || n <= 0) {
    return 0
  }

  // 初始化二维数组
  const dp = new Array(m).fill(0).map((_) => new Array(n).fill(0))
  // 当只有一列时
  for (let i = 0; i < m; i++) {
    dp[i][0] = 1
  }
  // 当只有一行时
  for (let j = 0; j < n; j++) {
    dp[0][j] = 1
  }

  for (let i = 1; i < m; i++) {
    for (let j = 1; j < n; j++) {
      dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
    }
  }

  return dp[m - 1][n - 1]
}

console.log(uniquePaths(3, 2)) // 3
console.log(uniquePaths(3, 3)) // 6
console.log(uniquePaths(2, 1)) // 1
```

## 最小路径和

题目来源：[最小路径和](https://leetcode.cn/problems/minimum-path-sum/)

> 给定一个包含非负整数的 m x n 网格，每次只能向下或者向右移动一步，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

1. 定义数组元素的含义：我们把网格看作一个二维数组，起点坐标为 (0,0)，右下角坐标为 (m-1)(n-1)，当从左上角走到 (i,j) 这个位置时，最小路径和为 `dp[i][j]`，所以我们的目标是求 `dp[m-1][n-1]`。
2. 找出数组元素之间的关系式：达到 (i,j) 这个位置有两种方式：一种是从 (i-1,j) 这个位置到达，一种是从 (i,j-1) 这个位置到达，所以最小路径和 `dp[i][j] = Math.min(dp[i - 1][j] + arr[i][j], dp[i][j - 1] + arr[i][j])`。
3. 找出初始值：可以易知，当 i=0 或者 j=0 时，只能往右或者往下，方式只有一种，最小路径和也只有一种。  
    

得出求解代码：

```
var minPathSum = function (grid) {
  const m = grid.length,
    n = grid[0].length

  if (m <= 0 || n <= 0) {
    return 0
  }

  const dp = new Array(m).fill(0).map((_) => new Array(n).fill(0))

  dp[0][0] = grid[0][0]

  // 最左边的列
  for (let i = 1; i < m; i++) {
    dp[i][0] = dp[i - 1][0] + grid[i][0]
  }
  // 最上面的行
  for (let j = 1; j < n; j++) {
    dp[0][j] = dp[0][j - 1] + grid[0][j]
  }

  for (let i = 1; i < m; i++) {
    for (let j = 1; j < n; j++) {
      dp[i][j] = Math.min(dp[i - 1][j] + grid[i][j], dp[i][j - 1] + grid[i][j])
    }
  }

  return dp[m - 1][n - 1]
}

const grid = [
  [1, 3, 1],
  [1, 5, 1],
  [4, 2, 1],
]

console.log(minPathSum(grid)) // 7  路径 1→3→1→1→1 的总和最小
```

## 编辑距离

题目来源：[编辑距离](https://leetcode.cn/problems/edit-distance/)

> 给你两个单词 word1 和 word2，请返回将 word1 转换成 word2 所使用的最少操作数。你可以对一个单词进行如下三种操作：
> 
> 插入一个字符
> 
> 删除一个字符
> 
> 替换一个字符

1. 定义数组元素的含义：当字符串 word1 的长度为 i，字符串 word2 的长度为 j 时，将 word1 转化为 word2 所使用的最少操作次数为 `dp[i][j]`。
2. 找出数组元素之间的关系式：大部分情况下，`dp[i][j]` 和 `dp[i-1][j]`、d`p[i][j-1]`、`dp[i-1][j-1]` 肯定存在某种关系。这题中，我们找出其中关系如下：
3. 如果 word1\[i\] 与 word2\[j\] 相等，这个时候不需要进行任何操作，显然有 `dp[i][j] = dp[i-1][j-1]`。
4. 如果 word1\[i\] 与 word2\[j\] 不相等，则有三种操作，我们需要找出这三种操作的最小值，则 `dp[i][j] = min(dp[i-1][j-1],dp[i][j-1],dp[[i-1][j]]) + 1`：  
    1. 如果把字符 word1\[i\] 替换成与 word2\[j\] 后相等，则有 `dp[i][j] = dp[i-1][j-1] + 1`
    2. 如果在字符串 word1 末尾插入一个与 word2\[j\] 相等的字符，则有 `dp[i][j] = dp[i][j-1] + 1`
    3. 如果把字符 word1\[i\] 删除，则有 `dp[i][j] = dp[i-1][j] + 1`
5. 找出初始值：当 `dp[i][j]` 中，如果 i 或者 j 有一个为 0，i-1 或 j-1 则为负数，所以初始值需要先计算出所有 i 或 j 为 0 的情况。  
    

```
var minDistance = function (word1, word2) {
  const m = word1.length,
    n = word2.length
  const dp = new Array(m).fill(0).map((_) => new Array(n).fill(0))
  dp[0][0] = 0

  for (let i = 1; i <= m; i++) {
    dp[i][0] = dp[i - 1][0] + 1
  }

  for (let j = 1; j <= n; j++) {
    dp[0][j] = dp[0][j - 1] + 1
  }

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      // 注意第 i 个字符的下标是 i-1
      if (word1[i - 1] === word2[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1]
      } else {
        dp[i][j] = Math.min(dp[i - 1][j - 1], dp[i][j - 1], dp[i - 1][j]) + 1
      }
    }
  }
  return dp[m][n]
}

console.log(minDistance('horse', 'ros')) // 3
```
