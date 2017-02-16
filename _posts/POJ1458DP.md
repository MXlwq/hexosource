---
title: POJ1458动态规划
date: 2016-04-09 22:21:50
tags:
- 动态规划
categories:
- 算法
---
# Common Subsequence
Time Limit: 1000MS		Memory Limit: 10000K
Total Submissions: 46006		Accepted: 18845

----
## Description
A subsequence of a given sequence is the given sequence with some elements (possible none) left out. Given a sequence X = < x1, x2, ..., xm > another sequence Z = < z1, z2, ..., zk > is a subsequence of X if there exists a strictly increasing sequence < i1, i2, ..., ik > of indices of X such that for all j = 1,2,...,k, xij = zj. For example, Z = < a, b, f, c > is a subsequence of X = < a, b, c, f, b, c > with index sequence < 1, 2, 4, 6 >. Given two sequences X and Y the problem is to find the length of the maximum-length common subsequence of X and Y.

----
## Input
The program input is from the std input. Each data set in the input contains two strings representing the given sequences. The sequences are separated by any number of white spaces. The input data are correct.

----
## Output
For each set of data the program prints on the standard output the length of the maximum-length common subsequence from the beginning of a separate line.

----
## Sample Input

abcfbc         abfcab
programming    contest 
abcd           mnp
----

## Sample Output

4
2
0
----

---
## 分析

最长公共子序列，题目意思是给出两个字符串，求他们的最长公共子序列
LCS的入门题 模板

<!--more-->
---
## Java代码
```java
import java.util.Scanner;

/**
 * Created by LWQ on 2016/4/9.
 */
public class OneFourFiveEight {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (true) {
            ///dp[i][j]表示s1中第i个字符、s2中第j个字符之前的最长公共子序列长度(字符从1开始算)
            int[][] dp = new int[500][500];
            String s1 = sc.next();
            int length1 = s1.length();
            String s2 = sc.next();
            int length2 = s2.length();
            for (int i = 1; i <= length1; i++) {
                for (int j = 1; j <= length2; j++) {
                    if (s1.charAt(i - 1) == s2.charAt(j - 1))
                        dp[i][j] = dp[i - 1][j - 1] + 1;
                    else
                        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
            System.out.println(dp[length1][length2]);
        }
    }
}


```