










http://www.cnblogs.com/grandyang/p/5628836.html





## Bit Manipulation




### UTF8Validation
A character in UTF8 can be from 1 to 4 bytes long, subjected to the following rules:


	    For 1-byte character, the first bit is a 0, followed by its unicode code.
	    For n-bytes character, the first n-bits are all one's, the n+1 bit is 0, followed by n-1 bytes with most significant 2 bits being 10.

	    public boolean validUtf8(int[] data) {
	        int cnt = 0;
	        for (int d : data) {
	            if (cnt == 0) {
	                if ((d >> 5) == 0b110) cnt = 1;
	                else if ((d >> 4) == 0b1110) cnt = 2;
	                else if ((d >> 3) == 0b11110) cnt = 3;
	                else if (d >> 7 == 1) return false;
	            } else {
	                if ((d >> 6) != 0b10) return false;
	                --cnt;
	            }
	        }
	        return cnt == 0;
	    }


### HammingDistance

    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x ^ y);
    }

### Numberof1Bits


    // 求n 的二进制表示中 1 的个数
    public int hammingWeight(int n) {
        int ones = 0;
        while (n != 0) {
            ones = ones + (n & 1);
            n = n >>> 1;
        }
        return ones;
    }


## Trie

### LongestCommonPrefix
给定一个字符串数组，返回这些字符串的最长公共前缀
`{leets, leetcode, leet, leeds}, r = lee`

	
	    public String longestCommonPrefixAccepted(String[] strs) {
	        Trie trie = new Trie();
	        for (String s : strs) {
	            if ("".equals(s)) {
	                return "";
	            }
	            trie.insert(s);
	        }
	        return trie.prefix();
	    }

        public String prefix() {
            TrieNode p = root;
            StringBuilder prefix = new StringBuilder();
            while (p.children.size() == 1) {
                Set<Character> keys = p.children.keySet();
                Iterator<Character> it = keys.iterator();
                char key = ' ';
                while (it.hasNext()) {
                    key = it.next();
                    prefix.append(key);
                }
                p = p.children.get(key);
                if (p.isLeaf) {
                    break;
                }
            }
            return prefix.toString();
        }


### AddandSearchWord

实现一个数据结构，支持以下两种操作

		void addWord(word)
		bool search(word)，其中search word 支持 a-z 和通配符. . 表示任意一个字符

	
	    public boolean search(String query) {
	        return search(trie.root, query, 0);
	    }
	    
	    private boolean search(TrieNode root, String query, int t) {
	        // base case:
	        if (t == query.length()) {
	            return root.isWord;
	        }
	        
	        List<TrieNode> pSet = new ArrayList<TrieNode>();
	        char c = query.charAt(t);
	        if (c == '.') {
	            for (TrieNode child : root.children) {
	                if (child != null) {
	                    pSet.add(child);
	                }
	            }
	        } else {
	            TrieNode child = root.children[c - 'a'];
	            if (child == null) {
	                return false;
	            }
	            pSet.add(child);
	        }
	        
	        boolean r = false;
	        for (TrieNode branch : pSet) {
	            r = r || search(branch, query, t + 1);
	        }
	        
	        return r;
	    }


### ImplementTrie

	
	class TrieNode {
	    public char val;
	    public boolean isWord;
	    public TrieNode[] children = new TrieNode[26];
	    public TrieNode() {
	    }
	    TrieNode(char c) {
	        TrieNode node = new TrieNode();
	        node.val = c;
	    }
	}
	
	public class Trie {
	    private TrieNode root;
	    public Trie() {
	        root = new TrieNode();
	        root.val = ' ';
	    }
	
	    public void insert(String word) {
	        TrieNode ws = root;
	        for (int i = 0; i < word.length(); i++) {
	            char c = word.charAt(i);
	            if (ws.children[c - 'a'] == null) {
	                ws.children[c - 'a'] = new TrieNode(c);
	            }
	            ws = ws.children[c - 'a'];
	        }
	        ws.isWord = true;
	    }
	
	    public boolean search(String word) {
	        TrieNode ws = root;
	        for (int i = 0; i < word.length(); i++) {
	            char c = word.charAt(i);
	            if (ws.children[c - 'a'] == null) return false;
	            ws = ws.children[c - 'a'];
	        }
	        return ws.isWord;
	    }
	
	    public boolean startsWith(String prefix) {
	        TrieNode ws = root;
	        for (int i = 0; i < prefix.length(); i++) {
	            char c = prefix.charAt(i);
	            if (ws.children[c - 'a'] == null) return false;
	            ws = ws.children[c - 'a'];
	        }
	        return true;
	    }
	}
	

## Math

### IntegertoEnglishWords
将数字转成英文单词，输入数字 n 不超过 2^31

		 123 -> "One Hundred Twenty Three"
		 12345 -> "Twelve Thousand Three Hundred Forty Five"
		 1234567 -> "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"

	
	
	    private final String[] LESS_THAN_20 = { "", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine",
	            "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen",
	            "Nineteen" };
	
	    private final String[] TENS = { "", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty",
	            "Ninety" };
	
	    private final String[] THOUSANDS = { "", "Thousand", "Million", "Billion" };
	
	    public String numberToWords(int num) {
	        if (num == 0) return "Zero";
	
	        int i = 0;
	        String words = "";
	
	        while (num > 0) {// 外层循环计算有多少个 千
	            if (num % 1000 != 0) {
	                words = helper(num % 1000) + THOUSANDS[i] + " " + words;
	            }
	            num /= 1000;
	            i++;
	        }
	
	        return words.trim();
	    }
	
	    // helper 计算千以下的转换
	    private String helper(int num) {
	        if (num == 0) {
	            return "";
	        } else if (num < 20) {
	            return LESS_THAN_20[num] + " ";
	        } else if (num < 100) {
	            return TENS[num / 10] + " " + helper(num % 10);
	        } else {
	            return LESS_THAN_20[num / 100] + " Hundred " + helper(num % 100);
	        }
	    }



## Stack

### StackWash / StackPermute
给定入栈序列 input，每个元素可以入栈出栈一次，判断 outSeq 是否为合法的出栈序列

	    /**
	     * 给定入栈序列 input，每个元素可以入栈出栈一次，判断 outSeq 是否为合法的出栈序列
	     * eg. input=[1,2,3,4,5], out1 = [4,5,2,3,1] 不合法
	     * out2 = [4,5,3,2,1] 合法
	     * */
	    public boolean isStackPermute(List<Integer> input, List<Integer> outSeq) {
	        int i = 0;
	        Deque<Integer> stack = new LinkedList<>();
	        for (int e : outSeq) {
	            if (i >= input.size() && !stack.isEmpty() && stack.peek() != e) {
	                return false;
	            }
	            
	            while (stack.isEmpty() || stack.peek() != e) {
	                stack.push(input.get(i++));
	            }
	            // while ends with stack.peek() == e
	            stack.pop();
	        }
	        return true;
	    }
	
	
	    public List<List<Integer>> stackPermute(List<Integer> input) {
	        List<List<Integer>> r = Lists.newArrayList();
	        List<Integer> path = Lists.newArrayList();
	        Deque<Integer> stack = new LinkedList<>();
	        backtrack(input, stack, r, path, input.size());
	        return r;
	    }
	
	    private void backtrack(List<Integer> input, Deque<Integer> stack, List<List<Integer>> r, List<Integer> path, int n) {
	        // base case: input empty&& stack empty
	        if (path.size() == n) {
	            r.add(new ArrayList<>(path));
	            return ;
	        }
	        
	        if (!input.isEmpty()) {
	            Integer e = input.remove(0);
	            stack.push(e);
	            backtrack(input, stack, r, path, n);
	            stack.pop();
	            input.add(0, e);
	        }
	        if (!stack.isEmpty()) {
	            Integer e = stack.pop();
	            path.add(e);
	            backtrack(input, stack, r, path, n);
	            path.remove(path.size() - 1);
	            stack.push(e);
	        }
	        
	    }



### SimplifyPath
给出一个UNIX 文件系统路径，将它简化


    public String simplifyPath(String path) {
        Deque<String> s = new LinkedList<String>();
        for (String dir : path.split("/")) {
            if ("".equals(dir) || ".".equals(dir)) {
                continue;
            } else if ("..".equals(dir)) {
                if (!s.isEmpty()) {
                    s.pop();
                }
            } else {
                s.push(dir);
            }
        }
        StringBuilder r = new StringBuilder();
        while (!s.isEmpty()) {
            r.append("/").append(s.removeLast());
        }
        return r.length() == 0 ? "/" : r.toString();
    }


### ImplementStackusingQueues

		public class MyStack {
	        Queue<Integer> q = new LinkedList<Integer>();
	        public MyStack() {
	        }
	        
	        /**
	         * x 加入队列，x 放到队头
	         * [1, 2, 3] + 4 => [1, 2, 3, 4] => [4, 1, 2, 3]
	         * */
	        public void push(int x) {
	            q.offer(x);
	            for (int i = 0; i < q.size() - 1; i++) {
	                q.offer(q.poll());
	            }
	        }
	        
	        /**直接 poll 即可*/
	        public int pop() {
	            return q.poll();
	        }
	        
	        public int top() {
	            return q.peek();
	        }
	        
	        public boolean empty() {
	            return q.isEmpty();
	        }
	    }


