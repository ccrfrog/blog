











## 深度优先搜索DFS



### FlattenBinaryTreetoLinkedList




### ConstructBinaryTreefromPreorderandInorderTraversal
从前序遍历 和 后序序列 构造二叉树。注意 序列中不能有重复元素。

	`public TreeNode buildTree(int[] preorder, int[] inorder) {}`

* idea: preorder[0] 为根 root，根据 root 从 inorder找出左右子树的长度递归处理。
base case: preorder.length = 1 



    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (postorder.length == 0) {
            return null;
        }
        if (postorder.length == 1) {
            return new TreeNode(postorder[0]);
        }
        
        TreeNode root = new TreeNode(postorder[postorder.length - 1]);
        int leftIdx = 0;
        for (int i = 0; i < inorder.length; i++) {
            if (inorder[i] == root.val) {
                leftIdx = i;
                break;
            }
        }
        
        int[] leftpostorder = Arrays.copyOfRange(postorder, 0, leftIdx);
        int[] leftInorder = Arrays.copyOfRange(inorder, 0, leftIdx);
        int[] rightpostorder = Arrays.copyOfRange(postorder, leftIdx, postorder.length - 1);
        int[] rightInorder = Arrays.copyOfRange(inorder, leftIdx + 1, inorder.length);
        root.left = leftpostorder.length > 0 ? buildTree(leftInorder, leftpostorder) : null;
        root.right = rightpostorder.length > 0 ? buildTree(rightInorder, rightpostorder) : null;
        return root;
    }




### ConstructBinaryTreefromInorderandPostorderTraversal
从中序遍历 和 后序遍历 序列构造二叉树


### BalancedBinaryTree
Given a binary tree, determine if it is height-balanced. balanced: 每一个结点两个子树的高度差不超过1



## 广度优先遍历BFS

### WordLadder2
找出所有 将 from变成to 的最短路径，每次改变一个字母

### WordLadder
给定两个单词from & to, 和一个词典dict，求将 from变成to 的最短路径，每次改变一个字母


### PopulatingNextRightPointersinEachNode
给定含next 字段的二叉树，填充每个结点的 next 值为同层的右兄弟结点



### BinaryTreeZigzagLevelOrderTraversal
二叉树 zigzag 层序遍历，

		3
	   / \
  	  9  20
  	  /  \
  	 15   7

	[
	  [3],
	  [20,9],
	  [15,7]
	]
* idea: 使用 forward 变量判断方向。遍历每层元素判断方向 if forward 直接加入到list，否则先 入栈再从栈加入list。


### BinaryTreeRightSideView
给定二叉树，返回从右侧看该二叉树 得到的结点。函数原型为

	public List<Integer> rightSideView(TreeNode root) {}

* idea: 层序遍历，只输出每层最后一个结点。


### BinaryTreeLevelOrderTraversal2
与上一题唯一的不同在，最下层先输出。eg

		3
	   / \
  	  9  20
  	  /  \
  	 15   7

	[
	  [15,7],
	  [9,20],
	  [3]
	]

* idea: 外层使用LinkedList，每层结束时加入到头部即可。`wrapList.add(0, subList);`

### BinaryTreeLevelOrderTraversal
二叉树层序遍历，函数原型为

	    public List<List<Integer>> levelOrder(TreeNode root) {}
        
* idea: 注意每层对应一个 list，外层循环开始时 q.size() 即为本层元素个数 l，循环l 次将本层元素的下一层入队，并加入list



## Binary Search Tree二分查找树

重要特点/技巧：


		1.  中序序列是有序的
		2.  中序遍历时维持一个指针 prev，指向前一个元素（在递归调用right 前prev = root）



### UniqueBinarySearchTrees2
Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1...n。

		public List<TreeNode> generateTrees(int n) {}

* idea: 问题可以描述为 gen(1, n)，如果以 i 为根结点，那么 左子树只能出现 n[1, i-1]，右子树只能出现 n[i+1, n]。
递归生成 left/right，遍历left/right 生成 以i 为根的树加入结果集。


### ValidateBinarySearchTree
判断一个给定的bst 是否是有效的bst（每个结点大于左子树最大值，小于右子树最小值）

* idea: 求左子树的最大元素，判断是否小于root，求右子树的最小元素，判断是否大于root。递归处理左右子树。

### RecoverBinarySearchTree
Two elements of a binary search tree (BST) are swapped by mistake. Recover the tree without changing its structure.

* idea: 假设被交换的两个元素是a b，那么在中序序列里会存在 a.next < a, b.prev > b，通过prev 指针和中序遍历找到这两个元素a b。
交换它们的值即可。


### LowestCommonAncestorofaBinarySearchTree
给定bst root和两个结点p/q，返回p q 的lca。函数原型是

	public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {}

* idea: 在bst 中，lca 的特点是 p.val < lca.val < q.val，假设 p比q 小。因此本题变得非常简单。


