---
title: Java 代码与优化专题
date: 2024-12-26
categories: [Java, Optimization]
tags: [Java, Code, Optimization]
layout: post
---

我写的  
```java
import static java.lang.Math.*;

class Solution {
    public long smallestNumber(long num) {
        if(num == 0 ) return 0;
        List<Integer> arr = new ArrayList<Integer>();
        long num_temp = num;
        while(num_temp != 0) {
            int tmp = (int)(num_temp % 10);
            arr.add(abs(tmp));
            num_temp /= 10;
        }
        Collections.sort(arr);
        int zero_num = 0;

        for(int i = 0; i < arr.size(); i++) {
            if(arr.get(i) == 0)zero_num++;
            else break;
        }
        long res = 0;
        if(num > 0) {
            res = (long)arr.get(zero_num);
            res *= pow(10, zero_num);
            for(int i = zero_num + 1; i < arr.size(); i++) {
                res = res * 10 + arr.get(i);
            }
        } else {
            for(int i = arr.size() - 1; i >= 0 ; i--) {
                res = res * 10 + arr.get(i);
            }
            res = -res;
        }
        return res;
    }
}
```
优化之后的  

```java
import java.util.Arrays;

class Solution {
    public long smallestNumber(long num) {
        if (num == 0) return 0;

        char[] digits = Long.toString(Math.abs(num)).toCharArray();
        Arrays.sort(digits);

        if (num > 0) {
            // 对于正数，找到第一个非零数字作为起始数字
            int firstNonZeroIndex = 0;
            while (digits[firstNonZeroIndex] == '0') {
                firstNonZeroIndex++;
            }
            // 将第一个非零数字放在最前面
            char temp = digits[0];
            digits[0] = digits[firstNonZeroIndex];
            digits[firstNonZeroIndex] = temp;

            return Long.parseLong(new String(digits));
        } else {
            // 对于负数，将数字倒序排列并转成负数
            StringBuilder sb = new StringBuilder(new String(digits));
            sb.reverse();
            return -Long.parseLong(sb.toString());
        }
    }
}

```
