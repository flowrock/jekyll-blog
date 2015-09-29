---
layout: post
title: "Regular Expression Matching"
modified:
categories: 
excerpt: DP solution to LeetCode hard problem to implement basic RE.
tags: [java, leetcode, hard, dp]
date: 2015-09-28T15:58:06-04:00
comments: true
---

###Problem description
{% highlight text %}
Implement wildcard pattern matching with support for '?' and '*'.
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).
The function prototype should be:
bool isMatch(const char *s, const char *p)
Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
{% endhighlight %}

###Idea
The basic idea to solve this problem is to use dp. Basically, the most easy solution to come up with is using a two dimensional matrix to store dp result, the dimensions of the matrix would be the length of s and p plus 1. Initially, dp[0][0] is set to true. Note i starts from 0 while j starts from 1, this is because for the first iteration, when s is empty, it should also see if the second character of p is * or not, if is, then the first character of p is skipped and a the match between an empty s and a x* like p should be true. One observation is that when meets a *, if dp[i][j-2] is true, dp[i][j] should be true, because the * now means zero preceding. If not, then means * should mean more than zero preceding, or there is a mismatch. To check, see dp[i-1][j] is valid or not, if valid, check character s[i-1] and p[j-1] to see they are a match.

###Solution
{% highlight java %}
public static boolean isMatch(String s, String p) {
	int slen = s.length(), plen = p.length();
	boolean[][] dp = new boolean[slen+1][plen+1];
	for (int i=0; i<slen+1; i++)
		Arrays.fill(dp[i], false);
	dp[0][0] = true;
	for (int i=0; i<=slen; i++) {
		for (int j=1; j<=plen; j++) {
			if (p.charAt(j-1) == '*')
				dp[i][j] = dp[i][j-2] || 
				(i>0 && dp[i-1][j] && 
				(s.charAt(i-1)==p.charAt(j-2) || 
				p.charAt(j-2)=='.'));
			else 
				dp[i][j] = i>0 && dp[i-1][j-1] && 
				(s.charAt(i-1)==p.charAt(j-1) || 
				p.charAt(j-1)=='.');
		}
	}
	return dp[slen][plen];
}
{% endhighlight %}