### FindModeinBinarySearchTree
Given a BST with duplicates, find all the mode(s) 
(the most frequently occurred element) in the given BST。 mode元素可能有多个，全部返回。
函数原型

		public int[] findMode(TreeNode root) {}


* idea: 注意中序序列是有序的。本题与 从已排序数组里找 mode 元素基本一样。只是将 计算出现次数的代码插入到 递归遍历bst中。



### ConvertSortedListtoBinarySearchTree
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST

* idea: list 与array 的不同：不能随机访问。为了达到平衡，核心是找list 的中间位置。
设置两个指针， slow/fast，设置不同的步长即可到达中间位置。


### ConvertSortedArraytoBinarySearchTree
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

* constraints，高度要平衡的bst
* idea: 中间元素当root，左右部分递归处理

函数原型为

	public TreeNode sortedArrayToBST(int[] nums) {}


### MinimumAbsoluteDifferenceinBST
给定非负数构成的 bst，返回最小的 差值的绝对值
* idea：mDiff 只可能出现在 有序相邻的两个元素里，假设中序序列为 (e1, e2, e3... en)，那么 有 diff(ei, ei+1) < diff(ei, ei+2)
中序遍历，求每个 元素和它之前的值的差值，更新 mDiff 即可。该题便利用到了上边的特点1和2。


## Binary Search

基础的二分查找代码需要闭眼秒写出来。


	 private int bs(int[] nums, int low, int high, int target) {
        if (low > high) {
            return -1;
        }
        int mid = low + (high - low) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (target < nums[mid]) {
            return bs(nums, low, mid - 1, target);
        } else {
            return bs(nums, mid + 1, high, target);
        }
    }	


leetcode 里的题，经常会用到二分查找的变形。例如用二分查找 从 nums 里找大于等于 target 的第一个元素。代码如下


	public int firstGreaterEqual(int[] nums, int target) {
        int low = 0;
        int high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] < target) {// 去掉小于target 的元素
                low = mid + 1;
            } else {// nums[mid]>=target，target落在左半部分，此时 将high指向mid 允许步步逼近 target
                high = mid;
            }
			// 结束条件：左边将 小于target的元素去掉，右边挨个逼近 low，最终会结束在 low==mid==high
            if (high == mid && low == mid) {
                break;
            }
        }
        return low;
    }


### SearchforaRange
给定已排序数组和target，返回 target在数组里的开始和结束索引值。
`eg. Given [5, 7, 7, 8, 8, 10] and target value 8, return [3, 4]`

在 firstGreaterEqual子程序基础上该题很容易解。


    public int[] searchRange(int[] nums, int target) {
        int idx = firstGreaterEqual(nums, target);
        if (idx >= nums.length || nums[idx] != target) {// 没找到
            return new int[]{-1, -1};
        }
        return new int[]{idx, firstGreaterEqual(nums, target + 1) - 1};
    }


### SearchInsertPosition
给定已排序的数组和目标值t ，如果t 出现在数组里，返回它的索引值，否则返回它的插入位置。
数组里没有重复元素

		[1,3,5,6], 5 → 2
 		[1,3,5,6], 2 → 1
 		[1,3,5,6], 7 → 4
 		[1,3,5,6], 0 → 0

* idea：二分查找，1)找到返回mid，2）未找到时 low 即为插入位置


### FindPeakElement
从数组num 里找极值，极值的定义是 大于相邻元素
notice: `num[-1] = num[n] = -∞`

* idea: 写几个case 看看。要求用 logN 时间解。比较 mid与 mid-1/mid+1，然后分别只需考虑左半部分，右半部分
* base case: length=1, length=2



### SearchinRotatedSortedArray
从轮转排序的数组中找 target值，找到返回索引值，否则返回 -1。
`(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2)`

* idea1：找pivot 元素，例如 0，pivot的元素特征是 pivot左边元素比它大。根据pivot 元素将数组还原成一个有序的 将前pivot个元素copy到末尾
例如 `(4 5 6 7 0 1 2 4 5 6 7)` 然后从 0开始的元素里搜索。

* idea2: 写出所有可能的 轮转情况，观察到 数组最有一边是 有序的，中间的数小于最右边的数，则右半段是有序的，若中间数大于最右边数，则左半段是有序的，
要在有序的半段里用首尾两个数组来判断目标值是否在这一区域内，这样就可以确定保留哪半边了。分治策略。同FindPeakElement



### ArrangingCoins
You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.
Given n, find the total number of full staircase rows that can be formed.
n is a non-negative integer and fits within the range of a 32-bit signed integer.


## Backtracking

### WordSearch(高频)
给定2维数组board 和单词 word ，判断word 是否在board 顺序相邻的cell 里存在

		[
		  ['A','B','C','E'],
		  ['S','F','C','S'],
		  ['A','D','E','E']
		]

		word = "ABCCED", -> returns true,
		word = "SEE", -> returns true,
		word = "ABCB", -> returns false


