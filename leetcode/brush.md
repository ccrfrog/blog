




## Backtracking



### CombinationSum
给定一个候选数字集，和一个目标值 target，输出所有唯一的组合C，使得C 中元素之和等于 target。例如
[2, 3, 6, 7], target = 7,结果集为 `[[7],[2, 2, 3]]`


		public List<List<Integer>> combinationSum(int[] nums, int target) {
		    List<List<Integer>> list = new ArrayList<>();
		    Arrays.sort(nums);
		    backtrack(list, new ArrayList<>(), nums, target, 0);
		    return list;
		}
		
		private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start){
		    if(remain < 0) return;
		    else if(remain == 0) list.add(new ArrayList<>(tempList));
		    else{ 
		        for(int i = start; i < nums.length; i++){
		            tempList.add(nums[i]);
		            backtrack(list, tempList, nums, remain - nums[i], i); // not i + 1 because we can reuse same elements
		            tempList.remove(tempList.size() - 1);
		        }
		    }
		}			
			 


Subsets, Combinations, Permutations 三个题的代码需要牢记。

### Combinations
给定两个整数n, k，返回所有从 [1..n] 中取k 个数的组合。注意 Combinations 的解空间
与 permutation 最大的不同是，解空间是一棵不对称的树。


	public static void combine(List<List<Integer>> combs, List<Integer> comb, int start, int n, int k) {
        if (k == 0) {
            combs.add(new ArrayList<Integer>(comb));
            return;
        }
        for (int i = start; i <= n; i++) {
            comb.add(i);
            combine(combs, comb, i + 1, n, k - 1);
            comb.remove(comb.size() - 1);
        }
    }



### Subsets
Given a set of distinct integers, nums, return all possible subsets
没什么可说的必须牢记的代码。


	private void backtrack(int depth, int length, int[] nums, int[] path, 
            List<List<Integer>> subsets) {
        if (depth >= length) {
            subsets.add(createSolution(path, nums));
            return ;
        }
        for (int i = 0; i < 2; i++) {
            path[depth] = i;
            backtrack(depth + 1, length, nums, path, subsets);
        }
    }



### Permutations 
求排列

	public void backtrack(int t, int n, int[] x, List<List<Integer>> permute) {
        if (t >= n) {
            permute.add(createSolution(x));
            return ;
        }
        for (int i = t; i < n; i++) {
            swap(x, t, i);// x[t] = h(i)当前状态第i个可选值
			backtrack(t + 1, n, x, permute);
            swap(x, t, i);
        }
    }


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

	public void moveZeroes(int[] nums) {}

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


### MissingNumber
给定n个元素的数组，其中的元素取值范围是 0,1, ... n
找出数组中缺失的元素：n + 1 个元素里出现了 n个。

	public int missingNumberxor(int[] nums) {}


	    public int missingNumberxor(int[] nums) {
        int xor = 0, i = 0;
        for (i = 0; i < nums.length; i++) {
            xor ^= i ^ nums[i];
        }
        return xor ^ i;
    }

### PlusOne
给定一个由数组表示的 非负整数，例如 328 = [3, 2, 8]，给该数字加1。最高位在最左边

* idea: 从右向左 遇到小于 9的数字+1 返回即可。遇到9 则置为0，待上一位进位。如果数字全部是9 则需升位，数组长度加1

	
	    public int[] plusOne(int[] digits) {
	        int n = digits.length;
	        for (int i = n - 1; i >= 0; i--) {
	            if (digits[i] < 9) {
	                digits[i]++;
	                return digits;
	            }
	            digits[i] = 0;
	        }
	        int[] newNumber = new int[n + 1];
	        newNumber[0] = 1;
	        return newNumber;
	    }


### ProductofArrayExceptSelf
给定 n 个元素的数组nums，返回一个 output 数组 使得 output[i] 等于 nums中除 nums[i] 之外其它元素的乘积。
eg. given `[1,2,3,4], return [24,12,8,6]`

* constraints: 不使用 除法操作，时间复杂度为 O(n)