### ImplementQueueusingStacks

	
	    class MyQueue {
	        Deque<Integer> s1 = new LinkedList<Integer>();
	        Deque<Integer> s2 = new LinkedList<Integer>();
	        Integer front = null;
	        public MyQueue() {
	        }
	        
	        public void push(int x) {
	            if (s1.size() == 0) {
	                front = x;
	            }
	            s1.push(x);
	        }
	        
	        public int pop() {
	            while (!s1.isEmpty()) {
	                s2.push(s1.pop());
	            }
	            Integer top = s2.pop();
	            front = s2.peek();
	            while (!s2.isEmpty()) {
	                s1.push(s2.pop());
	            }
	            return top;
	        }
	        
	        public int peek() {
	            return front;
	        }
	        
	        public boolean empty() {
	            return s1.isEmpty();
	        }
	    }


### EvaluateReversePolishNotation


	    public int evalRPN(String[] tokens) {
	        Deque<Integer> s = new LinkedList<Integer>();
	        Set<String> opts = new HashSet<>();
	        opts.add("+");opts.add("-");opts.add("*");opts.add("/");
	        for (String t : tokens) {
	            if (opts.contains(t)) {
	                Integer right = s.pop();
	                Integer left = s.pop();
	                switch (t) {
	                    case "+":
	                        s.push(left + right);
	                        break;
	                    case "-":
	                        s.push(left - right);
	                        break;
	                    case "*":
	                        s.push(left * right);
	                        break;
	                    case "/":
	                        s.push(left / right);
	                        break;
	                    default:
	                        break;
	                }
	            } else {
	                s.push(Integer.valueOf(t));
	            }
	        }
	        
	        return s.peek();
	    }


### BinarySearchTreeIterator

	    class BSTIterator {
	        Deque<TreeNode> stack = new LinkedList<>();
	
	        public BSTIterator(TreeNode root) {
	            // init: p 指向当前最小结点，stack 保存从root到p 路径中所有parent
	            TreeNode p = root;
	            while (p != null) {
	                stack.push(p);
	                if (p.left != null) {
	                    p = p.left;
	                } else break;
	            }
	        }
	
	        /** @return whether we have a next smallest number */
	        public boolean hasNext() {
	            return !stack.isEmpty();
	        }
	
	        /** @return the next smallest number */
	        public int next() {
	            TreeNode top = stack.pop();
	            // 求 top.successor
	            TreeNode p = top;
	            if (p.right != null) {
	                p = p.right;
	                while (p != null) {
	                    stack.push(p);
	                    if (p.left != null) {
	                        p = p.left;
	                    } else break;
	                }
	            }
	            return top.val;
	        }
	    }


### SortStack
使用push/pop/peek 操作给一个栈排序


* idea: 新建一个 stack r 保存排序后的结果。那么 外层循环变成了当s 不为空时出栈，将较小的元素push 到 r。


        Deque<Integer> r = new LinkedList<Integer>();
        while (!s.isEmpty()) {
            int top = s.pop();
            while (!r.isEmpty() && r.peek() > top) {
                s.push(r.pop());
            }
            r.push(top);
        }
        return r;


### MinStack

	public class MinStack {
	    
	    private Deque<Integer> stack;
	    private int min = Integer.MAX_VALUE;
	    
	    public MinStack() {
	        this.stack = new LinkedList<Integer>();
	    }
	    
	    public void push(int x) {
	        if (x <= min) {
	            min = x;
	        }
	        stack.push(x);
	    }
	    
	    public void pop() {
	        Integer top = stack.pop();
	        if (top <= min) {// 剩余的元素里找最小的
	            min = Integer.MAX_VALUE;
	            Iterator<Integer> it = stack.iterator();
	            while (it.hasNext()) {
	                Integer e = it.next();
	                min = (e <= min) ? e : min; 
	            }
	        }
	    }
	    
	    public int top() {
	        return stack.peek();
	    }
	    
	    public int getMin() {
	        return min;
	    }
	}



## 字符串 String


### ValidPalindrome
判断是否回文串，只考虑数字，字符，忽略大小写。

	
	    public boolean isPalindrome(String s) {
	        if (s == null) {
	            return true;
	        }
	        StringBuilder input = new StringBuilder();
	        for (int i = 0; i < s.length(); i++) {
	            char c = s.charAt(i);
	            if (Character.isDigit(c) || Character.isLetter(c)) {
	                input.append(Character.toLowerCase(c));
	            }
	        }
	        // edge casees
	        if (input.length() <= 1) {
	            return true;
	        }
	        // two pointers
	        int left = 0;
	        int right = input.length() - 1;
	        while (left < right) {
	            if (input.charAt(left) != input.charAt(right)) {
	                return false;
	            }
	            left ++;
	            right --;
	        }
	        return true;
	    }


### ReverseWordsinaString3
翻转每个单词 

	    public String reverseWords(String s) {
	        StringBuilder sb = new StringBuilder(s);
	        int left = 0;
	        for (int i = 0; i <= s.length(); i++) {
	            if (i == s.length() || s.charAt(i) == ' ') {
	                reverse(sb, left, i - 1);
	                left = i + 1;
	            }
	        }
	        return sb.toString();
	    }

### ReverseWordsinaString2
in-place: 不使用额外的存储空间

	    public String reverseWords(String s) {
	        StringBuilder sb = new StringBuilder(s);
	        int left = 0;
	        for (int i = 0; i <= s.length(); i++) {
	            if (i == s.length() || s.charAt(i) == ' ') {
	                reverse(sb, left, i - 1);
	                left = i + 1;
	            }
	        }
	        reverse(sb, 0, sb.length() - 1);
	        return sb.toString();
	    }
	
	    private void reverse(StringBuilder s, int l, int r) {
	        while (l < r) {
	            char t = s.charAt(r);
	            s.setCharAt(r, s.charAt(l));
	            s.setCharAt(l, t);
	            l++;r--;
	        }
	    }


### ReverseWordsinaString
Given s = "the sky is blue". return "blue is sky the"
	
	    public String reverseWords(String s) {
	        s = s.trim();
	        String[] arr = s.split("\\s+");
	        List<String> list = new LinkedList<String>();
	        
	        for (String word : arr) {
	            list.add(0, word);
	        }
	        return String.join(" ", list.toArray(new String[list.size()]));
	    }


### LongestSubstringWithoutRepeatingCharacters
给定字符串s 求最长没有重复字符的子串(对应的长度值)


	// 设s[i,j] 为没有重复字符的子患，遍历所有可能
    public int lengthOfLongestSubstring(String s) {
        int i = 0, j = 0, max = 0;
        Set<Character> set = new HashSet<>();

        while (j < s.length()) {
            if (!set.contains(s.charAt(j))) {
                set.add(s.charAt(j++));
                max = Math.max(max, set.size());
            } else {
                set.remove(s.charAt(i++));
            }
        }

        return max;
    }


### LongestPalindromicSubstring
求最长回文子串


	    /**
	     * 假设目标串(最长回文子串)为s[p-x, p+x]
	     * 其中p取值范围是 1-len(s), x取值范围是1 - len(s)/2
	     * 遍历所有可能的p, x值找出目标串
	     * */
	    private int lo, maxLen;
	    public String longestPalindromeAccepted(String s) {
	        int len = s.length();
	        if (len < 2) {
	            return s;
	        }
	        for (int i = 0; i < len-1; i++) {
	            extendPalindrome(s, i, i);  //assume odd length, try to extend Palindrome as possible
	            extendPalindrome(s, i, i+1); //assume even length.
	        }
	        return s.substring(lo, lo + maxLen);
	    }
	    private void extendPalindrome(String s, int j, int k) {
	        while (j >= 0 && k < s.length() && s.charAt(j) == s.charAt(k)) {
	            j--;
	            k++;
	        }
	        if (maxLen < k - j - 1) {
	            lo = j + 1;
	            maxLen = k - j - 1;
	        }
	    }




### FirstUniqueCharacterinaString
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.
`s = "leetcode" return 0. s = "loveleetcode", return 2.`
	
	    public int firstUniqChar(String s) {
	        int freq [] = new int[26];
	        for(int i = 0; i < s.length(); i ++)
	            freq [s.charAt(i) - 'a'] ++;
	        for(int i = 0; i < s.length(); i ++)
	            if(freq [s.charAt(i) - 'a'] == 1)
	                return i;
	        return -1;
	    }



### CompareVersionNumbers
比较版本号
`0.1 < 1.1 < 1.2 < 13.37`
`1.0.0 = 1.0`


		public int compareVersion(String v1, String v2) {
	        String[] list1 = v1.split("\\.");
	        String[] list2 = v2.split("\\.");
	        if (list1.length <= list2.length) {
	            return compare(list1, list2);
	        } else {
	            int r = compare(list2, list1);
	            return -r;
	        }
	    }
	
	    private int compare(String[] shorter, String[] longer) {
	        for (int i = 0; i < longer.length; i++) {
	            if (i >= shorter.length) {
	                Integer vl = Integer.valueOf(longer[i]);
	                if (vl == 0) {
	                    continue;
	                }
	                return -1;
	            }
	            
	            int sub1 = Integer.valueOf(shorter[i]);
	            int sub2 = Integer.valueOf(longer[i]);
	            if (sub1 > sub2) {
	                return 1;
	            } else if (sub1 < sub2) {
	                return -1;
	            }
	        }
	        return 0;
	    }


## 链表 LinkedList


