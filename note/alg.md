## 使用Scanner

```java
import java.util.Scanner;
Scanner sc = new Scanner(System.in);


//运用上面创建的sc对象调用对应的方法，控制台即可等待用户输入，我们自定义一个变量来接收即可
//我们想要输入的数据类型不同，也需要调用不同的方法，具体应用如下：
String str = sc.nextLine();
String str = sc.next();
byte a1= sc.nextByte();
short a2= sc.nextShort();
int a3 = sc.nextInt();
long a4 = sc.nextLong();
float a5 = sc.nextFloat();
double a6 = sc.nextDouble();
boolean a7 = sc.nextBoolean();
BigInteger bi=sc.nextBigInteger();
BigDecimal bd = sc.nextBigDecimal();
————————————————
版权声明：本文为CSDN博主「努力努力再努力c.」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/m0_51358164/article/details/125877018
```

next()与nextLine()的区别

next()用法总结：

next()用法总结：

1. 一定要读取到有效字符后才可以结束输入。
2. 对输入的有效字符之前所遇到的空白，会自动将其去除。
3. 只有输入的有效字符后才将其后面输入的空白作为结束符，即以以空格作为自己的读取结束标识符
4. next()不能得到<mark>带有空格</mark>的字符串。
5. 读取结束后，该方法会将我们的鼠标定位在我们输入数据的那一行。

nextLine()用法总结：
1、以回车符作为结束标识符，获取到的是回车符前输入的所有字符串（包括空格）。
2、读取结束后，<mark>该方法会将我们的鼠标定位在我们输入数据的那一行的下一行</mark>。

先使用nextLine再使用next()、nextInt()等没问题，但是先使用next()和nextInt()等之后就不可以再紧跟nextLine使用。（<mark>这一点很重要！！！</mark>）
这是因为next()等这些方法读取结束后会紧跟一个回车符，而nextLine会直接读取到这个回车符，这就导致出现我们还没有来得及输入我们想要输入的数据，nextLine就以为我们已经输入完了这样的情况
>使用next后想再用nextLine读取，先用nextLine一次
判断是否有下一个输入可以用sc.<mark>hasNext()</mark>或sc.hasNextInt()或sc.hasNextDouble()或sc.hasNextLine()

输出

