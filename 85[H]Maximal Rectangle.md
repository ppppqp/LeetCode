# 85. Maximal Rectangle
>Given a rows x cols binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

[Problem Description](https://leetcode-cn.com/problems/remove-duplicate-letters)

>![图片alt](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg "图片title")

>Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]\
>Output: 6\
>Explanation: The maximal rectangle is shown in the above picture.


## 1. Basics
  转化为[84.Largest Rectangle in Histogram](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)。
  这个问题是求在一个柱状图中最大的矩形。

## 2. Idea
对每行遍历，查看以该行为底的柱状图的最大矩形。
* 查看以该行为底的柱状图中的最大矩形的方法：


  单调（递增）栈，栈内储存序列中的序号，使得栈中的序号所对应的柱状图的高度从底至上是递增的。这样的意义在于：
    * 如果加入的元素比栈顶元素还要大，则这个元素对我的最大矩形面积没有高度贡献（最小矩形高度才是决定因素），只有宽度贡献（所以要如入栈）
    * 如果加入的元素比栈定元素小，说明***以栈定元素为高度的矩形宽度就到此为止了，不会再宽了（再宽高度会减小）***。所以，可以求出以该高度为高度的矩形的最大面积：在栈中下面一个元素的值一定比这个元素小（递增性），而为了维护这个递减性，在这两个序号之间的但没有存在于这个栈中的元素，值一定比该元素大（否则会留在栈中）。所以看这两个栈中相邻元素的序号差值就可以直到该矩形的宽度。



## 3. Implementation
通过维护一个单调栈。 如果一个栈是单调的，则只需要在加入元素的时候判断是否和顶部元素相比会破坏单调性。
* 若破坏，则将顶部元素pop，并计算以顶部元素对应高度为高度的矩形的面积，更新ans，继续判断
* 若不破坏，则push该元素，退出循环
* 若栈为空，退出循环

## 4. Constraints
* 为了使得最后栈能顺利清空，需要在原数组两端加上$0$（试想如果数列是$1,2,3,4,5$，则这五个序号push进去，循环退出了，没有东西触发他们的矩形面积计算）

___
## 1. Basics
 Convert to [84.Largest Rectangle in Histogram](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)。
  This problem is asking for the maximal rectangle in a histogram.

## 2. Idea
* Iterate through each line to see the largest rectangle in the histogram, which consists of the rectangles that "grows" from this line. 
![alt](https://pic.leetcode-cn.com/aabb1b287134cf950aa80526806ef4025e3920d57d237c0369ed34fae83e2690-image.png)

* The method of computing the largest rectangle in the histogram:
  Use a monotone (increasing) stack. Store the index in the stack so that the corresponding height increases from the bottom to the top. The meanings are:
    * If the element to push has greater height than the top, the element has no contribution to the height of the maximum rectangle because all elements below it have less height and therefore are more critical. 
    * If the element to push has less height than the top, ***the rectangle that has the height as the top element can not increase in width anymore(if the width increase, the height will decrease)***. Therefore, we can calculate the largest rectangle that has the height as the top element: the next element under this one in the stack must has less height(by monotone property), and the indeices between the two element but not in the stack must have greater height(Otherwise they will be left in the stack). The difference between the two adjacent element is the width of the largest rectangle.




## 3. Implementation
Maintain a monotone stack. We only need to maintain it by checking whether the element to add will conflict with the stack top.
* if it will, we pop the top, compute the maximal rectangle area that has the height as the top element's height, update the answer and continue
* if it won't , we push the element and break
* if the stack is empty, break


## 4. Constraints
* Inorder to clean the stack, we need to add $0$ at the start and the end of the array. Imagine if the array if $1,2,3,4,5$, pushing the five indices won't tirgger anything to compute their largest rectangle.



___

```c++
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        if(matrix.size()==0)return 0;
        vector<vector<int>>map(matrix.size(), vector<int>(matrix[0].size()+2, 0));
        for(int i = 0; i < matrix.size(); i++){
            for(int j = 1; j <= matrix[0].size(); j++){
                if(i == 0) map[i][j] = matrix[0][j-1]-'0';
                else{
                    if(matrix[i][j-1] == '0') map[i][j] = 0;
                    else map[i][j] = map[i-1][j]+1;
                }
            }

        }
        int ans = 0;
        for(int i = 0; i < map.size(); i++){
            stack<int>s;
            for(int j = 0; j < map[0].size(); j++){
                while(!s.empty() && map[i][j] < map[i][s.top()]){
                    int temp = s.top();
                    s.pop();
                    int area = (j - s.top()-1)*map[i][temp];
                    ans = max(ans, area);
                }
                s.push(j);
            }
        }
        return ans;
    }
};
```