### SwapNodesinPairs
Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given 1->2->3->4, you should return the list as 2->1->4->3. 

	
	    public ListNode swapPairs(ListNode head) {
	        ListNode p1 = head;
	        ListNode p2 = (head == null) ? null : head.next;
	        return swapPairs(p1, p2);
	    }
	    
	    private ListNode swapPairs(ListNode p1, ListNode p2) {
	        // base case: p1,p2 both null, p2 null 
	        if (p2 == null) {
	            return p1;
	        }
	        
	        ListNode nextP1 = p2.next;
	        ListNode nextP2 = (nextP1 == null) ? null : nextP1.next;
	        p2.next = p1;
	        p1.next = swapPairs(nextP1, nextP2);
	        return p2;
	    }



### SortList
Sort a linked list in O(n log n) time using constant space complexity
	
	
	
	    /**
	     * divide: 找中间元素 mid
	     * conquer: mergeSort(head, mid)
	     *          mergeSort(mid.next, tail)
	     * combine: 归并两个已经有序的linked list
	     * */
	    public ListNode sortList(ListNode head) {
	        // base case: 0 个或1个 元素
	        if (head == null || head.next == null) {
	            return head;
	        }
	        
	        // 找中间元素
	        ListNode prev = null;
	        ListNode slow = head;
	        ListNode fast = head;
	        while (fast != null && fast.next != null) {
	            prev = slow;
	            slow = slow.next;
	            fast = fast.next.next;
	        }
	        prev.next = null;
	        
	        return merge(sortList(head), sortList(slow));
	    }
	
	
	    // 归并两个已经排好序的 list
	    private ListNode merge(ListNode h1, ListNode h2) {
	        //base case: 其中一个为null
	        if (h1 == null) {
	            return h2;
	        }
	        if (h2 == null) {
	            return h1;
	        }
	        if (h1.val <= h2.val) {
	            h1.next = merge(h1.next, h2);
	            return h1;
	        } else {
	            h2.next = merge(h1, h2.next);
	            return h2;
	        }
	    }
	


### RotateList

	Given a list, rotate the list to the right by k places, where k is non-negative.
	Given 1->2->3->4->5->NULL and k = 2,
	return 4->5->1->2->3->NULL
	
	
	    public ListNode rotateRight(ListNode head, int k) {
	        if (head == null || k == 0) {
	            return head;
	        }
	        k = k % length(head);
	        if (k == 0) {
	            return head;
	        }
	        // fast 先走k 步
	        ListNode sentinel = new ListNode();
	        sentinel.next = head;
	        ListNode slow = sentinel;
	        ListNode fast = sentinel;
	        for (int i = 0; i < k; i++) {// k 肯定小于n
	            fast = fast.next;
	        }
	        
	        while (fast.next != null) {
	            slow = slow.next;
	            fast = fast.next;
	        }// 循环结束时 slow 指向目标元素(倒数第k 个元素的prev)，fast 指向 tail
	        ListNode newHead = slow.next;
	        slow.next = null;
	        fast.next = head;
	        return newHead;
	    }
	
	    private int length(ListNode head) {
	        int n = 0; 
	        ListNode p = head;
	        while (p != null) {
	            n++;
	            p = p.next;
	        }
	        return n;
	    }
    

### ReverseLinkedList2
Reverse a linked list from position m to n. Do it in-place and in one-pass.
For example:
	
		Given 1->2->3->4->5->NULL, m = 2 and n = 4,
		return 1->4->3->2->5->NULL. 


	    public ListNode reverseBetween(ListNode head, int m, int n) {
	        if (m == n) {
	            return head;
	        }
	        ListNode sentinel = new ListNode(0);
	        sentinel.next = head;
	        ListNode mp = sentinel;
	        ListNode np = sentinel;
	        for (int i = 0; i < m - 1; i++) {
	            mp = mp.next;
	        }// mP.next = oldHead
	        
	        for (int i = 0; i < n; i++) {
	            np = np.next;
	        }
	        ListNode oldHead = mp.next;
	        ListNode rest = np.next;
	        np.next = null;
	        mp.next = reverse(oldHead, null);
	        oldHead.next = rest;
	        return sentinel.next;
	    }
	    
	    private ListNode reverse(ListNode head, ListNode newHead) {
	        if (head == null) {
	            return newHead;
	        }
	        ListNode next = head.next;
	        head.next = newHead;
	        return reverse(next, head);
	    }


### ReverseLinkedList 链表反转

	
	    /**
	     * p为指向旧list的指针，p向右移动直到null
	     * newHead 指向新list 头部，newHead 一直向左
	     * 初始调用 
	     * Iterative(head, null)
	     * */
	    private ListNode reverseIterative(ListNode head, ListNode newHead) {
	        ListNode p = head;
	        while (p != null) {
	            ListNode next = p.next;
	            p.next = newHead;
	            newHead = p;
	            p = next;
	        }
	        return newHead;
	    }
	


### ReorderList
Given a singly linked list L: L0?L1?…?Ln-1?Ln,
reorder it to: L0?Ln?L1?Ln-1?L2?Ln-2?。
eg. Given {1,2,3,4}, reorder it to {1,4,2,3}

	
	    public void reorderList(ListNode head) {
	        if (head == null || head.next == null) {
	            return ;
	        }
	        reorder(head);
	    }
	
	    /**
	     * 找tail 和 tail的前一个元素，设置好3 个指针
	     * 递归处理子问题
	     * 以下边list为例
	     * 1->2->3->4->null => 1->4->2->3->null 
	     * 需要修改 1.next 3.next 4.next 3个指针
	     * */
	    private ListNode reorder(ListNode head) {
	        if (head.next == null) {
	            return head;
	        }
	        ListNode p = head;
	        ListNode prev = null;
	        while (p.next != null) {
	            prev = p;
	            p = p.next;
	        }// 循环结束后 p指向 tail，prev 指向 tail 的前一个元素
	        
	        if (head.next == p) {// base case：只有两个元素 直接返回
	            return head;
	        }
	        
	        ListNode hNext = head.next;
	        head.next = p;
	        
	        prev.next = null;
	        p.next = reorder(hNext);
	        return head;
	    }



### RemoveNthNodeFromEndofList


		 /**
	     * 两个指针：p2 先向前走n + 1步，p1 指向头，p1/p2 保持n 步间距
	     * 当 p2指向尾部时，p1 便是目标结点
	     * 
	     * 使用prev 指针比较方便删除操作，因此先new 一个start 结点，让 start.next = head
	     * */
	    public ListNode removeNthFromEnd(ListNode head, int n) {
	        ListNode start = new ListNode(0);
	        ListNode slow = start, fast = start;
	        slow.next = head;
	
	        //Move fast in front so that the gap between slow and fast becomes n
	        for (int i = 1; i <= n + 1; i++) {
	            fast = fast.next;
	        }
	        //Move fast to the end, maintaining the gap
	        while (fast != null) {
	            slow = slow.next;
	            fast = fast.next;
	        }
	        //Skip the desired node
	        slow.next = slow.next.next;
	        return start.next;
	    }
	


### RemoveDuplicatesfromSortedList2
给定已排序的链表，删除所有包含重复的元素，只留下 不同的元素

	Given 1->2->3->3->4->4->5, return 1->2->5.
	Given 1->1->1->2->3, return 2->3


	    public ListNode deleteDuplicates(ListNode head) {
	        // base case
	        if (head == null || head.next == null) {
	            return head;
	        }
	        if (head.val == head.next.val) {//1 1 2
	            return deleteDuplicates(findUnique(head));
	        } else {// 1 2 3
	            head.next = deleteDuplicates(head.next);
	            return head;
	        }
	    }
	
	    // 找第1 个与 head.val 值不同的元素 或者 null
	    private ListNode findUnique(ListNode head) {
	        ListNode next = head.next;// 1 1 5
	        while (next != null && next.val == head.val) {
	            next = next.next;
	        }
	        return next;
	    }


### RemoveDuplicatesfromSortedList


	    /**
	     * 一遍扫描，对每个p 将p.next 指向与 p.val 不相等的 下一个元素，然后 p右移
	     * */
	    public ListNode deleteDuplicates(ListNode head) {
	        if (head == null) {
	            return head;
	        }
	        for (ListNode p = head; p != null; p = p.next) {// 从p开始找下一个不等于p的元素 theNext
	            ListNode theNext = p.next;
	            while (theNext != null && theNext.val == p.val) {
	                theNext = theNext.next;
	            }
	            p.next = theNext;
	        }
	        
	        return head;
	    }



### PartitionList
给定链表和 值x，切分链表使得左半部分所有节点的值小于 x，而右半部分大于等于x
eg. Given 1->4->3->2->5->2 and x = 3,
return `1->2->2->4->3->5.`

* constraints: 保持原链表中元素的相对顺序。

* idea: 因为要保持原来的相对顺序，一遍遍历构造两个链表，s1 指向 小于x的结点，s2 指向大于等于x 的结点。遍历结束后
将 s1/s2 组合成一个。注意边界条件。

	    public ListNode partition(ListNode head, int x) {
	        if (head == null || head.next == null) {
	            return head;
	        }
	        
	        // 至少两个元素
	        ListNode s1 = new ListNode(0);// 指向小于x 的链表
	        ListNode s2 = new ListNode(0);
	        ListNode p1 = s1;
	        ListNode p2 = s2;
	        
	        for (ListNode p = head; p != null; p = p.next) {
	            if (p.val < x) {
	                p1.next = p;
	                p1 = p1.next;
	            } else {// p.val >= x
	                p2.next = p;
	                p2 = p2.next;
	            }
	        }
	        p2.next = null;// 注意这一行 特别容易忽略
	        
	        p1.next = s2.next;
	        return s1.next;
	    }


