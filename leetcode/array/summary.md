



# summary
一般有O(n) 空间复杂度的简单解法，
更常见的要求是需要减小空间复杂度到O(1)


注意以下常用技巧

1. 从两端都可以方便地操作数组
2. 一般不删除元素，直接覆盖


## 基本子程序
1. 快速排序的partition 函数，原地将数组A划分成三部分：小于pivot 元素部分，pivot和大于pivot元素部分；
返回pivot 元素的位置

	    PARTITION(A, p, r)
		x = A[r] # 选择A[r] 当pivot 元素
		i = p-1  # i指向小于pivot 部分最后一个元素
		for j = p to r-1 #遍历未处理的元素
			if A[j] <= x
				i += 1
				A[j] <-> A[i]
		A[i+1] <-> A[r] # pivot 元素放到正确位置
		return i+1 # 返回pivot元素的位置
		


2. 双指针技巧，设置两个指针 `next_head`, `next_tail` 分别指向头尾，可以将数组划分成三部分，
两个指针互相逼近相遇时结束。例如`even_odd` 给定整数数组，将其中的元素原地重排序，使得偶数出现在
数组的前边部分。

	    # 原地一遍扫描将偶数放到数组前边部分
		def even_odd(nums):
		    # 分别指向下一个偶数，奇数元素，注意与PARTITION 子程序变量i的区别
		    next_even, next_odd = 0, len(nums) - 1
		    while next_even < next_odd: # 分别向右和向左移动，指向同一个元素时结束(考虑len=1的数组)
		        if nums[next_even] % 2 == 0:  # even: 直接右移
		            next_even += 1
		        else:  # odd: 交换到odd 部分，next_odd 左移
		            nums[next_even], nums[next_odd] = nums[next_odd], nums[next_even]  # 注意python 里swap的写法
		            next_odd -= 1
		


使用到该技巧的题有：`SortColors`



