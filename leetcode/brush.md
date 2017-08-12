




## Array

### TwoSum

给定数组nums 和值 target，判断是否存在`nums[i] + nums[j] = target`
存在返回对应的索引值i, j，否则返回[-1, -1]

	public int[] twoSum(int[] nums, int target) {}

因本题要求返回索引值，基于 排序后的二分查找，或者夹逼查找都不方便返回索引值，时间复杂度也不是最优。
本题的最优解法是基于map 的一遍遍历

	Integer rest = target - nums[i];// left = nums[i]
	if (map.containsKey(rest)) {// nums[i]=2, right=7, target=9
		Integer idx = map.get(rest);
		return new int[]{idx, i};
	} else {
		map.put(nums[i], i);
	}

### MoveZeroes

给定一个数组nums，将其中的元素0 移到数组的末尾，保持其它元素的相对顺序不变
`eg. nums = {0, 1, 0, 3, 12} => {1, 3, 12, 0, 0}` 

要求 in-place：不能copy 数组，操作数尽可能少。函数原型为

	`public void moveZeroes(int[] nums) {}`

* idea: 如果类似快排里的 partition 子程序直接将0 交换到末尾不能保持其它元素的相对顺序，考虑将非0元素移到开头，再将末尾元素置0


### ContainsDuplicate
给定一个整数数组，判断其中是否含有重复元素

* idea: 一遍扫描nums 元素加入set，如果 set.size() != nums.length 则有重复元素


### FindMinimuminRotatedSortedArray
给定一个被旋转过的有序数组，例如 {0 1 2 4 5 6 7} might become {4 5 6 7 0 1 2}
返回其中最小的元素

	public int findMin(int[] nums) {}

* idea: 一遍扫描保存min 显然不是期望的。因为是已排序的考虑 logN 级别的时间复杂度。
写几个case 看看 min 分别在 左半部分和右半部分的情况。
base case: length = 1, length = 2, nums[a] < nums[b]

* 需要比较的是 low 与 mid 对应的值，随着 rotated 元素个数的增加，nums[low] 会先大于 nums[mid] 再小于 nums[mid] 

### FindMinimuminRotatedSortedArray2
在 FindMinimuminRotatedSortedArray 基础上增加一种 case：允许重复元素

* idea: 当 nums[mid] == nums[low] 时，low++ 即可


### FirstMissingPositive
给定未排序的数组 nums，返回第1个缺失的正整数(最小为1)
eg. Given `[1,2,0]` return 3, and `[3,4,-1,1]` return 2

* constraints：1 O(n) 时间，2 constant space
* idea： 如果非constant space，可以将每个元素e 加入map，再从1到n遍历，第1个不在map 里的即为目标值
根据 constant space要求，考虑直接在原数组上，将 `e = nums[i]` 交换到 e - 1 位置。e 可能当索引值，所以 e的范围应该是 0 <= e < nums.length。
根据题意 小于0的e 可以忽略因为只考虑正整数；大于length 的元素也可以忽略，因为 根据鸟巢原理 0 < target <= n + 1。
然后 从第1个元素开始 遇到的第1个 不等于 `i + 1` 的e 对应的 `i+1` 即为目标值。

函数原型为


		int firstMissingPositive(int nums[]) {}

### MergeSortedArray
给定两个已排序数组num1 num2，将 num2 merge到 num1 成1个数组。

* note：num1 有足够的空间容纳 num2 里的元素，另两个数组的初始元素个数分别为 m 和 n
* idea: 对num2 中的每一个元素e 从num1 里找正确的位置填放。依赖一个子程序从 num1 里找正确的位置，并将 比e大的元素都右移腾出空间。
`private int findPlace(int[] nums1, int m, int e) {}`

函数原型为


	public void merge(int[] nums1, int m, int[] nums2, int n) {}



### 