### MergeTwoSortedLists


### PalindromeLinkedList
判断一个单链表是不是回文

	    public boolean isPalindrome(ListNode head) {
	        ListNode copy = copyList(head);
	        ListNode reversed = reverse(copy);
	        
	        ListNode p1 = head;
	        ListNode p2 = reversed;
	        
	        while (p1 != null && p2 != null) {
	            if (p1.val != p2.val) {
	                return false;
	            }
	            p1 = p1.next;
	            p2 = p2.next;
	        }
	        return true;
	    }
	
	    private ListNode copyList(ListNode head) {
	        if (head == null) {
	            return null;
	        }
	        ListNode copyHead = new ListNode(head.val);
	        copyHead.next = copyList(head.next);
	        return copyHead;
	        
	    }
	
	    private ListNode reverse(ListNode head) {
	        
	        return recursive(head, null);
	    }
	
	    private ListNode recursive(ListNode head, ListNode newHead) {
	        if (head == null) {
	            return newHead;
	        }
	        ListNode tail = head.next;
	        head.next = newHead;
	        return recursive(tail, head);
	    }



### MergekSortedLists


		public ListNode mergeKLists(ListNode[] lists) {
	        if (lists.length == 0) {
	            return null;
	        }
	        // base case lists.length = 1 or 2
	        if (lists.length == 1) {
	            return lists[0];
	        }
	        // divide
	        int mid = lists.length / 2;
	        ListNode[] left = Arrays.<ListNode>copyOfRange(lists, 0, mid);
	        ListNode[] right = Arrays.<ListNode>copyOfRange(lists, mid, lists.length);
	        
	        // conquer & merge
	        return mergeTwoLists(mergeKLists(left), mergeKLists(right));
	    }
	    
	    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
	        if (l1 == null) {
	            return l2;
	        }
	        if (l2 == null) {
	            return l1;
	        }
	        if (l1.val <= l2.val) {
	            l1.next = mergeTwoLists(l1.next, l2);
	            return l1;
	        } else {
	            l2.next = mergeTwoLists(l1, l2.next);
	            return l2;
	        }
	    }
		


### LinkedListCycle2
找出 链表中环开始的结点，如果不存在环返回 null


	    /**
	     * http://www.cnblogs.com/hiddenfox/p/3408931.html
	     * cycle head 结点 x 的特点是有两个指针指向它，
	     * 画出一个 带环的链表，两条路径只可能是从 表头指向 x，从环指向 x
	     * slow/fast 指针肯定会在环中的某处相遇
	     * 
	     * */
	    public ListNode detectCycle(ListNode head) {
	        if (head == null) {
	            return head;
	        }
	        ListNode slow = head;
	        ListNode fast = head;
	
	        while (fast != null && fast.next != null) {
	            fast = fast.next.next;
	            slow = slow.next;
	
	            if (fast == slow) {
	                ListNode ph = head;
	                while (ph != slow) {
	                    slow = slow.next;
	                    ph = ph.next;
	                }
	                return slow;
	            }
	        }
	        return null;
	    }	



### LinkedListCycle
判断链表是否有环

	    public boolean hasCycle(ListNode head) {
	        if (head == null) {
	            return false;
	        }
	        ListNode slow = head;
	        ListNode fast = head.next;
	        while (slow != null && fast != null) {
	            slow = slow.next;
	            fast = fast.next;
	            if (fast != null) {
	                fast = fast.next;
	            }
	            if (slow == fast) {
	                return true;
	            }
	        }
	        return false;
	    }
	    



### IntersectionofTwoLinkedLists
找到两个链表相交的点。

* constraints1: 如果没有交点，返回 null
* 2: 链表需要维持原来的结构
* 3： 没有环
* 4：time = O(n), space = O(1)



	    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
	        int lenA = length(headA), lenB = length(headB);
	        // move headA and headB to the same start point
	        while (lenA > lenB) {
	            headA = headA.next;
	            lenA--;
	        }
	        while (lenA < lenB) {
	            headB = headB.next;
	            lenB--;
	        }
	        // find the intersection until end
	        while (headA != headB) {
	            headA = headA.next;
	            headB = headB.next;
	        }
	        return headA;
	    }


### CopyListwithRandomPointer
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.
Return a deep copy of the list.

	
	    public RandomListNode copyRandomListFromSolutions(RandomListNode head) {
	        if (head == null) return null;
	
	        Map<RandomListNode, RandomListNode> map = new HashMap<RandomListNode, RandomListNode>();
	
	        // loop 1. copy all the nodes
	        RandomListNode node = head;
	        while (node != null) {
	            map.put(node, new RandomListNode(node.label));
	            node = node.next;
	        }
	
	        // loop 2. assign next and random pointers
	        node = head;
	        while (node != null) {
	            map.get(node).next = map.get(node.next);
	            map.get(node).random = map.get(node.random);
	            node = node.next;
	        }
	
	        return map.get(head);
	    }



### AddTwoNumbers2
在 AddTwoNumbers 修改条件，最高位在 左边

* idea: 将输处链表反转再 使用 AddTwoNumbers 的解法。
* idea2: 使用两个 stack

	    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
	        Stack<Integer> s1 = new Stack<Integer>();
	        Stack<Integer> s2 = new Stack<Integer>();
	
	        while (l1 != null) {
	            s1.push(l1.val);
	            l1 = l1.next;
	        } ;
	        while (l2 != null) {
	            s2.push(l2.val);
	            l2 = l2.next;
	        }
	
	        int sum = 0;
	        ListNode list = new ListNode(0);
	        while (!s1.empty() || !s2.empty()) {
	            if (!s1.empty()) sum += s1.pop();
	            if (!s2.empty()) sum += s2.pop();
	            list.val = sum % 10;
	            ListNode head = new ListNode(sum / 10);
	            head.next = list;
	            list = head;
	            sum /= 10;
	        }
	
	        return list.val == 0 ? list.next : list;
	    }



### AddTwoNumbers
给定两个非空链表表示的非负整数，每个node 表示一个数位，将两个数相加当一个链表返回。

Input: `(2 -> 4 -> 3) + (5 -> 6 -> 4)`
Output: `7 -> 0 -> 8`


		public ListNode addTwoNumbers20170826(ListNode l1, ListNode l2) {
	        return add(l1, l2, 0);
	    }
	    
	    private ListNode add(ListNode l1, ListNode l2, int carry) {
	        if (l1 == null && l2 == null) {
	            return (carry == 0) ? null : new ListNode(carry);
	        }
	        
	        int sum = (l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + carry;
	        int v = sum % 10;
	        carry = sum >= 10 ? 1 : 0;
	        ListNode node = new ListNode(v);
	        node.next = add(l1 == null ? null : l1.next, l2 == null ? null : l2.next, carry);
	        return node;
	    }




## 动态规划

### HouseRobber2
在 HouseRobber 的基础上增加限制条件：所有的房子连成了一个圈
* idea: 解开环，分别考虑选头，选尾的情况，算两遍取最大值即可




### HouseRobber
给定非负整数数组， 取不相邻的元素使其和最大。求和的最大值


	    /**
	     * 最后一次rob 要么是 idx n-1, 要么是 n-2
	     * 设m[i] 为位置i 对应的最大值
	     * m[i] = max {m[i-2] + nums[i],
	     *                  m[i-1]}
	     * base case:
	     *         idx = 0: nums[0]
	     *         idx = 1: max{nums[0], nums[1]}
	     * */
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int[] m = new int[nums.length];
        for (int i = 0; i<m.length; i++) {
            m[i] = -1;
        }
        m[0] = nums[0];
        //m[i] 为前i个元素对应的 目标值
        //expect m[nums.length]
        dp(nums, nums.length - 1, m);
        return m[nums.length - 1];
    }

    private int dp(int[] nums, int idx, int[] m) {
        //base case
        if (idx == 0) {
            m[0] = nums[0];
            return nums[0];
        }
        if (idx == 1) {
            m[1] = Math.max(nums[0], nums[1]);
            return m[1];
        }
        // cache
        if (m[idx] != -1) {
            return m[idx];
        }
        int r = Math.max(dp(nums, idx-2, m) + nums[idx], // m[i-2] + nums[i] 
                dp(nums, idx-1, m));// m[i-1]
        m[idx] = r;
        return m[idx];
        
    }



### UniquePaths
给定 m * n 的矩阵，求从左上角走到右下角有多少条路径。每一步可以向右或向下移动。

    
    public int uniquePaths20170826Accepted(int m, int n) {
        int[][] cache = new int[m][n];
        return dp(m - 1, n - 1, cache);
    }
    
    private int dp(int m, int n, int[][] cache) {
        if (m == 0 || n == 0) {// 第0 行，只有一条路径
            cache[m][n] = 1;
            return 1;
        }
        if (cache[m][n] != 0) {
            return cache[m][n];
        }
        
        int left = dp(m, n - 1, cache);
        int up = dp(m - 1, n, cache);
        cache[m][n] = left + up;
        return cache[m][n];
    }