* idea: 对于 nums[i] 如果知道 它左边所有数字的乘积 forward[i]，也知道它右边数字的乘积backward[i]，
那么 forward[i] * backward[i] 即为目标值。
使用两个数组，分别求出左边和右边数字的乘积，再遍历 求对应的 target 即可


### RemoveDuplicatesfromSortedArray
给定有序数组nums，去掉其中的重复元素使得每个元素只出现一次，返回新数组的长度。
eg. given `[1, 1, 2]` return 2，新数组为 `[1, 2]`。

* idea：一遍扫描将非重复元素移到数组左边，j 指向最右边的非重复元素数组，循环结束时返回 `j+1` 即可。


	public int removeDuplicates(int[] nums) {}


### RemoveElement
给定一个数组nums 和一个值 val，从nums 里去掉所有 val 元素，返回新数组的长度。
* constraints: 使用 O(1) 额外空间。

* idea: 一遍扫描将不等于val 的元素移到数组左边，j 指向子数组最右边位置。


### RotateArray
n 个元素的数组向右轮转k 步。例如 `nums = [1, 2, 3, 4, 5, 6, 7]，k = 3`
返回 `[5, 6, 7, 1, 2, 3, 4]`。
至少有3种解法，给出尽可能多的解法。期望in place，O(1) 空间。

* idea 1：前n-k 个元素直接copy 到末尾，返回以 n-k 为头的子数组
* idea 2: 复用reverse 子程序，翻转子数组 [0, n-k]，[n-k, n]，再整体翻转即可
* idea 3: copy 末尾k 个元素，整体右移 k步，k个元素 copy回头部



### RotateImage
n*n 的二维数组，顺时针方向旋转90度

* idea: first reverse up to down, then swap the symmetry(row/column)


		1 2 3     7 8 9     7 4 1
	    4 5 6  => 4 5 6  => 8 5 2
	    7 8 9     1 2 3     9 6 3


### SetMatrixZeroes
给定m*n 矩阵, 如果其中一个元素是0 将整行和整列都置为0

* idea: 第一遍遍历 matrix 用两个数组 row/column 记下的所有包含0 的行和列。再次遍历 matrix 时`if row[i] == 0 || column[j] == 0 then m[i][j] = 0`


### SortColors
给定由 {0=red, 1=white, 2=blue} 组成的数组nums，排序。

* idea 1: 两遍扫描，第1遍记录0,1 的个数，第2遍先填充0 再填充1 再填充2


		Integer zeros = map.getOrDefault(0, 0);
        Integer ones = map.getOrDefault(1, 0);
        for (int i = 0; i < nums.length; i++) {
            nums[i] = zeros-- > 0 ? 0 : (ones-- > 0 ? 1 : 2);
        }


* idea 2: 一遍扫描常量空间，以1 为pivot 元素，将0交换到 开头，将2交换到末尾，需要注意的是 与2交换的也可能是个2 这时候需要i-1 继续处理。


### SpiralMatrix
Given a matrix of m x n elements，以螺旋形式返回 矩阵里的元素。

* idea: 按层输出，根据level 和 matrix 大小计算左上角/右下角 坐标，分别输出->, down, <-, up
4 个方向的元素。然后以level + 1 递归调用。
`private void spiral2(int[][] matrix, int level, List<Integer> list, int size) {}`
初始调用为 `spiral2(matrix, 0, list, m*n);`

base case1: rList.size == m*n, base case2: 一行，输出该行return，base case3: 一列，输出该列return。



### ThirdMaximumNumber
给定非空数组，输出第3大的数。如果不存在输出最大的元素。
如果存在重复，输出数值是第3大的数。例如 [2, 2, 3, 1] 返回 1。

* idea: 用一个优先级队列维护最大的3个数，注意 默认的 PriorityQueue 按自然序排列，因此传一个 Comparator 进去
从大到小排序。另 PriorityQueue 允许重复元素，根据题意 只将非重复元素加入 pq 即可。












