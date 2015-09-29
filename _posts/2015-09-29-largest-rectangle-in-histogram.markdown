---
layout: post
title: "Largest Rectangle in Histogram"
modified:
categories: 
excerpt: Solution to LeetCode hard problem using stack.
tags: [java, leetcode, hard, stack]
image:
  feature:
date: 2015-09-29T12:21:30-04:00
comments: true
---

###Problem description
Given n non-negative integers representing the histogram's bar height where 
the width of each bar is 1, find the area of largest rectangle in the histogram.
<figure>
    <img src="http://www.leetcode.com/wp-content/uploads/2012/04/histogram.png">
    <figcaption>Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].</figcaption>
    <img src="http://www.leetcode.com/wp-content/uploads/2012/04/histogram_area.png">
    <figcaption>The largest rectangle is shown in the shaded area, which has area = 10 unit.
</figcaption>
</figure>
For example,
Given height = [2,1,5,6,2,3],
return 10.

###Idea
Use a stack to keep the indexes which corresponding heights are in ascending mode. Once a smaller height is found, go back popping heights from stack and calculate the max histogram until the height on top of stack is smaller than the current. Stack changes for the example {2,1,5,6,2,3}: a. 2; b. null; c. 1; d. 1,5; e. 1,5,6; f. 1,5; g. 1,2; h. 1,2,3; Finally, the stack will be emptied and get the max. Note that the stack is always in ascending mode.

###Solution
{% highlight java %}
public int largestRectangleArea(int[] height) {
	Stack<Integer> stack = new Stack<Integer>();
    int max = 0, i = 0;
    while (i<height.length) {
        if (stack.isEmpty() || height[i] >= height[stack.peek()]) {
            stack.push(i++);
        }
        else {
            int t = stack.pop();
            max = Math.max(max, height[t]*(stack.isEmpty()?
            i:i-stack.peek()-1));
        }
        System.out.println(max);
    }
    while (!stack.isEmpty()) {
        int t = stack.pop();
        max = Math.max(max, height[t]*(stack.isEmpty()?
        i:i-stack.peek()-1));
    }
    return max;
}

{% endhighlight %}

