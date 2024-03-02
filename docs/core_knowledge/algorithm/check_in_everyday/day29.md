---
comments: true
---

## [860.柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

局部最优：遇到账单20，优先消耗美元10，完成本次找零。全局最优：完成全部账单的找零
```cpp linenums="1"
bool lemonadeChange(vector<int>& bills) {
    int five = 0, ten = 0;
    for(auto i : bills) {
        if(i == 5) five ++;
        else if(i == 10) ten ++, five --;
        else if(ten > 0) ten --, five --;
        else five -= 3;
        if(five < 0) return false;
    }
    return true;
}
```

## [406.根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

身高一定是从大到小排（身高相同的话则k小的站前面），让高个子在前面 ([参考图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201216201851982.png))

- 局部最优：优先按身高高的people的k来插入。插入操作过后的people满足队列属性
- 全局最优：最后都做完插入操作，整个队列满足题目队列属性

过程如下, 初始: `people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]`

排序完的people：`[ [7,0], [7,1], [6,1], [5,0], [5,2]，[4,4] ]`

- 插入[7,0]：`[[7,0]]`
- 插入[7,1]：`[[7,0],[7,1]]`
- 插入[6,1]：`[[7,0],[6,1],[7,1]]`
- 插入[5,0]：`[[5,0],[7,0],[6,1],[7,1]]`
- 插入[5,2]：`[[5,0],[7,0],[5,2],[6,1],[7,1]]`
- 插入[4,4]：`[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]`

```cpp linenums="1"
static bool cmp(vector<int>& a, vector<int>& b) {
    if(a[0] == b[0]) return a[1] < b[1];
    return a[0] > b[0];
}
vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    sort(people.begin(), people.end(), cmp);
    vector<vector<int>> res;
    for(int i = 0; i < people.size(); i ++) {
        int pos = people[i][1];
        res.insert(res.begin() + pos, people[i]); // insert插入很费时
    }
    return res;
}
```
C++中`vector`（可以理解是一个动态数组，底层是普通数组实现的）若插入元素大于预先普通数组大小，vector底部会有一个扩容的操作，即申请两倍于原先普通数组的大小，然后把数据拷贝到另一个更大的数组上 ([如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201218185902217.png))

## [452.用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

**重叠区间题目** , 只射重叠最多的气球，用的弓箭一定最少

局部最优：当气球出现重叠，一起射，所用弓箭最少。全局最优：把所有气球射爆所用弓箭最少

- 为了让气球尽可能的重叠，需要对数组进行排序(按照起始或终止都可)
- 如果气球重叠了，重叠气球中右边边界的最小值之前的区间一定需要一个弓箭

示例:`[[10,16],[2,8],[1,6],[7,12]]` ([如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201123101929791.png))
```cpp linenums="1"
static bool cmp(vector<int>& a, vector<int>& b) { return a[0] < b[0]; }
int findMinArrowShots(vector<vector<int>>& points) {
    if(points.size() == 0) return 0;
    sort(points.begin(), points.end(), cmp);
    int cnt = 1; // 不为空,至少需要一个
    for(int i = 1; i < points.size(); i ++) {
        // 气球i和i - 1不挨着
        if(points[i][0] > points[i - 1][1]) cnt ++;
        // i和i-1挨着 
        else points[i][1] = min(points[i][1], points[i - 1][1]);
    }
    return cnt;
}
```