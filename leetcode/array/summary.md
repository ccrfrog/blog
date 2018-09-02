



# summary
一般有O(n) 空间复杂度的简单解法，
更常见的要求是需要减小空间复杂度到O(1)


注意以下常用技巧

1. 从两端都可以方便地操作数组
2. 一般不删除元素，直接覆盖


## 基本子程序
1. 快速排序的partition 函数，原地将数组A划分成三部分：小于pivot 元素部分，pivot和大于pivot元素部分
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
		