### UniqueBinarySearchTrees
Given n, how many structurally unique BST's that store values 1...n? n = 3, 5种结构不同的 bst，分别
画出以1 2 3 为根的bst。确定根元素后，number of BST = 左子树个数 * 右子树个数


		    public int numTrees20170825Accepted(int n) {
		        int[] cache = new int[n + 1];
		        dp(n, cache);
		        return cache[n];
		    }
		    
		    private int dp(int n, int[] cache) {
		        if (n == 0) {
		            cache[n] = 1;
		            return 1;
		        }
		        if (cache[n] != 0) {
		            return cache[n];
		        }
		        int ways = 0;
		        for (int i = 1; i <= n; i++) {// 以i 为根
		            ways += dp(i-1, cache) * dp(n - i, cache);
		        }
		        cache[n] = ways;
		        return ways;
		    }

### WordBreak2
给定字符串s和字典 dict，求所有可能的句子，`eg. s = catsanddog, dict = ["cat", "cats", "and", "sand", "dog"]`


	
	    /**
	     * 思路：backtrack
	     * 每层尝试从 s 中吃掉一个 dict里的单词，直到s 为empty，输出一个 sentence
	     * */
	    public List<String> wordBreak(String s, List<String> dict) {
	        return backtrack(s, dict);
	    }
	
	    private List<String> backtrack(String s, List<String> dict) {
	        // from cache
	        if (cache.containsKey(s)) {
	            return cache.get(s);
	        }
	        // base case: s == empty
	        LinkedList<String> rs = new LinkedList<String>();
	        if ("".equals(s)) {
	            rs.add("");
	            return rs;
	        }
	        // backtrack
	        for (String word : dict) {
	            if (!s.startsWith(word)) {
	                continue;
	            }
	            int idx = word.length();// catsanddog
	            String sub = s.substring(idx);// sanddog
	            List<String> subRs = backtrack(sub, dict);
	            for (String e : subRs) {
	                rs.add(word + (e.isEmpty() ? "" : " ") + e);
	            }
	        }
	        cache.put(s, rs);
	        return rs;
	    }


### WordBreak
给定字符串s 和 字典dict，判断s 是否可以被dict 里的词分隔
eg. dict = {leet, code, apple}。
s = leetcode, return true 。
s = successprogrammer return false。

* idea: 交叉子问题暗示用dp 解法，考虑一个子问题，对 dict中的单词word，只要 s.endsWith(word) 且 s-word 也可以分割，那
s 就是可以被分割的。递归式为 dp(s) = e in dict && s.endsWith(e) && dp(s-e)，加入 memoization 当缓存，直接根据递归式写代码即可。

	
	    private boolean dp2(String s, List<String> dict, int n, Boolean[] t) {
	        if (n == 0 || "".equals(s)) {
	            t[0] = true;
	            return true;
	        }
	        
	        if (t[n] != null) {
	            return t[n];
	        }
	
	        boolean r = false;
	        for (String word : dict) {
	            if (s.endsWith(word)) {
	                int eIdx = s.length() - word.length();
	                String sub = s.substring(0, eIdx);
	                r = dp2(sub, dict, sub.length(), t);
	                if (r) {
	                    break;
	                }
	            }
	        }
	        t[n] = r;
	        return r;
	    }


### Triangle
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

	  [
	     [2],
	    [3,4],
	   [6,5,7],
	  [4,1,8,3]
	  ]


The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11). 函数原型为
`public int minimumTotal(List<List<Integer>> triangle) {}`

* idea: 求最后一行每一列对应的 sum，再从 中取出最小值。很容易得出递归式 dp(row, column) = min(dp(row-1, column-1), dp(row-1, column)) + line.get(column)
可以在 O(n) 时间求解 。

递归式比较简单，可以考虑直接写出迭代代码



	    public int minimumTotal20170824(List<List<Integer>> triangle) {
	        int row = triangle.size();
	        
	        for (int i = 1; i < row; i++) {
	            List<Integer> prev = triangle.get(i-1);
	            List<Integer> line = triangle.get(i);
	            
	            for (int c = 0; c < line.size(); c++) {
	                Integer e = line.get(c);
	                if (c == 0) {
	                    line.set(c, e + prev.get(0));
	                } else if (c == line.size() - 1) {
	                    line.set(c, e + prev.get(prev.size()-1));
	                } else {
	                    Integer left = prev.get(c - 1);
	                    Integer right = prev.get(c);
	                    line.set(c, e + Math.min(left, right));
	                }
	            }
	        }
	        
	        List<Integer> last = triangle.get(row - 1);
	        int min = Integer.MAX_VALUE;
	        for (Integer e : last) {
	            min = Math.min(min, e);
	        }
	        return min;
	    }



### TargetSum
给定非负整数构成的数组 a[n]和 target，假设有两种符号 + -，判断有多少种分配符号的方法 使得 +/-a1 +/-a2... +/-an = target


   
	    private int dp(int[] nums, int target, int i, Map<Pair, Integer> dp) {
	        Pair key = new Pair(i, target);
	        //base case
	        if (i == 0) {
	            int ways = (nums[0] == target) ? 1 : 0;
	            ways += (nums[0] == -target) ? 1 : 0;
	            dp.put(key, ways);
	            return ways;
	        }
	        //cache
	        if (dp.get(key) != null) {
	            return dp.get(key);
	        }
	        int ways = dp(nums, target - nums[i], i - 1, dp) + dp(nums, target + nums[i], i - 1, dp);
	        dp.put(key, ways);
	        return ways;
	    }





### PerfectSquares
给出一个正整数n，求至少需要多少个完全平方数（例如1，4，9，16……）相加能得到n。 
例如，n = 12，返回3，因为12 = 4 + 4 + 4。n = 13，返回2，因为13 = 4 + 9


	    private int dp(int x, int[] A, int[] m) {
	        if (x == 0) {
	            m[0] = 0;
	            return 0;
	        }
	        if (m[x] != -1) {
	            return m[x];
	        }
	        int min = Integer.MAX_VALUE;
	        for (int i = A.length - 1; i >= 0; i--) {
	            int rest = x - A[i];
	            if (rest >= 0) {
	                min = Math.min(min, dp(rest, A, m));
	            }
	        }
	        m[x] = min + 1;
	        return m[x];
	    }	
	
 



### PartitionEqualSubsetSum
给定一个数组 nums，判断nums 是否能被分成两个子集s1 & s2 使得 sum(s1) = sum(s2)

* idea: 1. 如果 sum(nums) 为odd 则肯定不可以 直接返回 false
* 2. 问题可以转化为 能否从nums 里找若干个元素。使得 sum(x1, x2, ... xk) = sum(nums)/2。
base case：target=0=>true, target<0=> false


	    private boolean dp(int[] nums, int i, int target, Boolean[][] t) {
	        if (target == 0) {
	            return true;
	        }
	        if (target < 0) {
	            return false;
	        }
	        if (i < 0) {
	            return false;
	        }
	        if (t[i][target] != null) {
	            return t[i][target]; 
	        }

	        boolean answer = dp(nums, i - 1, target - nums[i], t)// 取nums[i]
	                || dp(nums, i - 1, target, t);//不取
	        
	        t[i][target] = answer;
	        return t[i][target];
	    }




### MaximumSubarray
给定一个未排序的数组，求它的连续子数组的和的最大值。
For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6

    	public int maxSubArray(int[] nums) {}

* idea: 很明显 最大和对应的子数组不只一个，题要求最优解对应的值暗示着dp 解法。
* 1）最优子结构，设 dp(num, i) 为以i 结尾的子数组对应的目标值，那么 
dp(num, i) = (dp(num, i-1) > 0 ? dp(num, i-1) : 0) + num[i]，
因为递归式简单，考虑直接bottom-up 求值。

	    public int maxSubArray(int[] nums) {
	        int n = nums.length;
	        int[] maxSum = new int[n];//maxSum[i] means the maximum subarray ending with A[i];
	        maxSum[0] = nums[0];// at least one number
	        int max = maxSum[0];
	
	        for (int i = 1; i < n; i++) {
	            maxSum[i] = nums[i] + (maxSum[i - 1] > 0 ? maxSum[i - 1] : 0);
	            max = Math.max(max, maxSum[i]);
	        }
	
	        return max;
	    } 


### LongestIncreasingSubsequence
给定未排序的整数数组，找出最长升序子序列的长度。
`eg. [10, 9, 2, 5, 3, 7, 101, 18], The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4`

* idea: 数组只有一个索引值是变量，设 `lis[i]` 为 index = i 结尾的子数组对应的目标值，考虑怎样根据子问题的解 `lis[k], k in [0.. i-1]`
求 `lis[i]`。`lis[i] = (num[i] > num[k]) ? max(lis[k] + 1) : 1`。例如 求 lis[7] 时，因为 n[7]=18 > n[5]=7，
lis[7] 只需在 lis[5] 的基础上加1即可，因为[.. 7, 18] 构成一个更长的升序子序列。而 n[7] < n[6] 表示 [.. 101, 18] 不能构成一个
更长的升序子序列。

        int length = 1;
        for (int i = 0; i < n; i++) {
            int prev = dpRecursive(nums, i, cache) + 1;
            if (nums[i] < nums[n]) {
                length = Math.max(length, prev);
            }
        }

该解法时间复杂度为 O(n^2)。注意最后 要遍历 lis[0..n] 取最大值。


