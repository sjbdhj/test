---
title: 编程题一——寻找捣乱分子对
date: 2020-05-29
tags:
 - 简介
 - 代码
categories:
 - 编程题
---

# 问题描述：
    多人排成一个队列，我们认为从低到高是正确的序列，但是总有部分人不遵守秩序。如果说，前面的人比后面的人高(两人身高一样认为是合适的)，那么我们就认为这两个人是一对“捣乱分子”，比如说,现在存在一个序列:176, 178, 180, 170, 171
    这些捣乱分子对为<176, 170>, <176, 171>, <178, 170>, <178, 171>, <180, 170>, <180, 171>，那么，现在给出一个整型序列，请找出这些捣乱分子对的个数(仅给出捣乱分子对的数目即可，不用具体的对)
# 解题思路：
    最简单的就是用两层循环，即考察每个元素，检查该元素后面的元素是否小于它，如果是就找到一个捣乱分子对。复杂度为O(n^2)。这道题的一种改进方案是用分治法，用到了归并排序的思想。我们可以将数组分成两部分，总的捣乱分子对为： 前半部分的捣乱分子对 + 后半部分的捣乱分子对 + X，这里的X为两部分合并中出现的捣乱分子对，什么情况下会出现呢？假设前半部分的元素范围为 A[from] 到A[mid]，后半部分的元素范围为A[mid + 1] 到A[to]，考虑前半部分的某元素A[i]和后半部分的某元素A[j]，如果A[j] < A[i]，由于两部分都是排好序的，因此捣乱分子对增加 mid - i + 1个，也就是说A[j]小于A[i], A[i+1].. A[mid]，这个关系显然成立。
# 参考代码：
```C++
    int Merge(int *pArray, int from, int mid, int to)
{
	int i = from, j = mid + 1;
	int k = 0, num = 0;
	int *pTmp = new int[to-from+1];
	
	while(i<=mid && j<=to) //归并排序的主框架
	{
		if(pArray[i] <= pArray[j])
			pTmp[k++] = pArray[i++];
		else
		{
			num += (mid - i + 1);         //增加捣乱分子对
			for(int l = i; l <= mid; l++) //输出捣乱分子
				cout<<pArray[l]<<' '<<pArray[j]<<endl;
 
			pTmp[k++] = pArray[j++];
		}
	}
	while(i <= mid) pTmp[k++] = pArray[i++];
	while(j <= to) pTmp[k++] = pArray[j++];
 
	for(k = from ; k <= to; k++)
		pArray[k] = pTmp[k - from];
	delete [] pTmp;
	return num;
}
int MergeSort(int *pArray, int from, int to)
{
	if(from < to)
	{
		int mid = (from + to) /2;
		int num = MergeSort(pArray, from ,mid) + MergeSort(pArray, mid+1, to); //分别算出两部分的捣乱分子对
		num += Merge(pArray, from, mid, to);   //合并中出现的捣乱分子对
		return num;
	}
	return 0;
}
```