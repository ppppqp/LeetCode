# 2020 RoundA Plates
[Problem Description](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ffc7/00000000001d40bb)
### Problem 
>Dr. Patel has N stacks of plates. Each stack contains K plates. Each plate has a positive beauty value, describing how beautiful it looks.\
Dr. Patel would like to take **exactly P** plates to use for dinner tonight. **If he would like to take a plate in a stack, he must also take all of the plates above it in that stack as well.**\
Help Dr. Patel pick the P plates that would maximize the total sum of beauty values.

### input
> First line: #of test cases T.\
>  Each test case begins with a line containing N, K and P. \
> N lines follow. The i-th line contains K integers, describing the beauty values of each stack of plates from top to bottom.

### output
>For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is the maximum total sum of beauty values that Dr. Patel could pick.

## Basics
  联想到背包问题，共N个盘子，取恰好P个，使得beauty最大。对应是一个背包问题，N个物品，容量是P，价值是beauty。
## Constraints
  取一个盘子时，在上面的所有盘子必须同时取到。
## Implementation
  使用数组储存到第$i$叠盘子前所能达到的最大beauty值，进行动态规划。

  $dp[i] = max(dp[i], \ dp[i-\Sigma_{num}]+ \Sigma_{V})$
  
  * $dp[i]$： 第$i$叠盘子前所能达到的最大beauty值
  * $\Sigma_{num}$: 取第j个盘子时，同时要增加多少个盘子（第j个盘子上面的所有盘子）
  * $\Sigma_{V}$:取第j个盘子时，同时要增加多少beauty

___

## Basics
  Think about the knapsack problem. We have N plates, and we want to take P of them to maximize the beauty. The corresponding  problem is N objects, P capacity, beauty as value.

## Constraints
  When you take a plate, you have to take all plates above it in its stack.
## Implementation
  Dynamic Programming.

  $dp[i] = max(dp[i], \ dp[i-\Sigma_{num}]+ \Sigma_{V})$
  
  * $dp[i]$：The best beauty we can have before the $i^{th}$ stack.
  * $\Sigma_{num}$: When taking the $j^{th}$ plates from the stack, the number of plates in all you have to take.
  * $\Sigma_{V}$: When taking the $j^{th}$ plates from the stack, the number of beauty in all you can have.

___

```c++
#include <cmath>
#include <iostream>
#include <vector>
using namespace std;
int solve() {
  int N, K, P;
  cin >> N >> K >> P;
  vector<vector<int> > value(N, vector<int>(K, 0));
  vector<int> dp(P + 1, 0);
  for (int i = 0; i < N; i++) {
    for (int j = 0; j < K; j++) {
      cin >> value[i][j];
      // cout << value[i][j] << ' ';
    }
  }
  for (int i = 0; i < N; i++) {
    for (int j = P; j >= 0; j--) {
      int sum = 0, sumV = 0;
      for (int k = 0; k < K; k++) {
        sum++;
        sumV += value[i][k];
        if (j >= sum) dp[j] = max(dp[j], dp[j - sum] + sumV);
      }
    }
  }

  return dp[P];
}
int main() {
  ios::sync_with_stdio(false);
  int T;
  cin >> T;
  for (int i = 0; i < T; i++) {
    cout << "Case #" << i + 1 << ": " << solve() << '\n';
  }
}
```