### DecodeWays
给定字符映射 mapping={A->1, B->2, ... Z-> 26}, 和数字msg，求解码方法数。eg. msg = 12，可以解码成 L 或者 AB，解码方法数是 2

	
	    public int numDecodings(String s) {
	        if (s == null || s.length() == 0) {
	            return 0;
	        }
	        int[] dw = new int[s.length() + 1];// dw 表示前n个字符 对应的解码数
	        for (int i = 0; i < dw.length; i++) {
	            dw[i] = -1;
	        }
	        dw[0] = 1;
	        // target = dw[s.length]
	        dp(s, s.length(), dw);
	        
	        return dw[s.length()];
	    }
	
	    private int dp(String msg, int length, int[] dw) {
	        //base case
	        if ("".equals(msg) || length == 0) {
	            return 1; 
	        }
	        
	        // cache
	        if (dw[length] != -1) {
	            return dw[length];
	        }
	        
	        int numOfDecodings = 0;
	        for (int k = 1; k <= 26; k++) {
	            String v = String.valueOf(k);
	            if (msg.endsWith(v)) {
	                int endIdx = msg.length() - v.length();
	                String restMsg = msg.substring(0, endIdx);
	                numOfDecodings += dp(restMsg, restMsg.length(), dw);
	            }
	        }
	        dw[length] = numOfDecodings;
	        return dw[length];
	    }



### CoinChangeMin
给定不同的硬币面值coins，和一个总数amount。 
求 最少的硬币数使得 coins[1] + coins[2] + .. + coins[k] = amount。 
例如 coins = [1, 2, 5], amount = 11,  11 = 5 + 5 + 1, so r = 3



    public void dfs(int[] coins, int index, int amount, int count) {
        //base case: 面值最小的硬币
        if (index == -1) {
            return;
        }
        // 从面值最大的开始考虑
        int number = amount / coins[index];// number 表示面值最大的coin 最多能取几个
        for (int i = number; i >= 0; i--) {
            int remain = amount - coins[index] * i;
            int newcount = count + i;// newcount: 取i个面值最大的coin 对应的硬币数
            if (remain > 0 && newcount < curMin) {
                dfs(coins, index - 1, remain, newcount);
            } else if (newcount < curMin) {
                curMin = newcount;
                return ;
            } else {
                break;
            }
        }
        
    }



### CoinChange
给定值n和 若干不同面值的硬币(数量无限制) S = {S1, S2, ... Sk}，
求不同找零数量的方案数，例如 n = 4, S = {1, 2, 3} ,solutions = {1,1,1,1},{1,1,2},{2,2},{1,3}。方案数是 4

* idea1: 按Sk 取与不取划分子问题： count(S, k, n) = count(S, k, n-Sk) + count(S, k-1, n)
* idea2: 按 Sk 最多能取多少个划分子问题： 


		int k = amount / coins[j];// k = coins[j] 可以取的最多数量
        int sum = 0;
        for (int i = k; i >= 0; i--) {
            sum += coinChangeAux(coins, j-1, amount - i*coins[j], cache);
        } 

* base case 都是 n=0 时 count = 1，只有一种方案。


### ClimbingStairs
n 个台阶的楼梯，每次可以登 1步或者2步，爬上顶部有多少种不同的方法

    public int climbStairs20170821(int n) {
        int[] ways = new int[n + 1];
        dp(n, ways);
        return ways[n];
    }

    private int dp(int n, int[] ways) {
        if (n == 0 || n == 1) {
            ways[n] = 1;
            return 1;
        }
        if (ways[n] != 0) {
            return ways[n];
        }
        ways[n] = dp(n - 1, ways) + dp(n - 2, ways);
        return ways[n];
    }




### BestTimetoBuyandSellStock2
可以执行多次交易，求最大可能的利润


    public int maxProfit(int[] prices) {
        int profit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) {
                profit += prices[i] - prices[i - 1]; 
            }
        }
        return profit;
    }



### BestTimetoBuyandSellStock
给定数组 num = [7, 1, 5, 3, 6, 4]，num[i] 为第i 天某个股票的价格

* 只能做一次买和一次卖，求最大可能的利润
* 如上例 max profit = 6 - 1 = 5，不是7-1 因为 必须先买后卖



	    public int maxProfit(int[] prices) {
	        if (prices.length == 0) {
	            return 0;
	        }
	        int mProfit = 0;
	        int sofarMin = prices[0];
	        Pair p = new Pair(-1, -1);
	        int buy = -1; int sell = -1;
	        for (int i = 0; i < prices.length; ++i) {
	            if (prices[i] < sofarMin) {// 当日价格比 min 还小，更新min，记录买入天
	                sofarMin = prices[i];
	                buy = i;
	            } else {
	                int profit = prices[i] - sofarMin;
	                if (profit > mProfit) {// 更新最大利润，保存 buy/sell Pair
	                    mProfit = profit;
	                    sell = i;
	                    p.buy = buy;
	                    p.sell = sell;
	                }
	            }
	        }
	        // 此时 p.buy/p.sell 对应买入和卖出的天数
	        return mProfit;
	    }



## DIVIDE & CONQUER


### SearchA2DMatrix
	
	[
	  [1,   4,  7, 11, 15],
	  [2,   5,  8, 12, 19],
	  [3,   6,  9, 16, 22],
	  [10, 13, 14, 17, 24],
	  [18, 21, 23, 26, 30]
	]

	
	public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null) {
            return false;
        }
        int row = 0;
        int column = matrix[0].length - 1;
        while ( row <= matrix.length - 1 && column >= 0) {
            int cell = matrix[row][column];
            if (target == cell) {
                return true;
            } else if (target < cell) {
                column -= 1;
            } else {
                row += 1;
            }
        }
        return false;
    }


### MajorityElement
The majority element is the element that appears more than ⌊ n/2 ⌋ times

		public int majorityElement1(int[] nums) {
	 	   Arrays.sort(nums);
	 	   return nums[nums.length/2];
		}


### KthLargestElementinanArray


	private int find(int[] nums, int left, int right, int i) {
        if (left == right) {
            return nums[left];
        }
        int p = randomPartition(nums, left, right);// p = num of smaller elements
        if (p + 1 == i) {// pivot为目标元素
            return nums[p];
        } else if (i < p) {
            return find(nums, left, p - 1, i);
        } else {// k > p
            return find(nums, p + 1, right, i - p - 1);
        }
    }



	    /**
    	 * 从nums 里随机选择一个元素当pivot 
    	 * 将nums 划分成 小于pivot | pivot | 大于pivot 3部分
    	 * 返回pivot 索引值
    	 * */
    public int randomPartition(int[] nums, int a, int b) {
        Random random = new Random();
        int r = Math.abs(random.nextInt()) % (b - a + 1) + a;
        int pivot = nums[r];
        swap(nums, r, nums.length - 1);
        
        int last = a - 1;
        for (int i = a; i < b; i++) {
            if (nums[i] < pivot) {
                swap(nums, ++last, i);
            }
        }
        last += 1;
        swap(nums, last, nums.length - 1);
        return last;
    }




### ExpressionAddOperators
给定一个仅包含数字0-9的字符串，和一个目标值 target，返回所有可能的加操作符方式 使得表达式的值为 target
	

	"123", 6 -> ["1+2+3", "1*2*3"] 
	"232", 8 -> ["2*3+2", "2+3*2"]
	"105", 5 -> ["1*0+5","10-5"]
	"00", 0 -> ["0+0", "0-0", "0*0"]
	"3456237490", 9191 -> []

没搞定

### DifferentWaystoAddParentheses
给定由数字和操作符组成的 string，oprt和数字有不同的加括号形式，返回所有可能的计算结果。
eg. `Input: "2*3-4*5"`

	(2*(3-(4*5))) = -34
	((2*3)-(4*5)) = -14
	((2*(3-4))*5) = -10
	(2*((3-4)*5)) = -10
	(((2*3)-4)*5) = 10
	Output: [-34, -14, -10, -10, 10]

	函数原型为 public List<Integer> diffWaysToCompute(String input) {}

## DESIGN


### LFUCache
http://bookshadow.com/weblog/2016/11/22/leetcode-lfu-cache/



### LRUCache

			public class LRUCache {
			    private static final float hashTableLoadFactor = 0.75f;
			    private LinkedHashMap<Integer, Integer> map;
			    private int cacheSize;
			    
			    public LRUCache(int capacity) {
			        this.cacheSize = capacity;
			        int hashTableCapacity = (int) Math.ceil(cacheSize / hashTableLoadFactor) + 1;
			        map = new LinkedHashMap<Integer, Integer>(hashTableCapacity, hashTableLoadFactor, true) {
			            // (an anonymous inner class)  
			            private static final long serialVersionUID = 1;
			
			            @Override
			            protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
			                return size() > LRUCache.this.cacheSize;
			            }
			        };
			    }
			    public int get(int key) {
			        return map.getOrDefault(key, -1);
			    }
			    public void put(int key, int value) {
			        map.put(key, value);
			    }
			}



## 深度优先搜索DFS


### BinaryTreeMaximumPathSum
Given a binary tree, find the maximum path sum.

