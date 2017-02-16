---
title: POJ1163动态规划
date: 2016-04-09 18:17:21
tags:
- 动态规划
categories:
- 算法
---

[TopCoder文章:Dynamic Programming: From novice to advanced](http://www.hawstein.com/posts/dp-novice-to-advanced.html)

# The Triangle

Time Limit: 1000MS      Memory Limit: 10000K
Total Submissions: 43089        Accepted: 26054
----
## Description

　　　　7
　　　3　　8
　　8　　1　　0
　2　　7　　4　　4
4　　5　　2　　6　　5
　　(Figure 1)
Figure 1 shows a number triangle. Write a program that calculates the highest sum of numbers passed on a route that starts at the top and ends somewhere on the base. Each step can go either diagonally down to the left or diagonally down to the right. 

-----
## Input

Your program is to read from standard input. The first line contains one integer N: the number of rows in the triangle. The following N lines describe the data of the triangle. The number of rows in the triangle is > 1 but <= 100. The numbers in the triangle, all integers, are between 0 and 99.
Output

Your program is to write to standard output. The highest sum is written as an integer.

---
## Sample Input

5
7
3 8
8 1 0 
2 7 4 4
4 5 2 6 5

---
## Sample Output
30

---
## 分析
简单动态规划，这道题如果用枚举法，在数塔层数稍大的情况下，则需要列举出的路径条数将是一个非常庞大的数目，最终会导致TLE。
因此我们可以从下往上推，相邻的两个数中找较大的与上层相加，得出的结果相邻的两个数中再找较大的与上层相加，以此类推。
<!--more-->
---
## Java代码
```java
import java.util.Scanner;

/**
 * Created by LWQ on 2016/4/9.
 */
public class OneOneSixThree {
    public static void main(String[] args) {
        int row, i, j;
        int[][] dp = new int[101][101];
        Scanner sc = new Scanner(System.in);
        row = sc.nextInt();
        for (i = 1; i <= row; i++) {
            for (j = 0; j < i; j++) {
                dp[i][j] = sc.nextInt();
            }
        }
        for (i = row - 1; i >= 1; i--) {
            for (j = 0; j < i; j++) {
                dp[i][j] += Math.max(dp[i + 1][j], dp[i + 1][j + 1]);
            }

        }
        System.out.println(dp[1][0]);

    }
}

```
