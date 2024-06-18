---
comments: true
---

## 理论概述

> 回溯法又称为回溯搜索法，它是一种搜索的方式, **本质是穷举**

- 回溯是递归的的副产品, 只要有递归就有回溯

**解决的问题:**

1. 组合问题: N个数里面按一定的规则找出k个数的集合
2. 切割问题: 一个字符串按一定规则有几种切割方式
3. 子集问题: N个数的集合有几个符合条件的子集
4. 排列问题: N个数按一定规则全排列,有几种方式
5. 棋盘问题: N皇后, 解数独等... 

回溯算法解决的问题都可以抽象为树形结构, 一般在集合中递归搜索，集合的大小构成了树的宽度，递归的深度构成的树的深度。--> [图片参考](https://code-thinking-1253855093.file.myqcloud.com/pics/20210130173631174.png)

伪代码模板参考: 
```c++ linenums="1"
void backtracking(params..) {
    if() {
        // 存结果
        return;
    }
    for(选择：本层集合中元素[树中节点孩子的数量就是集合的大小]) {
        // 处理节点
        // 递归 --> 调用backtracking(路径, 选择列表);
        // 回溯, 撤销处理结果
    }
}
```

## [77.组合](https://leetcode.cn/problems/combinations/)

> 给定两个整数 n 和 k，返回范围 `[1, n]` 中所有可能的 k 个数的组合

暴力的话,k是几就几层循环, 但代码如何实现? --> 利用回溯抽象成树形结构, [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201123195223940.png)



```cpp linenums="1"
vector<vector<int>> res;
vector<int> path;
// idx记录本层递归的中，集合从哪里开始遍历
void backtracking(int n, int k, int idx) {
    if(path.size() == k) {
        res.push_back(path);
        return;
    }
    for(int i = idx; i <= n; i ++) {
        path.push_back(i);
        backtracking(n, k, i + 1); // 递归
        path.pop_back(); // 回溯,撤销处理的节点
    }
}
vector<vector<int>> combine(int n, int k) {
    backtracking(n, k, 1);
    return res;
}
```