* idea: 按路径是否经过root 分两种情况，1不经过root，此时 最大路径和为root到左子树或右子树某个结点的和 max(left.singlePath, right.singlePath)，
2 经过root，此时 目标值为 left.singlePath + right.singlePath + root.val，这两种情况的较大值即为目标值。dfs 求 这两个值。



	    public int maxPathSum(TreeNode root) {
	        return dfs(root).maxPath;
	    }
	
	    private ResultType dfs(TreeNode root) {
	        if (root == null) {
	            return new ResultType(0, Integer.MIN_VALUE);
	        }
	        // Divide
	        ResultType left = dfs(root.left);
	        ResultType right = dfs(root.right);
	        
	        // Conquer
	        int singlePath = Math.max(left.singlePath, right.singlePath) + root.val;
	        singlePath = Math.max(singlePath, 0);
	        
	        int maxPath = Math.max(left.maxPath, right.maxPath);
	        maxPath = Math.max(maxPath, left.singlePath + right.singlePath + root.val);
	
	        return new ResultType(singlePath, maxPath);
	    }
	
	    
	    class ResultType {
	        // singlePath: 从root往下走到任意点的最大路径，这条路径可以不包含任何点
	        // maxPath: 从树中任意到任意点的最大路径，这条路径至少包含一个点
	        int singlePath;
	        int maxPath;
	        public ResultType(int singlePath, int maxPath) {
	            this.singlePath = singlePath;
	            this.maxPath = maxPath;
	        }
	    }



### SumRoottoLeafNumbers
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.
An example is the root-to-leaf path 1->2->3 which represents the number 123.
Find the total sum of all root-to-leaf numbers.
	

		     1
	   		/ \
		   2   3	

Return the sum = 12 + 13 = 25

### SameTree
Given two binary trees, write a function to check if they are equal or not.
equal: 结构相同且 每个结点的值相等


### PathSum2
在 PathSum 基础上 找出所有 路径


### PathSum
给定二叉树和 sum, 判断 binary tree 里是否有一条 root-to-leaf 路径，每个结点之和等于sum

Given the below binary tree and sum = 22


              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22


	    private void dfs(TreeNode root, int pathSum, int target, List<Boolean> rs) {
	        if (root.left == null && root.right == null) {
	            if (pathSum + root.val == target) {
	                rs.add(Boolean.TRUE);
	            } else {
	                rs.add(Boolean.FALSE);
	            }
	            return ;
	        }
	        if (root.left != null) {
	            dfs(root.left, pathSum + root.val, target, rs);
	        }
	        if (root.right != null) {
	            dfs(root.right, pathSum + root.val, target, rs);
	        }
	    }

### NumberofIslands
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands.
This method approaches the problem as one of depth-first connected components search

	11000
	11000
	00100
	00011
	answer = 3


	11110
	11010
	11000
	00000
	answer = 1

函数原型为

		    public int numIslands(char[][] grid) {}


    public int numIslands(char[][] grid) {
        // Store the given grid
        // This prevents having to make copies during recursion
        g = grid;
        // Our count to return
        int c = 0;
        // Dimensions of the given graph
        y = g.length;
        if (y == 0) return 0;
        x = g[0].length;
        // Iterate over the entire given grid
        for (int i = 0; i < y; i++) {
            for (int j = 0; j < x; j++) {
                if (g[i][j] == '1') {
                    dfs(i, j);
                    c++;
                }
            }
        }
        return c;
    }


    private void dfs(int i, int j) {
        // Check for invalid indices and for sites that aren't land
        if (i < 0 || i >= y || j < 0 || j >= x || g[i][j] != '1') return;
        // Mark the site as visited
        g[i][j] = '0';
        // Check all adjacent sites
        dfs(i + 1, j);
        dfs(i - 1, j);
        dfs(i, j + 1);
        dfs(i, j - 1);
    }

    int y; // The height of the given grid
    int x; // The width of the given grid
    char[][] g; // The given grid, stored to reduce recursion memory usage
    


### MaximumDepthofBinaryTree
Given a binary tree, find its maximum depth. The maximum depth is the 
number of nodes along the longest path from the root node down to the farthest leaf node.

### FlattenBinaryTreetoLinkedList
将 二叉树 扁平化，right 指针当链表的 next 指针。

	     1
        / \
       2   5
      / \   \
     3   4   6


    1
     \
      2
       \
        3
         \
          4
           \
            5
             \
              6


* idea： 代码很简单，关键是要能理解。根据题意：原左子树置为右子树，新左子树置为 null。
因此先处理右子树，保存最后处理的结点(左子树根结点)到 prev，再处理左子树(此时已经将左子树与右子树的关系设置好)，再将 root.right 置为 prev。


	    public void flatten(TreeNode root) {
	        if (root == null) {
	            return;
	        }
	        flatten(root.right);
	        flatten(root.left);
	        root.right = prev;
	        root.left = null;
	        prev = root;
	    }



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



### KthSmallestElementinaBST
bst 中找第k小的元素


	    int kth = 0;
	    int visited = 0;
	    public int kthSmallest(TreeNode root, int k) {
	        inorder(root, k);
	        return kth;
	    }
	    
	    private void inorder(TreeNode root, int k) {
	        if (root == null) {
	            return ;
	        }
	        inorder(root.left, k);
	        visited += 1;
	        if (k == visited) {
	            kth = root.val;
	            return ;
	        }
	        inorder(root.right, k);
	    }


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


	    private void traverse(TreeNode root, List<Integer> list) {
	        if (root == null) return;
	        traverse(root.left, list);
	        if (prev != null) {
	            if (root.val == prev) {
	                count++;
	            } else {
	                count = 1;
	            }
	        }
	        if (count > max) {
	            max = count;
	            list.clear();
	            list.add(root.val);
	        } else if (count == max) {
	            list.add(root.val);
	        }
	        prev = root.val;
	        traverse(root.right, list);
	    }
	
	    Integer prev = null;
	    int count = 1;
	    int max = 0;




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


	    private int find(int[] nums, int low, int high) {
	        int len = high - low + 1;
	        if (len == 1) {
	            return low;
	        }
	        if (len == 2) {// 这样写保证后续 nums 至少有3个元素 mid-1 不会越界
	            return nums[low] > nums[high] ? low : high;
	        }
	        int mid = low + (high - low) / 2;
	        if (nums[mid] > nums[mid-1] && nums[mid] > nums[mid + 1]) {
	            return mid;
	        } else if (nums[mid] > nums[mid-1] ) {// 从右边部分找
	            return find(nums, mid + 1, high);
	        } else {
	            return find(nums, low, high - 1);
	        }
	        
	    }


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

### BattleshipsinaBoard
给定二维数组，计算其中 battleship 个数。battleship 由 x 表示，空位用. 表示，约束条件如下
battleship 占用 1*n 或者 n*1 个位置，n 的大小可变。至少有一个 空位分隔不同的 battleship

* constraints: 因为 至少有一个 空位分隔不同的 battleship 这个约束的存在，本题变得很简单。
* idea: 计算 每个battleship 的 左上角X 的个数即可，需要满足 X 的左边和上方都不是 X


	    /**
	     * 根据约束条件 空位分隔不同的 battleship，只需数一下 左边，上边为. 的 x 个数即可
	     *  
	     * */
	    public int countBattleships(char[][] board) {
	        int r = 0;
	        for (int i = 0; i < board.length; i++) {
	            for (int j = 0; j < board[0].length; j++) {
	                char cell = board[i][j];
	                if (j - 1 >= 0 && board[i][j - 1] == 'X') {
	                    continue;
	                }
	                if (i - 1 >= 0 && board[i - 1][j] == 'X') {
	                    continue;
	                }
	                if (cell == '.') {
	                    continue;
	                }
	                r += 1;
	            }
	        }
	        return r;
	    } 


### WordSearch2
在 WordSearch 的基础上，给定包含多个单词的dict ，找出 dict 里所有出现在 board里的单词


	    public List<String> findWords(char[][] board, String[] words) {
	        List<String> res = new ArrayList<>();
	        TrieNode root = buildTrie(words);
	        for (int i = 0; i < board.length; i++) {
	            for (int j = 0; j < board[0].length; j++) {
	                dfs(board, i, j, root, res);
	            }
	        }
	        return res;
	    }
	
	    public void dfs(char[][] board, int i, int j, TrieNode p, List<String> res) {
	        char c = board[i][j];
	        if (c == '#' || p.next[c - 'a'] == null) {
	            return;
	        }
	        p = p.next[c - 'a'];
	        if (p.word != null) {   // found one
	            res.add(p.word);
	            p.word = null;     // de-duplicate
	        }
	
	        board[i][j] = '#';
	        if (i > 0) dfs(board, i - 1, j ,p, res); 
	        if (j > 0) dfs(board, i, j - 1, p, res);
	        if (i < board.length - 1) dfs(board, i + 1, j, p, res); 
	        if (j < board[0].length - 1) dfs(board, i, j + 1, p, res); 
	        board[i][j] = c;
	    }
	
	    public TrieNode buildTrie(String[] words) {
	        TrieNode root = new TrieNode();
	        for (String w : words) {
	            TrieNode p = root;
	            for (char c : w.toCharArray()) {
	                int i = c - 'a';
	                if (p.next[i] == null) {
	                    p.next[i] = new TrieNode();
	                }
	                p = p.next[i];
	           }
	           p.word = w;
	        }
	        return root;
	    }
	    
	    class TrieNode {
	        TrieNode[] next = new TrieNode[26];
	        String word;
	    }


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


	    public boolean exist(char[][] board, String word) {
	        int row = board.length;
	        int column = board[0].length;
	        boolean exist = false;
	        boolean[][] visited = new boolean[row][column];
	        for (int i = 0; i < row; i++) {
	            for (int j = 0; j < column; j++) {
	                exist = dfs(board, word, 0, i, j, visited);
	                if (exist) {
	                    return true;
	                }
	            }
	        }
	        return exist;
	    }
	
	    private boolean dfs(char[][] board, String word, int level, int i, int j, boolean[][] visited) {
	        // base case0: out of range
	        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) {
	            return false;
	        }
	        if (visited[i][j]) {
	            return false;
	        }
	        // base case1: board[i][j] != word[level] 
	        if (board[i][j] != word.charAt(level)) {
	            return false;
	        }
	        
	        // base case2: level == word.length - 1
	        if (level == word.length() - 1) {
	            return true;
	        }
	        visited[i][j] = true;
	        boolean r = false;
	        r = dfs(board, word, level + 1, i - 1, j, visited) //up
	                || dfs(board, word, level + 1, i, j + 1, visited)// right
	                || dfs(board, word, level + 1, i + 1, j, visited)// down
	                || dfs(board, word, level + 1, i, j - 1, visited);
	        visited[i][j] = false;
	        return r;
	    }	



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

