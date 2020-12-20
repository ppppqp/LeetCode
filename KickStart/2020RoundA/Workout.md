# 2020 RoundA Plates
[Problem Description](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ffc7/00000000001d3f5b)
### Problem 
>Tambourine has prepared a fitness program so that she can become more fit! The program is made of $N$ sessions. During the $i$ session, Tambourine will exercise for $Mi_{i}$ minutes. The number of minutes she exercises in each session are strictly increasing.\
The difficulty of her fitness program is equal to the ***maximum difference in the number of minutes between any two consecutive training sessions.*** \
To make her program less difficult, Tambourine has decided to add up to $K$ additional training sessions to her fitness program. She can **add these sessions anywhere in her fitness program, and exercise any positive integer number of minutes in each of them.** After the additional training session are added, the number of minutes she exercises in each session must still be strictly increasing. What is the minimum difficulty possible?

### input
>  First line: #of test cases T.\
>  Each test case begins with a line containing N, K. \
>  The second line contains N integers, the i-th of these is Mi, the number of minutes she will exercise in the i-th session.

### output
>For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is they is the minimum difficulty possible after up to K additional training sessions are added.

## Basics
    考虑简单情况：要在一串递增的数字中，插入一个数，是的相邻数字之间的差最小，要插入哪个数字？
  * 解法： 遍历一遍，找到相邻最大的两个数，取平均，就是我们要插入的数
## Constraints
  如果要放入好多数，则不能每次都使用这种方法。比如 $1, 2, 10, 100$ 的数列，插入两个数，一定是将$10-100$的区间三分，比先二分再二分要好。
## Implementation
  还是贪心算法，但思路不同。
  * 为达到所有相邻数字间的差值$d < d_{x}$，对应有某一个最小的插值数量$k$。而相邻数字间的差值范围就是不插值情况下的最大差到$1$（不可为零）。 
  * 遍历所有可能的$d_{x}\in [1, max(v_{i}-v_{i-1})]$
    * 对于某个区间$[i-1, i]$, 将其分割到差值 $< d_{x}$ 的最小分割数是$ceil(\dfrac{v_{i} - v_{i-1}}{d_{x}}) -1$
    * 将所有区间需要的最小分割数加起来就是总的分割数
  * 得到每个差值对于的分割数后，只需要找到分割数$\leq K$的对应最小$d_{K}$即可
  * 因为K和d成反比关系（分的越多，差值越小），使用二分查找更快（类似`lowerBound()`）
___

## Basics
Simple Situation: In an increasing sequence, insert a number to minimize the maximum difference between adjacent numbers. Which number to insert?  
  * Solution: Iterate to find the pair that makes maximum difference, take the average.
## Constraints
  If we can insert many nubmers, it doesn't work by repeating the method above. For example,
   $1, 2, 10, 100$, the optimal is to triplize the $10-100$ rather than halves.
## Implementation
  Still greedy, but a little different. 
  * To reach the maximum difference $d < d_{x}$，there's a corresponding minimum insertion we have to make $k$。The difference is greater than $1$（Not zero).
  * Iterate through all the possible $d_{x}\in [1, max(v_{i}-v_{i-1})]$
    * For some interval $[i-1, i]$, the minimum insertion number to  split it till the difference $< d_{x}$ is $ceil(\dfrac{v_{i} - v_{i-1}}{d_{x}}) -1$
    * The sum of the insertion number for all intervals is the insertion needed.
  * With all the insertion numbers for any difference, we just need to find $\leq K$'s corresponding (least) $d_{K}$
  * As K is negatively related to d(more splits, less difference), we can use binary search（like `lowerBound()`）

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