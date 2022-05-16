---
title: Algorithm
date: 2022/2/24
description: Algorithm Learning Record.
tag: algorithm
author: Songjiaxuan
---

# Algorithm Learning Record

###	Bit manipulation



#### Exchange the values of two numbers without requesting extra space

a = a $\wedge$ b
b = a $\wedge$ b
a = a $\wedge$ b

$\wedge$ operation satisfies the exchange law

>a and b must refer to different memories, but the values can be the same
>
>If the memory location is the same, it is equivalent to dissimilarity with itself and the value will be washed to zero

####	Example questions

- There is only one number in an array that occurs an odd number of times, find this number

  - perform $\wedge$ on all items of this array, and the final result obtained is the number that occurs an odd number of times. Cause a $\wedge$ a = 0, 0 $\wedge$ b = b, so the number that occur an even number will result 0, the number that occur an odd number will result itself.

- There are only two numbers in an array that occur an odd number of times, find these two numbers

  - we first get the value $eor$ = $odd_1$ $\wedge$ $odd2$, since $odd_1$ and $odd_2$ are not equal, this value $eor$must not be 0, so the binary representation of this value mush have at least one bit of 1, and we take the rightmost 1 by default.

  - but how to get the rightmost 1: $rightOne = eor \& (\thicksim eor + 1)$

    ```
    eor = 1001101100
    ～eor = 0110010011
    ～eor+1 = 0110010100
    eor & (～eor + 1)=0000000100a		
    ```

  - then we device the array into two parts according to whether the $rightOne$ position is 1 or not, a and b must have different value on the $rightOne$ position, so a and b must be devided into different parts

  - we then create a new value $eor^{'}$ to perform $\wedge$ on all items which have the value 1 on the $rightOne$ position. So the $eor ^{'}$ equals a or b, and $eor^{'}$ $\wedge$  $eor$ can get the other a or b 

  ![截屏2022-02-24 09.04.28.png](https://s2.loli.net/2022/02/24/7ZlOEtHcMD2SjC8.png)

###  Insert Sort

Just like playing poker, every time you receive a card, you insert that card into the right place in front of you, and the cards in front of you are already in order

```
if (arr == null || arr.length < 2):
	return

% 0~0 有序
% 0~i 想有序
for (i = 1; i < arr.length; i++): % 0~i
	for (j = i-1; j>0 && arr[j]>arr[j+1]; j--):
		swap(arr, j, j+1)
```

###	Binary Sort

- Find if a number N exists in a sorted array
- Find the leftist position that geq a number N in a sorted array
- The problem of the local minumum

###  Get_Max

```
def get_max(array):
		return process(array, 0, array.length-1)
		
def process(array, L, R):
		if (L == R):
				return array[L]  % array[L...R]范围内只有一个数，直接返回
				
    mid = L + ((R - L) >> 1)  %mid = (L + R) / 2,在数组长度很长的情况下，这种写法可能会溢出，因此会得到一个负值，我们可以写成mid = L + (R - L) / 2 = L + ((R - L) >> 1)，除以2相当于左移一位
    leftMax = process(array, L, mid)
    rightMax = process(array, mid + 1, right)
    reutnr Max.max(leftMax, rightMax)
```

![截屏2022-02-24 18.02.19.png](https://s2.loli.net/2022/02/24/n58iGPqAvTmrkbj.png)

#####	递归问题的时间复杂度

当一个递归母问题的时间复杂度可以进行以下分解时(master公式): $T(N) = aT(\frac{N}{b}) + O(N^{d})$，对于上述问题，$T(N) = 2T(\frac{N}{2})+O(1)$

- 若$log_b a < d$， $O(N^d)$
- 若$log_b a > d$，$O(N^{log_b a})$
- 若$log_b a = d$，$O(N^d * logN)$

###  MergeSort

```
def merge_sort(array, L, R):
		if (L == R):
				return 
		return process(array, L, R):
		
def process(array, L, R):
		mid = L + ((R-L)>>1)
		process(array, L, mid) %左边有序
		process(array, mid+1, R)  %右边有序
		merge(array, L, mid, R)  %merge
		
		
def merge(array, L, mid, R):
		help = {}
		i = 0
		p1 = L
		p2 = mid + 1
		
		while (p1 <= mid && p2 <= R):  %两边都不越界
				help[i++] = array[p1] <= array[p2] ? array[p1++] : array[p2++]
		while (p1 <= mid):  %右半部分越界
				help[i++] = array[p1++]
    while (p2 <= R):   %左半部分越界
    		help[i++] = array[p2++]
    for i in range(help.length):
    		array[L+i] = help[i]
```

####	小和问题和逆序对问题

小和问题：在一个数组中，每一个数左边比当前小的数累加起来，叫做这个数组的小和。例如[1,3,4,2,5]，1左边比1小的数没有，和为0；3左边比3小的数有一个，和为1；4左边比4小的数有两个，和为4；2左边比2小的数有1个，和为1；5左边比5小的数有4个，和为10。因此改数组的小和为0+1+4+1+10=16

逆序对问题：在一个数组中，左边的数如果比右边的数大，则这两个数构成一个逆序对，请打印所有逆序对

######	SmallSum

- 我们首先转换思路，对于数组[1,3,4,2,5]，我们最终要求的是整体的小和，可以分解为子小和的求和；左边有多少数比当前数小，就会产生多少个子小和；那么就等同于右边有多少个数比当前数大，就产生几个当前数的子小和

- 对于数组[1,3,4,2,5]，对1来说，右边有4个数比1大，因此最终会产生4个1的子小和；对3来说，右边有两个数比3大，最终会产生2个3的子小和；以此类推

- 根据归并排序merge时的特性谁小取谁，我们可以对该数组分解，然后一步一步地merge

  ![截屏2022-02-25 09.58.43.png](https://s2.loli.net/2022/02/25/KynIdaW3SlVNzxZ.png)

  - 最开始数组[1]和[3]进行merge，当左侧比右侧小时产生小和，因此产生1个1的子小和（现在的右侧只有一个数比1大）；然后将1拷贝进来，随后左侧越界，将右侧的3拷贝进来得到子数组[1,3]
  - 现在[1,3]和[4]进行merge，首先1和4比较，左侧小，产生一个1的子小和，然后左侧指针到3，3和4比较，产生一个3的子小和，随后左侧指针越界，将右侧的拷贝进行，得到子数组[1,3,4]
  -  到现在，[1,3,4,2,5]的左侧已经完成，现在开始右侧，产生一个2的子小和并得到子数组[2,5]
  - 现在[1,3,4]和[2,5]进行merge，刚开始1和2进行比较，产生子和，但是是产生两个1的子和，因为右侧有两个数比1大（注意这个过程不是对右侧进行遍历的过程，而是简单的下标计算，因为右边数组是有序的，前边的数比左侧大，那么后边的数也一定比左侧大）。然后1拷贝下来，随后左侧指针到3位置，和右侧2比较，不产生子小和。右侧指针到5位置，3和5比较，产生一个3的子小和；随后产生一个4的子小和。最后得到数组[1,2,3,4,5]，并且也得到了所有的子小和

- 需要注意的是，如果进行merge时，左右数组中待比较元素相同，优先选择右侧数组，且不产生子小和。只有严格意义上的右侧大于左侧才产生子小和

  ```
  def smallSum(array):
  		if (array == null || array.length < 2):
  				return 0;
  		return process(array, 0, arr.length - 1)
  		
  def process(array, L, R)  %array[L...R]即要排好序，也要求小和
  		if(L == R):
  				return 0
  		mid = L + ((R - L) >> 1)
  		return process(array, L, mid) + process(array, mid+1, R) + merge(array, L, mid, R)  %左侧子小和 + 右侧子小和 + merge过程中产生的子小和
  		
  		
  def merge(array, L, mid, R):
  		help = {} %排序数组
  		i = 0
  		p1 = L
  		p2 = mid + 1
  		res = 0 %小和
  		while (p1 <= mid && p2 <= R): %左右都不越界时
  				res += array[p1] < array[p2]? (R - p2 + 1) * array[p1] : 0. %计算右边有多少个数大于左边
  				help[i++] = array[p1] < array[p2]? array[p1++] : array[p2++] %更新排序数组
  				
  		while (p1 <= mid):  %右侧越界，左侧全部拷贝
  				help[i++] = array[p1++]
      while (p2 <= R): %左侧越界，右侧全部拷贝
      		help[i++] = array[p2++]
      for i in range(help.length):
      		array[L + i] = help[i]
      		
      return res
  ```

######  逆序对问题

与小和问题类似，对于数组[3,2,4,5,0]，3的逆序对为[3,2] ，[3,0]，2的逆序对为[2,0]，4的逆序对为[4,0]，5的逆序对为[5,0]，求所有逆序对数量。只需求出右边有多少个数比当前数小即可