1. System.out.println();//换行打印，输出之后会自动换行
2. System.out.print();//不换行打印
3. System.out.printf();//按格式输出
4. System.out.printf("%.1f,3.55)，保留1位小数，2位是2f
## 前缀和

### 一维前缀和

![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-03-29-15-41-07-image.png)

```java
class NumArray {
    // 前缀和数组
    private int[] preSum;

    /* 输入一个数组，构造前缀和 */
    public NumArray(int[] nums) {
        // preSum[0] = 0，便于计算累加和
        preSum = new int[nums.length + 1];
        // 计算 nums 的累加和
        for (int i = 1; i < preSum.length; i++) {
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
    }

    /* 查询闭区间 [left, right] 的累加和 */
    public int sumRange(int left, int right) {
        return preSum[right + 1] - preSum[left];
    }
}
```

应用场景：求连续子数组之和，两个前缀和相减，便得到子数组之和

求子数组和最大，可以维护最小前缀和

### 二维前缀和

![](https://mobsidian.oss-cn-beijing.aliyuncs.com/1692152740-dSPisw-matrix-sum.png)

```java
class MatrixSum {
    private final int[][] sum;

    public MatrixSum(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        sum = new int[m + 1][n + 1]; // 注意：如果 matrix[i][j] 范围很大，需要使用 long
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                sum[i + 1][j + 1] = sum[i + 1][j] + sum[i][j + 1] - sum[i][j] + matrix[i][j];
            }
        }
    }

    // 返回左上角在 (r1,c1) 右下角在 (r2-1,c2-1) 的子矩阵元素和（类似前缀和的左闭右开）
    public int query(int r1, int c1, int r2, int c2) {
        return sum[r2][c2] - sum[r2][c1] - sum[r1][c2] + sum[r1][c1];
    }

    // 如果你不习惯左闭右开，也可以这样写
    // 返回左上角在 (r1,c1) 右下角在 (r2,c2) 的子矩阵元素和
    public int query2(int r1, int c1, int r2, int c2) {
        return sum[r2 + 1][c2 + 1] - sum[r2 + 1][c1] - sum[r1][c2 + 1] + sum[r1][c1];
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/circle/discuss/UUuRex/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 差分数组

![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-03-29-15-51-39-image.png)

```java
// 差分数组工具类
class Difference {
    // 差分数组
    private int[] diff;

    /* 输入一个初始数组，区间操作将在这个数组上进行 */
    public Difference(int[] nums) {
        assert nums.length > 0;
        diff = new int[nums.length];
        // 根据初始数组构造差分数组
        diff[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            diff[i] = nums[i] - nums[i - 1];
        }
    }

    /* 给闭区间 [i, j] 增加 val（可以是负数）*/
    public void increment(int i, int j, int val) {
        diff[i] += val;
        if (j + 1 < diff.length) {
            diff[j + 1] -= val;
        }
    }

    /* 返回结果数组 */
    public int[] result() {
        int[] res = new int[diff.length];
        // 根据差分数组构造结果数组
        res[0] = diff[0];
        for (int i = 1; i < diff.length; i++) {
            res[i] = res[i - 1] + diff[i];
        }
        return res;
    }
}
```

> 当 `j+1 >= diff.length` 时，说明是对 `nums[i]` 及以后的整个数组都进行修改，那么就不需要再给 `diff` 数组减 `val` 了。

## 哈希表优化

哈希表适合在一些数中找另一个数，比如两数之和为target，即nums[i]+nums[j]=target，问题变化为nums[j]=target-nums[i]，即向右边遍历j，寻找左边的数为target-nums[i];故可以利用哈希表，key为nums[j]

## 树
### 序列化
==ACM创建树其实就是二叉树的反序列化==

[二叉树的题，就那几个框架，枯燥至极🤔 (qq.com)](https://mp.weixin.qq.com/s/DVX2A1ha4xSecEXLxW_UsA)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
```

```java
    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] nodes=data.split(",");
        LinkedList<String> list=new LinkedList<>();
        for(String node:nodes){
            list.addLast(node);
        }
        return deserialize(list);

    }
    public TreeNode deserialize(LinkedList<String> nodes){
        if(nodes.isEmpty()) return null;
        String node=nodes.removeFirst();
        if(node.equals("#")) return null;
        TreeNode root=new TreeNode(Integer.parseInt(node));
        root.left=deserialize(nodes);
        root.right=deserialize(nodes);
        return root;

    }
```

> 注意：data是前序遍历的结果（如1,2,#,4,#,#,3,#,#,），用#表示null
> 
> 后序类似，取出<mark>最后一个</mark>作为根节点，创建节点，构造左子树和右子树，先构造右子树再构造左子树，因为右子树在先
> 
> 中序遍历无法实现反序列化，也就无法创建树

层次遍历序列化

```java
String SEP = ",";
String NULL = "#";

/* 将二叉树序列化为字符串 */
String serialize(TreeNode root) {
    if (root == null) return "";
    StringBuilder sb = new StringBuilder();
    // 初始化队列，将 root 加入队列
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        TreeNode cur = q.poll();

        /* 层级遍历代码位置 */
        if (cur == null) {
            sb.append(NULL).append(SEP);
            continue;
        }
        sb.append(cur.val).append(SEP);
        /*****************/

        q.offer(cur.left);
        q.offer(cur.right);
    }

    return sb.toString();
}
```

反序列化，也就是层次遍历创建树

```java
/* 将字符串反序列化为二叉树结构 */
TreeNode deserialize(String data) {
    if (data.isEmpty()) return null;
    String[] nodes = data.split(SEP);
    // 第一个元素就是 root 的值
    TreeNode root = new TreeNode(Integer.parseInt(nodes[0]));

    // 队列 q 记录父节点，将 root 加入队列
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    for (int i = 1; i < nodes.length; ) {
        // 队列中存的都是父节点
        TreeNode parent = q.poll();
        // 父节点对应的左侧子节点的值
        String left = nodes[i++];
        if (!left.equals(NULL)) {
            parent.left = new TreeNode(Integer.parseInt(left));
            q.offer(parent.left);
        } else {
            parent.left = null;
        }
        // 父节点对应的右侧子节点的值
        String right = nodes[i++];
        if (!right.equals(NULL)) {
            parent.right = new TreeNode(Integer.parseInt(right));
            q.offer(parent.right);
        } else {
            parent.right = null;
        }
    }
    return root;
}
```


### 遍历
二叉树的先序、中序、后序遍历

先序: **任何子树的处理顺序都是，先头节点、再左子树、然后右子树**

中序: **任何子树的处理顺序都是，先左子树、再头节点、然后右子树**

后序: **任何子树的处理顺序都是，先左子树、再右子树、然后头节点**

> 前序，第一次到达当前节点就打印
> 
> 中序，第二次到达当前节点就打印
> 
> 后序，第三次到达当前节点就打印



 #### 先序遍历

准备一个栈，

1. 第一步__将非空根结点压栈__
   
   出栈并打印

2. **先将刚刚那个出栈的节点的非空右子树结点压栈 (空了就不压呗)，（利用栈的后进先出特性）**

3. **后将刚刚那个出栈的节点的非空左子树结点压栈 (空了就不压呗)**
   
   > 还是先序，只不过非递归方式就是我们自己放入栈，要先把右边的压进去，再把左边的压进去

​ 当前栈顶的节点出栈并打印

4. 重复 2,3

直到栈空了，说明所有的都出栈了 (这里出站顺序就是前序，我们可以做各种操作), 所有的都出栈了就一定是在所有节点 (每次循环栈 pop 出来的) 左孩子都已经压栈然后出栈或者就是空的，所有右孩子也都已经压栈然后出栈或者就是空的

![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2023-02-20-15-08-45-image.png)

#### 后序遍历
两个栈，一个工具栈，一个收栈

1. 第一步__将非空根结点压栈__

​ 出这个栈，入收栈

2. **先将刚刚那个出栈的节点的非空左子树结点压栈 (空了就不压呗), 注意这里又是左边先了**

3. **后将刚刚那个出栈的节点的非空右子树结点压栈 (空了就不压呗)**

​ 出这个栈 (此时就是我们上面刚放的那个右孩子节点 (注意如果是空的话就不是这个了)), 入收栈

4. 重复 2,3

此时再将收栈的节点一个一个 pop 出，那么最后的顺序就是后序了
![]()
![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2023-02-20-15-22-38-image.png)

#### 中序遍历
![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2023-02-20-15-43-03-image.png)


## 递归

### 排列与组合/子集

**形式一、元素无重不可复选，即 `nums` 中的元素都是唯一的，每个元素最多只能被使用一次**

```java
/* 组合/子集问题回溯算法框架 */
//这是从答案的角度
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i + 1);
        // 撤销选择
        track.removeLast();
    }
}
//输入的角度
void dfs(int[] nums,int i,int sum){
    if(i==nums.length()) return;
//不选
    dfs(nums,i,sum);
//选
    dfs(nums,i+1,sum+nums[i])
}

/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 剪枝逻辑
        if (used[i]) {
            continue;
        }
        // 做选择
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        // 撤销选择
        track.removeLast();
        used[i] = false;
    }
}
```

**形式二、元素可重不可复选，即 `nums` 中的元素可以存在重复，每个元素最多只能被使用一次**，其关键在于排序和剪枝，`backtrack` 核心代码如下：

```java
Arrays.sort(nums);
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 剪枝逻辑，跳过值相同的相邻树枝
        if (i > start && nums[i] == nums[i - 1]) {
            continue;
        }
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i + 1);
        // 撤销选择
        track.removeLast();
    }
}


Arrays.sort(nums);
/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 剪枝逻辑
        if (used[i]) {
            continue;
        }
        // 剪枝逻辑，固定相同的元素在排列中的相对位置
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
            continue;
        }
        // 做选择
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        // 撤销选择
        track.removeLast();
        used[i] = false;
    }
}
```

**形式三、元素无重可复选，即 `nums` 中的元素都是唯一的，每个元素可以被使用若干次**，只要删掉去重逻辑即可，`backtrack` 核心代码如下：

```java
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i);
        // 撤销选择
        track.removeLast();
    }
}


/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        backtrack(nums);
        // 撤销选择
        track.removeLast();
    }
}
```

> 组合/子集问题有两种角度，输入和答案的角度，而排列并没有
> 
> 子集问题的答案角度有个start，从start遍历答案，如果子集问题从0开始就变成全排列了，从start开始选，start之前的就不可以选了
> 
> 排列问题是从0开始的
> 
> 去重问题：添加剪枝逻辑，记得排序
> 
> 重复选问题：子集可以重复选的话dfs是从i开始的，而不是i+1开始；而排列去除used逻辑就可以了

### 动态规划

[分享丨【题单】动态规划（入门/背包/状态机/划分/区间/状压/数位/树形/数据结构优化） - 力扣（LeetCode）](https://leetcode.cn/circle/discuss/tXLS3i/)

#### 入门dp

+ 爬楼梯

+ 打家劫舍

+ 最大子数组和
  
  > ![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-04-11-20-23-34-image.png)

#### 背包问题

+ 01背包（每个物品只能选一次。）

+ 多重背包（物品可以重复选，有个数限制。）

+ 完全背包（物品可以重复选，无个数限制。）

#### 树形dp

1. 树的直径（dfs返回以root为起点的最长路径，并在每个节点更新左子树的最长路径+当前节点+右子树的最长路径）

2. 打家劫舍3（每个节点都有选与不选的状态）
   
   ![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-04-16-14-30-14-image.png)
   
   ```java
       //返回选当前结点时，以root为根的子树最大点权和
       //不选当前结点时，以root为跟的子树最大点权和）
   ```

3. 监控二叉树

## 栈

### 单调栈

1. 每日温度
   
   ![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-04-16-15-54-56-image.png)
   
   > 后进先出

2. 接雨水
   
   1. 方法一：假设每个位置是宽度为1的桶，计算这个桶的接水量就需要计算左边木板的高度（左边的最大值）和右边木板的高度（右边的最大值），取最小值，然后减去当前位置的高度就得到当前位置的接水量。计算左右边木板的高度可以用数组表示，当然也可以用双指针。这种做法相当于竖着计算
   
   2. 方法二：横着计算，从左往右遍历，面积由三个下标组成，栈顶元素的下标，栈顶下一个元素下标，当前元素下标，当前元素-栈顶下一个元素下标-1的差就是面积的宽度，当前元素和栈顶下一个元素的最小值减去栈顶元素就是高，即每个元素**找上一个更大的元素，在找的过程中计算（填坑）**，即用到单调栈，并且还要知道栈顶元素的下一个元素，才能计算，否则计算不了。

3. 股票价格维度（求左边比当前元素小的个数->上一个更大元素的下标，当前下标减去）

4. 柱状图最大面积（矩形的高度一定是heights中的数，宽度是right-left-1,其中right是下一个小于当前元素的下标，没有是n，left是上一个小于当前元素的下标，没有是-1，用到单调栈）

5. 最大矩形
   
   1. 转换成柱状图最大面积
   
   2. ![](https://mobsidian.oss-cn-beijing.aliyuncs.com/maxarea.png)

6. 上一个更大（小）的元素（从左往右遍历好理解）

7. 下一个更大（小）的元素（从右往左遍历好理解）

### 栈的应用

基本计算器2

法一

![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-04-18-15-05-23-image.png)

![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-04-18-15-05-55-image.png)

```java
class Solution {
    // 使用 map 维护一个运算符优先级
    // 这里的优先级划分按照「数学」进行划分即可
    Map<Character, Integer> map = new HashMap<>(){{
        put('-', 1);
        put('+', 1);
        put('*', 2);
        put('/', 2);
        put('%', 2);
        put('^', 3);
    }};
    public int calculate(String s) {
        // 将所有的空格去掉
        s = s.replaceAll(" ", "");
        char[] cs = s.toCharArray();
        int n = s.length();
        // 存放所有的数字
        Deque<Integer> nums = new ArrayDeque<>();
        // 为了防止第一个数为负数，先往 nums 加个 0
        nums.addLast(0);
        // 存放所有「非数字以外」的操作
        Deque<Character> ops = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            char c = cs[i];
            if (c == '(') {//左括号
                ops.addLast(c);
            } else if (c == ')') {
                // 计算到最近一个左括号为止
                while (!ops.isEmpty()) {
                    if (ops.peekLast() != '(') {
                        calc(nums, ops);
                    } else {
                        ops.pollLast();
                        break;
                    }
                }
            } else {
                if (isNumber(c)) {//数字
                    int u = 0;
                    int j = i;
                    // 将从 i 位置开始后面的连续数字整体取出，加入 nums
                    while (j < n && isNumber(cs[j])) u = u * 10 + (cs[j++] - '0');
                    nums.addLast(u);
                    i = j - 1;
                } else {//操作符
                    if (i > 0 && (cs[i - 1] == '(' || cs[i - 1] == '+' || cs[i - 1] == '-')) {
                        nums.addLast(0);
                    }
                    // 有一个新操作要入栈时，先把栈内可以算的都算了 
                    // 只有满足「栈内运算符」比「当前运算符」优先级高/同等，才进行运算
                    while (!ops.isEmpty() && ops.peekLast() != '(') {
                        char prev = ops.peekLast();
                        if (map.get(prev) >= map.get(c)) {
                            calc(nums, ops);
                        } else {
                            break;
                        }
                    }
                    ops.addLast(c);
                }
            }
        }
        // 将剩余的计算完
        while (!ops.isEmpty()) calc(nums, ops);
        return nums.peekLast();
    }
    void calc(Deque<Integer> nums, Deque<Character> ops) {
        if (nums.isEmpty() || nums.size() < 2) return;
        if (ops.isEmpty()) return;
        int b = nums.pollLast(), a = nums.pollLast();
        char op = ops.pollLast();
        int ans = 0;
        if (op == '+') ans = a + b;
        else if (op == '-') ans = a - b;
        else if (op == '*') ans = a * b;
        else if (op == '/')  ans = a / b;
        else if (op == '^') ans = (int)Math.pow(a, b);
        else if (op == '%') ans = a % b;
        nums.addLast(ans);
    }
    boolean isNumber(char c) {
        return Character.isDigit(c);
    }
}

作者：宫水三叶
链接：https://leetcode.cn/problems/basic-calculator-ii/solutions/648832/shi-yong-shuang-zhan-jie-jue-jiu-ji-biao-c65k/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

> 如果只有加减，优先级可以不用

法2：将中序表达式转换成逆波兰表达式，再计算逆波兰表达式

---

字符串解码

- 本题难点在于括号内嵌套括号，需要**从内向外**生成与拼接字符串，这与栈的**先入后出**特性对应。

```java
class Solution {
    public String decodeString(String s) {
                //双栈解法：
        //准备两个栈：一个存放数字，一个存放字符串
        //遍历字符有四种情况
        //1、如果是数字 将数字转成整型数字等待处理
        //2、如果是字符 将字符添加到当前临时字符串中
        //3、如果是'['  将当前数字和临时字符串添加到各自栈中
        //4、如果是']'  将数字和字符栈各取出，然后拼接成新的临时字符串
        //Java 推荐用Deque ArrayDeque实现栈
        //创建数字栈，创建字符串栈 及临时数字和临时字符串
        Deque<Integer> stack_digit=new ArrayDeque<>();
        Deque<StringBuilder> stack_string=new ArrayDeque<>();
        int tNum=0;
        StringBuilder tString = new StringBuilder();
        int i=0;
        while(i<s.length()){
            char ch = s.charAt(i++);
            if(ch=='['){
                stack_digit.push(tNum);
                stack_string.push(tString);
                tNum=0;
                tString=new StringBuilder();
            }else if(ch==']'){
                StringBuilder temp = stack_string.pop();
                int count=stack_digit.pop();
                for(int j=0;j<count;j++){
                    temp.append(tString.toString());
                }
                tString=temp;
            }else if('0'<=ch&&ch<='9'){
                tNum=tNum*10+ch-'0';
            }else{
                tString.append(ch);
            }
        }
        return tString.toString();
    }
}
```

> tString是结果，stack_string这个栈存放的上一次的结果，当[时，tString加入栈中并重新复制，也就是说遇到[时，把[前面的字符串和数字保存起来，当]时，tString就是[]中的内容

## 单调队列

1. 滑动窗口最大值

![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-04-16-14-44-06-image.png)

队首是最大的，为保持队列的单调性，如果队尾的元素小于当前元素，删除队尾

```java
            while(!dq.isEmpty()&&nums[dq.getLast()]<=num){
                dq.removeLast();
            }
```

当队头不在窗口，要将队头移除



## 位运算

### n&(n-1)

(n−1) 作用： 二进制数字 nnn 最右边的 111 变成 000 ，此 111 右边的 000 都变成 111 。

n&(n−1)n \& (n - 1)n&(n−1) 作用： 二进制数字 nnn 最右边的 111 变成 000 ，其余不变

![](https://mobsidian.oss-cn-beijing.aliyuncs.com/2024-04-28-12-07-35-image.png)

可以将最右边的1变为0，也可以求出1的个数

作者：Krahets
链接：https://leetcode.cn/problems/number-of-1-bits/solutions/2361978/191-wei-1-de-ge-shu-wei-yun-suan-qing-xi-40rw/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
### 异或运算

OR 运算的运算子有两种情况，计算结果为`true`。

（1）一个为 true，另一个为 false;

（2）两个都为 true。

上面两种情况，有时候需要明确区分，所以引入了 XOR。

XOR 排除了第二种情况，只有第一种情况（一个运算子为`true`，另一个为`false`）才会返回 true，所以可以看成是更单纯的 OR 运算。也就是说， **XOR 主要用来判断两个值是否不同。**

XOR 一般使用插入符号（caret）`^`表示。如果约定`0` 为 false，`1` 为 true，那么 XOR 的运算真值表如下。

```javascript
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0
```

> 不同为true

XOR 运算有以下的运算定律。由于非常简单，这里就省略证明了。

（1）**一个值与自身的运算，总是为 false。**
（2）**一个值与 0 的运算，总是等于其本身。**
（3）**可交换性**
（4）**结合性**

请看下面这道题。

> 一个数组包含 n-1 个成员，这些成员是 1 到 n 之间的整数，且没有重复，请找出缺少的那个数字。

最快的解答方法，就是把所有数组成员（A[0] 一直到 A[n-2]）与 1 到 n 的整数全部放在一起，进行异或运算。

> ```javascript
> A[0] ^ A[1] ^ ... ^ A[n-2] ^ 1 ^ 2 ^ ... ^ n
> ```

上面这个式子中，每个数组成员都会出现两次，相同的值进行异或运算就会得到 0。只有缺少的那个数字出现一次，所以最后得到的就是这个值。

[二进制的原码、反码、补码 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/99082236)

### 质数

给定整数 `n` ，返回 *所有小于非负整数 `n` 的质数的数量*

```java
class Solution {
    public int countPrimes(int n) {
        boolean[] isPrime = new boolean[n];
        Arrays.fill(isPrime, true);
        for (int i = 2; i * i < n; i++) 
            if (isPrime[i]) 
                for (int j = i * i; j < n; j += i) 
                    isPrime[j] = false;

        int count = 0;
        for (int i = 2; i < n; i++)
            if (isPrime[i]) count++;

        return count;
    }
}
```

> 第二个循环中j=i*i：`i = 5` 时算法会标记 5 × 2 = 10，5 × 3 = 15 等等数字，但是这两个数字已经被 `i = 2` 和 `i = 3` 的 2 × 5 和 3 × 5 标记了。所以从i * i开始
> 
> 遍历到sqrt(n)：因为如果存在一个大于sqrt(n)的非质数m，那么它一定可以分解成两个小于或等于sqrt(n)的因子，其中之一必然小于或等于sqrt(n)。如果我们遍历到sqrt(n)之后还存在非质数，那么这些非质数的倍数已经被之前的遍历过程标记为非质数了，也就是说被小于sqrt(n)的数遍历过了

### 最大公约数

基本思路如下：1.a÷b，令r为所得余数（0≤r＜b）。若r=0，算法结束；b即为答案；2.互换：置a←b，b←r，并返回第一步

```java
int many(int a,int b) {
    int r;
    while(b!=0)
    {
       r=a%b;
       a=b;
       b=r;
    }
    return a;
}
```

第一种方法就是通过计算出两个整数的最大公约数来计算最小公倍数，因为最小公倍数与最大公约数的关系就是，**两个数的最小公倍数等于这两个数的乘积除于两个数的最大公约数**

### 进制转换

十进制整数转换为二进制整数采用 **"除 2 取余，逆序排列"** 法。

> **具体做法是：**用 2 整除十进制整数，可以得到一个商和余数；再用 2 去除商，又会得到一个商和余数，如此进行，直到商为小于 1 时为止，然后把先得到的余数作为二进制数的低位有效位，后得到的余数作为二进制数的高位有效位，依次排列起来。