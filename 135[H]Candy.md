# 135 Candy
>There are N children standing in a line. Each child is assigned a rating value.
You are giving candies to these children subjected to the following requirements:\
***Each child must have at least one candy.***\
***Children with a higher rating get more candies than their neighbors.***\
 What is the minimum candies you must give?


[Problem Description](https://leetcode-cn.com/problems/candy/)


## 1. Idea
* 方法1: 两重遍历 

    考虑两个方向，从左往右，需要满足右边的孩子如果比左边的分高，则需要比左边的孩子多，如果比左边的分低，则可随便设置（设为1）。 从右往左，需要满足左边的孩子如果比右边的分高，则需要比右边的多，否则可以随便设置。使用从左到右一遍遍历，从右到左一遍遍历，两重遍历取$max$，即为满足综合的最小值。
* 方法2: 一边遍历贪心
    能少给就尽量少给，和上一种方法从左到右的一遍遍历一样，但这次考虑递减过程。
    * 递减开始的时候，给上最小值 $1$
    * 如果仍然递减，则为 $2, 1$
    * 以此类推，知道开始递增，假设递减序列持续了$i$项，此序列对应的糖果数为
      $i, i-1, i-2, ..., 2,1$
## 3. Implementation
* 方法一：
  * 第一遍遍历使用一个数组candy记录满足左条件的最小candy数
  * 第二遍遍历时同时将第二遍遍历结果和candy数组中的元素比较的最优可行解（max）

## 4. Analysis
* 方法1: 
  * 时间$O(n)$
  * 空间$O(n)$
* 方法2: 
  * 时间$O(n)$
  * 空间$O(1)$

___
## 1. Idea
* Method1: Traverse Twice 
    Consider two directions: left to right, we need to satisfy that if the child in the right has higer score than the left one, the right one has to have more candies. Otherwise, just one candy would satisfy. Similarly, for the right to left, if the child in the left has higer score than the right one, the left one has to have more candies. Otherwise, just one candy would satisfy. We can traverse first from left to right to find the **minimal candies we need to satisfy the left side condition**, and then traverse again to satisfy the right side condition. We take the $max$ of the result of both sides.

* Method2: One Traverse Greedy
    We give less if we can. Just like the first traverse of method1, we try to satisfy the left side condition. But this time we use greedy to satisfy the right side condition as well. 
    * At the start of the decreasing, we give the least value $1$
    * If it continue to decrease by one term, it should be $2,1$
    * If it starts to increase at the $i^{th}$ term, the sequence should be       $i, i-1, i-2, ..., 2,1$

## 3. Implementation
* Method1：
  * At the first traverse, use a vector *candy* to record the minimal candy we need to satisfy the left side condition.第一遍遍历使用一个数组candy记录满足左条件的最小candy数
  * At the second traverse, compare the result of first traversal in *candy* with the second result simulatneously and get the optimal feasible solution（max）

## 4. Analysis
* Method1: 
  * Time$O(n)$
  * Space$O(n)$
* Method2: 
  * Time$O(n)$
  * Space$O(1)$
___

```c++
//method 1
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candy(ratings.size(), 0);
        int right = 1, sum = 0;
        candy[0] = 1;
        for(int i = 1; i < candy.size(); i++){
            if(ratings[i] > ratings[i-1]){
               candy[i] = candy[i-1]+1;
            }
            else candy[i] = candy[i-1];
        } 
        for(int i = candy.size()-1; i >= 0; i--){
            if(i < candy.size()- 1 && ratings[i] > ratings[i+1]){
                right++;
            }
            sum += max(right, candy[i]);
            cout << max(right, candy[i]) << ' ';
        }
        return sum;
    }
};
```