---
comments: true
---

## [435.无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)

按照右边界排序，从左向右记录非交叉区间的个数。最后用 ^^区间总数-非交叉区间的个数 = 需要移除的区间的个数^^, [参考图](https://code-thinking-1253855093.file.myqcloud.com/pics/20230201164134.png)
```cpp linenums="1"
static bool cmp(vector<int>& a, vector<int>& b) { return a[1] < b[1]; }
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if(intervals.size() == 0) return 0;
    sort(intervals.begin(), intervals.end(), cmp);
    int cnt = 1, end = intervals[0][1];
    for(int i = 1; i < intervals.size(); i ++)
        if(end <= intervals[i][0]) end = intervals[i][1], cnt ++;
    return intervals.size() - cnt;
}
```

按照左区间排序,仿照[452题](./day29.md/#452) 
```cpp linenums="1"
static bool cmp(vector<int>& a, vector<int>& b) { return a[0] < b[0]; }
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if(intervals.size() == 0) return 0;
    sort(intervals.begin(), intervals.end(), cmp);
    int cnt = 1;
    for(int i = 1; i < intervals.size(); i ++) {
        if(intervals[i][0] >= intervals[i - 1][1]) cnt ++;
        else 
            intervals[i][1] = min(intervals[i - 1][1], intervals[i][1]);
    }
    return intervals.size() - cnt;
}
```

## [763.划分字母区间](https://leetcode.cn/problems/partition-labels/)

在遍历的过程中相当于是要找每一个字母的边界，如果找到之前遍历过的所有字母的最远边界，说明这个边界就是分割点了。此时前面出现过所有字母，最远也就到这个边界了, [参考图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201222191924417.png)

- 统计每一个字符最后出现的位置
- 遍历字符,更新字符的最远出现下标，若最远出现位置下标=当前下标,则找到分割点

```cpp linenums="1"
vector<int> partitionLabels(string s) {
    int h[27] = { 0 }; // i为字符，hash[i]为字符出现的最后位置
    // 统计每一个字符最后出现的位置
    for(int i = 0; i < s.size(); i ++) h[s[i] - 'a'] = i;
    vector<int> res;
    int l = 0, r = 0;
    for(int i = 0; i < s.size(); i ++) {
        r = max(r, h[s[i] - 'a']); // 找到字符出现的最远边界
        if(i == r) {
            res.push_back(r - l + 1);
            l = i + 1;
        }
    }
    return res;
}
```

## [56.合并区间](https://leetcode.cn/problems/merge-intervals/)

同理452题和435题, 排序 --> 若`intervals[i][0] <= intervals[i - 1][1]` 即intervals[i]的左边界 <= intervals[i - 1]的右边界，则一定有重叠, [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201223200632791.png)
```cpp linenums="1" hl_lines="9"
static bool cmp(vector<int>& a, vector<int>& b) { return a[0] < b[0]; }
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    if(intervals.size() <= 1) return intervals;
    sort(intervals.begin(), intervals.end(), cmp);
    vector<vector<int>> res;
    //第一个区间就可以放进结果集里，后面若重叠，在res上直接合并
    res.push_back(intervals[0]);
    for(int i = 1; i < intervals.size(); i ++) {
        if(intervals[i][0] <= res.back()[1]) // 发现重叠区间
    // 合并区间，只更新右边界即可，因res.back()的左边界一定是最小值
            res.back()[1] = max(res.back()[1], intervals[i][1]);
        else res.push_back(intervals[i]);
    }
    return res;
}
```