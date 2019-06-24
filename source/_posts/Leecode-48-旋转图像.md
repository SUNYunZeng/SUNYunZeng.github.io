---
title: Leecode 48.旋转图像
comments: true
toc: true
date: 2019-06-24 11:47:24
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

**示例1：**
```java
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**示例2：**
```java
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

# 思路

如果可以拷贝矩阵，则可以每一行与每一列同时旋转。

但是要求在原矩阵中操作，所以需要每一个元素进行位置旋转变换。

一个4*4的矩阵如下图所示：

{% asset_img rotate.png %}

顺时针旋转即相同颜色的元素进行依次替换。

**对于四阶矩阵，先从最外圈i=0开始，到里圈i=1结束。n+1阶矩阵从最外圈i=0开始，i=n/2里圈结束。**

**每j圈依次从j行第i个元素开始，n-i个元素结束。**

**四个元素互换条件：**

    tmp = matrix[j][n-i];
    matrix[j][n-i] = matrix[i][j];
    matrix[i][j] = matrix[n-j][i];
    matrix[n-j][i] = matrix[n-i][n-j];
    matrix[n-i][n-j] = tmp;

# 代码

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length-1;
        int tmp;
        // 循环几圈
        for(int i=0; i<=n/2;i++){
            //  每一圈要交换几轮
            for(int j=i;j<n-i;j++){
                // 四个元素依次交换
                tmp = matrix[j][n-i];
                matrix[j][n-i] = matrix[i][j];
                matrix[i][j] = matrix[n-j][i];
                matrix[n-j][i] = matrix[n-i][n-j];
                matrix[n-i][n-j] = tmp;
            }
        }
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-image
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
