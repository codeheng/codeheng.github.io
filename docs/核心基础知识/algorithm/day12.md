---
comments: true
---

## [102.层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

需要借助队列 --> [动图参考](https://code-thinking.cdn.bcebos.com/gifs/102%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.gif)
```cpp linenums="1"
vector<vector<int>> levelOrder(TreeNode* root) {
    queue<TreeNode*> q;
    vector<vector<int>> res;
    if (root != nullptr) q.push(root);
    while(!q.empty()) {
        int size = q.size();
        vector<int> vec;
        while(size --) {
            TreeNode* node = q.front(); q.pop();
            vec.push_back(node->val);
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        res.push_back(vec);
    }
    return res;
}
```

!!! Success "GO ON"
    利用层序遍历解决以下问题!

### [107.二叉树的层序遍历II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

> 返回其节点值自底向上的层次遍历

```cpp linenums="1" hl_lines="16"
vector<vector<int>> levelOrderBottom(TreeNode* root) {
    queue<TreeNode*> q;
    vector<vector<int>> res;
    if(root) q.push(root);
    while(!q.empty()) {
        int size = q.size();
        vector<int> vec;
        while(size --) {
            TreeNode* node = q.front(); q.pop();
            vec.push_back(node->val);
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        res.push_back(vec);
    }
    reverse(res.begin(), res.end());
    return res;
}
```

### [199.二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

```cpp linenums="1" hl_lines="9"
vector<int> rightSideView(TreeNode* root) {
    vector<int> res;
    queue<TreeNode*> q;
    if(root) q.push(root);
    while(!q.empty()) {
        int size = q.size();
        while(size --) {
            TreeNode* node = q.front(); q.pop();
            if (size == 0) res.push_back(node->val);
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
    }
    return res;
}
```

### [637.二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

```cpp linenums="1" hl_lines="10"
vector<double> averageOfLevels(TreeNode* root) {
    vector<double> res;
    queue<TreeNode*> q;
    if(root) q.push(root);
    while(!q.empty()) {
        int size = q.size();
        double sum = 0;
        for(int i = 0; i < size; i ++) { // 不可以用while(size --) size会变成-1
            TreeNode* node = q.front(); q.pop();
            sum += node->val;
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        res.push_back( sum / size );
    }
    return res;
}
```

### [429.N叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

```cpp linenums="1" hl_lines="11 12"
vector<vector<int>> levelOrder(Node* root) {
    vector<vector<int>> res;
    queue<Node*> q;
    if(root) q.push(root);
    while(!q.empty()) {
        int size = q.size();
        vector<int> vec;
        while(size --) {
            Node* node = q.front(); q.pop();
            vec.push_back(node->val);
            for (auto i : node->children) //孩子入队
                if(i) q.push(i);
        }
        res.push_back(vec);
    }
    return res;
}
```

### [515.在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

```cpp linenums="1" hl_lines="9"
vector<int> largestValues(TreeNode* root) {
    vector<int> res;
    queue<TreeNode*> q;
    if(root) q.push(root);
    while(!q.empty()) {
        int size = q.size(), max = INT32_MIN; // 初始最大为一个小值
        while(size --) {
            TreeNode* node = q.front(); q.pop();
            max = (node->val > max ? node->val : max);
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        res.push_back(max);
    }
    return res;
}
```

### [116.填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

```cpp linenums="1"

```

### [117.填充每个节点的下一个右侧节点指针II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

```cpp linenums="1"

```

### [104.二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

递归 : 
```cpp linenums="1"
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    int maxL = maxDepth(root->left), maxR = maxDepth(root->right);
    return max(maxL, maxR) + 1;
}
```

层序遍历进行迭代 : 
```cpp linenums="1" hl_lines="8"
int maxDepth(TreeNode* root) {
    int depth = 0;
    queue<TreeNode*> q;
    if(!root) return 0;
    q.push(root);
    while(!q.empty()) {
        int size = q.size();
        depth ++;
        while( size -- ) {
            TreeNode* node = q.front(); q.pop();
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
    }
    return depth; 
}
```

### [111.二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

在上题基础上修改: 当左右孩子都空，即最低点的一层, 进行return
```cpp linenums="1" hl_lines="13"
int minDepth(TreeNode* root) {
    int depth = 0;
    queue<TreeNode*> q;
    if(!root) return 0;
    q.push(root);
    while(!q.empty()) {
        int size = q.size();
        depth ++;
        while(size --) {
            TreeNode * node = q.front(); q.pop();
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
            if(!node->left && !node->right) return depth;
        }
    }
    return depth;
}
```

## [226.翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

每个节点左右孩子进行交换即可
```cpp linenums="1"
TreeNode* invertTree(TreeNode* root) {
    if (root == nullptr) return root;
    swap(root->left, root->right);
    invertTree(root->left); invertTree(root->right);
    return root;
}
```

也可以利用层序
```cpp linenums="1"
TreeNode* invertTree(TreeNode* root) {
    queue<TreeNode*> q;
    if(root != nullptr) q.push(root);
    while(!q.empty()) {
        int size = q.size();
        while(size --) {
            TreeNode* node = q.front(); q.pop();
            swap(node->left, node->right);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
    return root;
}
```


## [101.对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

要比较内侧和外侧节点是否分别对应相等 --> [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210203144624414.png)

- 即左节点的左孩子与右节点的右孩子(外侧) & 左节点的右孩子与右节点的做孩子(内侧)

```cpp linenums="1"
bool compare(TreeNode* left, TreeNode* right) {
    if (!left || !right) return left == right;
    return (left->val == right->val) 
            && compare(left->left, right->right) 
            && compare(left->right, right->left);
}
bool isSymmetric(TreeNode* root) {
    if (!root) return true;
    return compare(root->left, root->right);
}
```