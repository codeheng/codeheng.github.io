---
comments: true
---

## [239.滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

> 利用 **单调队列** 即队列中元素单调递减/递增(利用`deque`实现) -->[动图参考](https://code-thinking.cdn.bcebos.com/gifs/239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.gif)

```c++ linenums="1"
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> res;
    deque<int> dq; // 双端队列
    int i = 0, j = 0;
    while( j < nums.size() ) {
        while(!dq.empty() && nums[j] > dq.back()) dq.pop_back();
        if (j - i + 1 < k) j ++;
        else if (j - i + 1 == k) {
            res.push_back(dq.front());
            if (dq.front() == nums[i]) dq.pop_front();
            i ++, j ++;
        }
    }
    return res;
}
```

## [347.前K个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

利用小根堆,每次将最小的弹出,剩下的就是前k个最大的

- C++中`priority_queue`(优先队列)即是堆,只能队头取元素,队尾添元素(默认大根队)
```cpp linenums="1"
vector<int> topKFrequent(vector<int>& nums, int k) {
    vector<int> res;
    unordered_map<int, int> map; //记录<nums[i],对应次数>
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    // greater代表从小到大插入元素
    for(auto i : nums) map[i] ++;
    for (auto i : map) {
        pq.push({i.second, i.first});
        if (pq.size() > k) pq.pop();
    }
    while (k --) {
        res.push_back(pq.top().second);
        pq.pop();
    }
    return res;
}
```