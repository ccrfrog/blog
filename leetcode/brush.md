




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

要求 in-place：不能copy 数组，操作数尽可能少

* idea: 如果类似快排里的 partition 子程序直接将0 交换到末尾不能保持其它元素的相对顺序，考虑将非0元素移到开头，再将末尾元素置0