### Permutations2



输入数组里可以有重复元素


    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> r = new ArrayList<List<Integer>>();
        if (nums == null || nums.length == 0) {
            return r;
        }
        boolean[] used = new boolean[nums.length];
        List<Integer> list = new ArrayList<Integer>();
        Arrays.sort(nums);
        dfs(nums, used, list, r);
        return r;
    }
    
    public void dfs(int[] nums, boolean[] used, List<Integer> path, List<List<Integer>> r) {
        if (path.size() == nums.length) {
            r.add(new ArrayList<Integer>(path));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) {
                continue;
            }
            if (i > 0 && nums[i - 1] == nums[i] && !used[i - 1]) {
                continue;
            }
            used[i] = true;
            path.add(nums[i]);
            dfs(nums, used, path, r);
            used[i] = false;
            path.remove(path.size() - 1);
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

### MergeIntervals
Given a collection of intervals, merge all overlapping intervals.
Given [1,3],[2,6],[8,10],[15,18],
return [1,6],[8,10],[15,18]


    public List<Interval> merge(List<Interval> intervals) {
        if (intervals.size() <= 1) return intervals;

        // Sort by ascending starting point using an anonymous Comparator
        intervals.sort((i1, i2) -> Integer.compare(i1.start, i2.start));

        List<Interval> result = new LinkedList<Interval>();
        int start = intervals.get(0).start;
        int end = intervals.get(0).end;

        for (Interval interval : intervals) {
            if (interval.start <= end) // Overlapping intervals, move the end if needed
                end = Math.max(end, interval.end);
            else { // Disjoint intervals, add the previous one and reset bounds
                result.add(new Interval(start, end));
                start = interval.start;
                end = interval.end;
            }
        }

        // Add the last interval
        result.add(new Interval(start, end));
        return result;
    }

	

### InsertInterval
给定一组不相交的 interval list，将一个新的 interval 插入 list中
有必要的话 merge 使得 list 中的 interval 都是不相交的。
假设 输出参数 list 已经按 interval.start 排好序。
eg. Given intervals `[1,3],[6,9]`, `insert and merge [2,5] in as [1,5],[6,9]`


    public List<Interval> insert0912Accepted(List<Interval> intervals, Interval newInterval) {
        if (intervals.size() == 0) {
            intervals.add(newInterval);
            return intervals;
        }
        for (int i = 0; i < intervals.size(); i++) {
            Interval current = intervals.get(i);
            if (current.start > newInterval.end) {
                break;
            }
            
            if (!nonOverlap(current, newInterval)) {// 相交求新 interval
                newInterval.start = Math.min(current.start, newInterval.start);
                newInterval.end = Math.max(current.end, newInterval.end);
            }
        }
        List<Interval> r = new ArrayList<Interval>();
        for (int i = 0; i < intervals.size(); i++) {
            Interval current = intervals.get(i);
            if (nonOverlap(current, newInterval)) {
                if (current.start > newInterval.end 
                        && !r.contains(newInterval)) {
                    r.add(newInterval);
                }
                r.add(current);
            }
        }
        if (!r.contains(newInterval)) {//for 循环里可以处理 左边界情况，右边界需要另处理
            r.add(newInterval);
        }
        return r;
    }
    
    public boolean nonOverlap(Interval a, Interval b) {
        return a.end < b.start || b.end < a.start;
    }



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
Given a matrix of m x n elements，以螺旋形式返回 矩阵里的元素。函数原型为

	public List<Integer> spiralOrder(int[][] matrix) {}


* idea: 按层输出，根据level 和 matrix 大小计算左上角/右下角 坐标，分别输出->, down, <-, up
4 个方向的元素。然后以level + 1 递归调用。
`private void spiral2(int[][] matrix, int level, List<Integer> list, int size) {}`
初始调用为 `spiral2(matrix, 0, list, m*n);`

base case1: rList.size == m*n, base case2: 一行，输出该行return，base case3: 一列，输出该列return。


		private void spiral2(int[][] matrix, int level, List<Integer> list, int size) {
	        //base case1
	        if (list.size() == size) {
	            return ;
	        }
	        int a = level; int b = level;
	        int x = matrix.length - level - 1;
	        int y = matrix[0].length -level - 1;
	        //base case2: 一行
	        if (a == x) {
	            for (int column = b; column <= y; column++) {//->
	                list.add(matrix[a][column]);
	            }
	            return ;
	        }
	        //base case3: 一列
	        if (b == y) {
	            for (int row = a; row <= x; row++) {
	                list.add(matrix[row][b]);
	            }
	            return ;
	        }
	        int row = a; int column = b;
	        for (; column <= y; column++) {//->
	            list.add(matrix[row][column]);
	        }
	        column--;
	        for (row = a + 1; row <= x; row++) {//down
	            list.add(matrix[row][column]);
	        }
	        row--;
	        for (column = y - 1; column >= b; column--) {// <-
	            list.add(matrix[row][column]);
	        }
	        column++;
	        for (row = x - 1; row > a; row--) {//up
	            list.add(matrix[row][column]);
	        }
	        spiral2(matrix, level + 1, list, size);
	    }


### ThirdMaximumNumber
给定非空数组，输出第3大的数。如果不存在输出最大的元素。
如果存在重复，输出数值是第3大的数。例如 [2, 2, 3, 1] 返回 1。

* idea: 用一个优先级队列维护最大的3个数，注意 默认的 PriorityQueue 按自然序排列，因此传一个 Comparator 进去
从大到小排序。另 PriorityQueue 允许重复元素，根据题意 只将非重复元素加入 pq 即可。


### findModeInSortedArray
从一个有序数组里找出 mode 元素，mode元素：出现次数最多的元素。


	{1, 2, 2, 3} r=[2]
	{1, 2, 2, 3, 3, 5} r=[2, 3]
	{1, 2, 3, 4} r=[1,2,3,4]
	{1, 2, 2, 3, 3, 3, 5} r=[3]

* idea: 相同元素必须相邻出现，挨个计数，遇到出现次数更多的更新 r 和max 即可。
	
	    public List<Integer> findModeInSortedArray(int[] nums) {
	        List<Integer> r = new ArrayList<Integer>();
	        int prev = nums[0];
	        r.add(prev);
	        int count = 1;
	        int max = 1;
	        for (int i = 1; i < nums.length; i++) {
	            if (nums[i] == prev) {
	                count++;
	            } else {
	                count = 1;
	            }
	            if (count > max) {// 新的mode
	                r.clear();
	                r.add(nums[i]);
	                max = count;
	            } else if (count == max) {
	                r.add(nums[i]);
	            }
	            prev = nums[i];
	        }
	        return r;
	    }


### LongestConsecutiveSequence
给定未排序的数组，求最长连续元素序列的长度。
Given [100, 4, 200, 1, 3, 2],
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.
hulu 的出题法：在一部电视剧中，如果有几集文件损坏了导致无法播放，
那么从任意一集没有损坏的视频开始观看，用户最多可以连续看多少集？


    public int longestConsecutive(int[] nums) {
        int res = 0;
        Set<Integer> s = new HashSet<Integer>();
        for (int num : nums) {
            s.add(num);
        }
        for (int num : nums) {
            if (s.remove(num)) {
                int pre = num - 1, next = num + 1;
                while (s.remove(pre)) {
                    --pre;
                }
                while (s.remove(next)) {
                    ++next;
                }
                res = Math.max(res, next - pre - 1);
            }
        }
        return res;
    }


    public int longestConsecutiveHulu(int[] nums) {
        // 找最大元素max
        int max = Integer.MIN_VALUE;
        for (int e : nums) {
            max = (e > max) ? e : max;
        }
        int[] m = new int[max + 1];
        
        for (int i = 0; i < nums.length; i++) {
            m[nums[i]] = 1;
        }
        
        return maxConsecutive(m);
    }

    
    private int maxConsecutive(int[] m) {
        int start = -1;
        int maxlen = 1;
        for (int i = 0; i < m.length; i++) {
            if (m[i] == 0) {
                continue;
            }
            if (i == 0 || m[i - 1] == 0) {
                start = i;
            }
            if (i > 0 && m[i - 1] == 1) {
                maxlen = Math.max(i - start + 1, maxlen);
            }
        }
        return maxlen;
    }
