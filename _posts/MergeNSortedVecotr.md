---
layout:     post
title:      合并N个有序链表
subtitle:   合并N个有序有序链表-20240417
date:       2024-04-17
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
  -
  -
---
## 合并N个有序链表

思路： 最小堆，， 先 push进每个数组的第一个元素，
然后 将头部push进res，，然后将 改元素所在数组的下一个元素push进
所以要用pair<int, int>保存一下 属于第几个子数组
所以要用仿函数手写排序策略
```c++
vector<vector<int>> arrays = {
            {1,3,5,7},
            {2, 4, 6},
            {0,8,9,10,11},
            {1,5,7,8}
    };
```

代码：
```c++
class quecom{
public:
    bool operator()(pair<int, int> x, pair<int, int> y){
        return x.first > y.first;
    }
};
class Solution {
public:
    vector<int> mergekSortedArrays(vector<vector<int>> &arrays) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, quecom> que;
        vector<int> index(arrays.size(), 0);
        vector<int> res;
        for(int i = 0; i < arrays.size(); i++){
            que.push(pair<int, int>{arrays[i][index[i]++], i});
        }
        while(!que.empty()){
            pair<int, int> temp = que.top();
            que.pop();
            res.push_back(temp.first);
            if(index[temp.second] < arrays[temp.second].size())
                que.push(pair<int, int>{arrays[temp.second][index[temp.second]++], temp.second});
        }
        return res;
    }
};

int main(){
    vector<vector<int>> arrays = {
            {1,3,5,7},
            {2, 4, 6},
            {0,8,9,10,11},
            {1,5,7,8}
    };
    Solution s;
    vector<int> res = s.mergekSortedArrays(arrays);
    for(auto i : res){
        cout<<i<<" ";
    }
    return 0;
}
```