* idea 从 `board[i][j]` 任意一个cell 出发做 backtrack。顺时针方向 直到匹配word
     

	     base case:
	           level == word.length
	     for up/right/down/left 
	          if idx of i,j in (0, m), (0, n) 
	              backtrack


### RestoreIPAddresses
给定字符串s ，返回s 所有可能表示的 IP地址。eg. s = "25525511135", r = [255.255.11.135, 255.255.111.35]

* idea:原问题可以理解成 将3个 点插入 s，得到4个部分，每个部分的整数值应该在 [0,255] 之间
     * bk(r, path, s, dots): dots: 已经插入 s 的点的个数
     * 考虑长度为 1,2,3 的子串 prefix，如果prefix 满足条件，那么问题变成了
     * bk(r, path, s-prefix, dots + 1)
     *     basecase: dots == 3
     *     注意只考虑满足条件的 prefix/rest 

* test cases: 注意边界case，[0.1.01.1] 不算合法ip。求子串剩余的部分可能为 empty


	    private void bk(List<String> r, String path, String s, int dots) {
	        if (dots == 3) {// 3个点已经
	            Integer section = Integer.valueOf(s);
	            if (s.length() > 1 && s.startsWith("0")) {
	                return;
	            }
	            if (section >= 0 && section <= 255) {
	                r.add(path + s);
	            }
	            return ;
	        }
	        // 从s 取长度为 [1, 2, 3] 的子串
	        for (int i = 1; i <= 3; i++) {
	            if (i > s.length()) {
	                continue;
	            }
	            String prefix = s.substring(0, i);
	            Integer section = Integer.valueOf(prefix);
	            if (prefix.length() > 1 && prefix.startsWith("0")) {
	                continue;
	            }
	            String rest = s.substring(i);
	            if (section >= 0 && section <= 255 && rest.length() > 0) {
	                bk(r, path + prefix + ".", s.substring(i), dots + 1);
	            }
	        }
	    }



### LetterCombinationsofaPhoneNumber
给定九宫格键盘和 一个数字串，求所有可能的字母组合。例如 s=23, 所有可能的字母组合是 `[ad, ae, af, bd, be, bf, cd, ce, cf]`
九宫格键盘数字与字母的映射关系是

		mapping.put('2', "abc");
        mapping.put('3', "def");
        mapping.put('4', "ghi");
        mapping.put('5', "jkl");
        mapping.put('6', "mno");
        mapping.put('7', "pqrs");
        mapping.put('8', "tuv");
        mapping.put('9', "wxyz");

* idea: 问题可以这样描述 bk(r, path, s, idx, mapping)，r/path/s 不解释，idx 用来控制递归，每层对应 s 的一个字符。
base case: idx == s.length()，否则根据 s.charAt(idx) 找对应的 字母，遍历加到path 再递归调用即可。



### GenerateParentheses
Given n pairs of parentheses, generate all combinations of
well-formed parentheses eg. n = 3, ["((()))", "(()())", "(())()", "()(())", "()()()"]

* idea: 问题可以用 bk(r, path, open, close, n) 来描述，其中 r 保存输出结果，path 表示解空间树一条路径，
open 表示已经生成的 ( 个数，close 表示已经生成的 ) 个数。 base case: open == n && close == n，加入一个结果。
否则 继续生成 ( 的条件是 open < n，继续生成 ) 的条件是 close < open。


### CombinationSum2
在 CombinationSum 基础上加一个条件，每个重复元素只可以出现一次
例如 `[10, 1, 2, 7, 6, 1, 5] and target 8`，期望结果为

	[
	  [1, 7],
	  [1, 2, 5],
	  [2, 6],
	  [1, 1, 6]
	]


### CombinationSum
给定一个候选数字集，和一个目标值 target，输出所有不重复的组合C，使得 `sum(C) = target`。例如
[2, 3, 6, 7], target = 7,结果集为 `[[2, 2, 3], [7]]`

    private void bk(List<List<Integer>> r, Deque<Integer> path, 
            int[] nums, int t, int start) {
        if (t < 0) {return ;}
        if (t == 0) {
            r.add(new ArrayList<>(path));
            return ;
        }
        for (int i = start; i < nums.length; i++) {
            path.push(nums[i]);
            bk(r, path, nums, t - nums[i], i);
            //注意这里最后一个参数一定要传i，表示 不选元素 nums[i-1]
            path.pop();
        }
    }


Subsets, Combinations, Permutations 三个题的代码需要牢记。三个问题的解空间分别对应 子集树，选与不选树，排列树。

### Combinations
给定两个整数n, k，返回所有从 [1..n] 中取k 个数的组合。注意 Combinations 的解空间
与 permutation 最大的不同是，解空间是一棵不对称的树(选与不选树)。


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





