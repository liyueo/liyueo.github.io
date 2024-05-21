---
layout:     post
title:      差分数组
subtitle:   差分数组-20240409
date:       2024-05-21
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - diff
    - array
---

## 差分数组

**适用于频繁的对某个区间进行统一加几**

差分数组就是 diff[i] 就是 nums[i] 和 nums[i-1] 之差：

使用差分数组计算原来的数组 只需要前缀和就行

比如： 

![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/3a63655e-d551-4d72-9627-8ae238cf4b97)

diff[i] + 3   等同于对原数组i之后的所有数加了3.。

若要对某个区间[i, j] + 3  只需要 diff[i] + 3,, diff[j + 1]-1即可

使用场景：
leetcode1589题：1589. 所有排列中的最大和

要统计每个索引出现的次数 requests = [[0,2],[1,3],[1,1]]
只需要初始化一个数组，，，然后 将这个数组看成一个差分数组，，对第一个数++，，第二个数的下一个位置--。，，然后计算前缀和 便是 每个索引出现的次数
