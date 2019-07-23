#### CS-Notes 剑指offer_3

-------

##### 题目描述

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

- 要求时间复杂度 O(N)，空间复杂度 O(1)。因此不能使用排序的方法，也不能使用额外的标记数组。

##### 总结

对于这种数组元素在 [0, n-1] 范围内的问题，可以将值为 i 的元素调整到第 i 个位置上进行求解。

以 (2, 3, 1, 0, 2, 5) 为例，遍历到位置 4 时，该位置上的数为 2，但是第 2 个位置上已经有一个 2 的值了，因此可以知道 2 重复：

![T3](E:\我的坚果云\carbon.now.sh\数组 T3.png)

#### CS-Notes 剑指offer_4

---

##### 题目描述

给定一个二维数组，其每一行从左到右递增排序，从上到下也是递增排序。给定一个数，判断这个数是否在该二维数组中。

> ```html
> Consider the following matrix:
> [
>   [1,   4,  7, 11, 15],
>   [2,   5,  8, 12, 19],
>   [3,   6,  9, 16, 22],
>   [10, 13, 14, 17, 24],
>   [18, 21, 23, 26, 30]
> ]
> 
> Given target = 5, return true.
> Given target = 20, return false.
> ```

**要求时间复杂度 O(M + N)，空间复杂度 O(1)。其中 M 为行数，N 为 列数。**

##### 总结

- 需要充分利用该二维数组的特点（从左到右递增排序，从上到下也是递增排序），找出解题方法。

- 从**右上角/左下角**开始查找，就可以根据 target 和当前元素的大小关系来缩小查找区间

- 没有单独处理输入数组为空或非二维数组的情况。**要先对特殊情况做处理**。


![](E:\我的坚果云\carbon.now.sh\数组 T4.png)

#### CS-Notes 剑指offer_5

------

##### 题目描述

将一个字符串中的空格替换成 "%20"。

> ```html
> Input:
> "A B"
> 
> Output:
> "A%20B"
> ```

##### 总结

- 要注意**索引下标**的正确性，P--，--P要仔细考虑一下，否则会出现越界错误：java.lang.StringIndexOutOfBoundsException: String index out of range: -1

- 对于String的常用方法还不够熟悉，这里涉及到str.length(), str.charAt(index), str.setCharAt(index,c), str.toString()

  [Java String类](<https://www.runoob.com/java/java-string.html>)

![](E:\我的坚果云\carbon.now.sh\字符串 T5.png)

#### CS-Notes 剑指offer_6

------

题目描述

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

##### 总结

- **重刷**
- 三种方法：**递归，头插法，栈**
- 递归：需要明确**递归边界**和**递归逻辑**，**一层一层调用，直到递归结束条件成立，再一层一层返回**。配合addAll()，按序将返回值添加到ArrayList尾部。
- 头插法：
  - 直接用add(index=0,element)头插法构建ArrayList
  - 用头插法构建逆序链表,再用add(element)尾插法构建ArrayList

- 利用栈的LIFO性质，stack.push()，stack.pop()。

  区分：stack.peek()只返回栈顶的值，不删除栈顶的值

![](E:\我的坚果云\carbon.now.sh\链表 T6.png)

#### CS-Notes 剑指offer_7

------

##### 题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

##### 总结

- 前序遍历的第一个值为根节点的值，查找出该值在中序遍历中的位置，即可将其分为两部分，左部分为树的左子树中序遍历结果，右部分为树的右子树中序遍历的结果。接着进行递归重建左子树和右子树
- 通过画简单的示意图，确定左、右子树的前序遍历和中序遍历在两数组中的索引范围
- 对Java作用域不了解，出现Error，块级变量是定义在一个块内部的变量，其生存周期就是这个块

![](E:\我的坚果云\carbon.now.sh\树 T7.png)

#### CS-Notes 剑指offer_8

------

##### 题目描述

给定一个二叉树和其中的一个结点（pNode），请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

##### 总结

- 要注意到题目中给出的 TreeLinkNode next  即为当前结点指向父结点的指针

- 如果当前结点的右子树存在，那么返回右子树的最左叶子结点（即右子树中最先遍历的结点）
- 如果当前结点的右子树不存在：
  - 若当前结点为父结点的左结点，那么返回当前结点的父结点
  - 若当前结点为父结点的右结点，那么继续向上遍历父结点的父结点，直到找到一个结点是其父结点的左子结点，返回该结点


![](E:\我的坚果云\carbon.now.sh\二叉树 T8.png)

#### CS-Notes 剑指offer_9

------

##### 题目描述

用两个栈来实现一个队列，完成队列的 Push 和 Pop 操作。

##### 总结

- 两个栈，一个栈（in）处理入栈操作，一个栈（out）处理出栈操作。

- 队列先进先出，而栈后进先出，关键在于出队前，需要将栈中元素顺序进行反转，故每次pop操作需要将in中元素pop并依序push到out中，实现顺序的反转，再进行出队pop操作。

- 若考虑到push和pop是多次操作的，那么第二次push操作，也需要用同样的方法将新的队列从out栈中push到in栈中，再进行入队push操作。

- **进时，栈out是否为空，不为空，则栈out元素倒回到栈in，出时，将栈in元素全部弹到栈out中，直到栈in为空。**


![](E:\我的坚果云\carbon.now.sh\栈 T9.png)

#### CS-Notes 剑指offer_10.1

------

##### 题目描述

求斐波那契数列的第 n 项，n <= 39。

##### 总结

- 递归，每次递归需要重复计算子问题，效率极低，需要指数级时间（运行时间1263 ms，占用内存9272 K）
- 动态规划，建立一个数组，把子问题的解缓存起来，避免重复计算，需要线性时间，空间复杂度度为O(N)
- 只存储前两项的值，空间复杂度为O(1)

[浅谈动态规划](<https://blog.csdn.net/feizaoSYUACM/article/details/53846323>)

> 每个阶段只有一个状态->**递推**；
>
> 每个阶段的最优状态都是由上一个阶段的最优状态得到的->**贪心**；
>
> 每个阶段的最优状态是由之前所有阶段的状态的组合得到的->**搜索**；
>
> 每个阶段的最优状态可以从之前某个阶段的某个或某些状态直接得到而不管之前这个状态是如何得到的->**动态规划**。

![](E:\我的坚果云\carbon.now.sh\动态规划 T10.1.png)

#### CS-Notes 剑指offer_10.2

------

##### 题目描述

我们可以用 2\*1 的小矩形横着或者竖着去覆盖更大的矩形。请问用 n 个 2\*1 的小矩形无重叠地覆盖一个 2\*n 的大矩形，总共有多少种方法？

##### 总结

1.  n<= 0 大矩形为<= 2*0,直接return 0； 

2.  n= 1大矩形为2\*1，只有一种摆放方法，return 1；

3.  n= 2 大矩形为2\*2，有两种摆放方法，return 2；

4.  n>2分为两步考虑： 

   第一次摆放一块 2*1 的小矩阵，则摆放方法总共为f(n- 1)

|  √   |      |      |      |      |
| :--: | ---- | ---- | ---- | ---- |
|  √   |      |      |      |      |

​	第一次摆放一块1*2的小矩阵，则摆放方法总共为f(n-2)

|  √   |  √   |      |      |      |
| :--: | :--: | ---- | ---- | ---- |
|      |      |      |      |      |

故：f(n)=f(n-1)+f(n-2)

![](E:\我的坚果云\carbon.now.sh\动态规划 T10.2.png)

#### CS-Notes 剑指offer_10.3

------

##### 题目描述

一只青蛙一次可以跳上 1 级台阶，也可以跳上 2 级。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

##### 总结

同10.2一样的分析方法，同样是斐波那契数列。

#### CS-Notes 剑指offer_10.4

------

##### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

##### 总结

- 动态规划，同上，建立一个数组保存各个子问题的解，避免重复计算

- 数学推导，先推导出数学关系，后直接用公式计算


![](E:\我的坚果云\carbon.now.sh\动态规划 数学推导 T10.4.png)

#### CS-Notes 剑指offer_11

------

##### 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

##### 总结

- 第一次做只会采用顺序查找，想不到如何利用原始数组的**非减排序**性质。

- 将旋转数组对半分，包含最小值的一段仍然是旋转数组，另一段是非减排序的数组

- 关键在于如何判断出：哪一个是旋转数组，哪一个是非递减数组

  - 当 nums[m] <= nums[h] 时，表示 [m, h] 区间内的数组是非递减数组，[l, m] 区间内的数组是旋转数组，此时令 h = m；

  - 否则 [m + 1, h] 区间内的数组是旋转数组，令 l = m + 1。

- 还需要注意一种特殊情况（1 1 1 0 1），这时nums[m] =nums[l] =nums[h] ，无法判断出旋转数组是哪一侧，只能切换为顺序查找

- 时间复杂度：顺序查找 O(N)；二分查找 O(logN )


![](E:\我的坚果云\carbon.now.sh\数组 二分法T11.png)

#### CS-Notes 剑指offer_12

------

##### 题目描述

------

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则之后不能再次进入这个格子。 例如 a b c e s f c s a d e e 这样的3 X 4 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

##### 总结

- 对于这种过程稍微复杂的算法就无从下手，看了解题思路后还是不知道怎么写，只能跟着参考源码写了一遍，理解了整段代码的实现过程。
- 因为其中一个方法结束的 '}' 漏了一个，导致编译失败，基本的书写规范要注意。
- 解题思路：使用回溯法（backtracking）进行求解，它是一种暴力搜索方法，通过搜索所有可能的结果来求解问题。回溯法在一次搜索结束时需要进行回溯（回退），将这一次搜索过程中设置的状态进行清除，从而开始一次新的搜索过程。
  - 将一维数组转化为矩阵（二维数组），同时创建一个矩阵marked用于记录对应位置的matrix格子是否已经被进入过
  - 依次遍历整个矩阵，设定该格子为第一步进入的格子：
    1. 若路径长度（初始为0，当确认当前格子符合条件后，pathLen+1）等于字符串长度，说明已经包含了字符串所有字符，不用再继续向四周搜索，返回true

    2. 若行或列不在[0,rows/cols)之间||当前格子值不等于对应字符值，||当前格子已经被进入过，说明该格子不是正确的路径，返回false

    3. 若当前格子符合条件，进入该格子，继续调用backtracking(),一层层向下寻找周边符合条件的路径，直到pathLen==str.length()，即符合1，搜索完成，一层层的返回true

    4. 相邻格子不符合条件，返回false

    5. 2和4中返回false后，会回溯到上一个格子后，继续搜索下一相邻的格子，若周围四个格子都不符合，那么会返回false，继续向上回溯。


![](E:\我的坚果云\carbon.now.sh\回溯法 数组 T12.png)

#### CS-Notes 剑指offer_13

------

##### 题目描述

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

##### 总结

- 路径节点是否可以重复？按照CS-Notes中的参考代码，应该是不能重复的。

- 使用深度优先搜索（Depth First Search，DFS）方法进行求解。回溯是深度优先搜索的一种特例，它在一次搜索过程中需要设置一些本次搜索过程的局部状态，并在本次搜索结束之后清除状态。而普通的深度优先搜索并不需要使用这些局部状态，虽然还是有可能设置一些全局状态。

- 出现NullPointerException异常，即引用没有指向具体的实例，对于digitSum，在三个方法中都需要使用的一个量，应该采用成员变量。关于java的作用域，要多加注意，增强学习理解。

  ```java
  private void dfs(boolean[][] marked, int r, int c) {
      ...
      this.digitSum=new int[rows][cols];//成员变量（对象实例级变量）
      //int[][] digitSum=new int[rows][cols];//局部变量（方法级变量）
      ...
  }
  
  ```

![](E:\我的坚果云\carbon.now.sh\回溯法 T13.png)


#### CS-Notes 15

------

##### 题目描述

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

##### 总结

- 对n的2进制形式进行右移操作，n&1判断最右边是否为1。

  有符号位的右移操作，对于正数向右补0，对于负数向右补1。这里应该采用无符号位的右移操作

- 对1进行左移操作，&操作依次判断n的2进制形式每一位是否为1。

- 对最优解 n&(n-1)方法的理解：

  - 设n=1100，n-1操作，n最右边的1变为0，1之前的高位不变，1之后的低位全为1，即1011
  - n&(n-1)操作，该1前面的高位不变，1之后包括该位本身全为0
  - n中含有几个1，就能够进行几次n&(n-1)操作
  - 最后一次操作后等于0，作为跳出循环的标志 

![](E:\我的坚果云\carbon.now.sh\T15 位运算.png)


#### CS-Notes 16

------

##### 题目描述

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

##### 总结

- 题目比较简单，传统方法时间复杂度为O(N)，没有想到用递归方法，将复杂度将为O(logN)。
- 要考虑指数运算中几种特殊情况，分别讨论。此外要注意指数为负数的情况。

![](E:\我的坚果云\carbon.now.sh\递归 T 16.png)

#### CS-Notes 17

------

##### 题目描述

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数即 999。

##### 总结

- 最初思路：直接先求得最大值，然后循环（未考虑到大数问题）
- 此题需要考虑**大数问题**，n位数用整型(int)或者长整型(long long)容易溢出。常用的解决办法是用字符串或者数组来表达大数。参考代码采用字符串来解决大数问题
- **重难点：采用递归实现全排列**
- 顺序打印n位数，从第一个不为0的数开始输出，相邻的数之间以空格分开来

![](E:\我的坚果云\carbon.now.sh\递归 大数问题 T17.png)

#### CS-Notes 18.1

------

##### 题目描述

在 O(1) 时间内删除链表节点

##### 总结

- 根据被删除结点的位置不同，采用不同的处理方法：

  - 在最末尾即为尾节点，应从头节点开始遍历到最后一个，删除之，时间复杂度为 O(N)。
  - 不在最末尾，即tobeDelete.next!=null，此时可以将下一个节点的值赋给该节点，然后令该节点指向下下个节点，再删除下一个节点，时间复杂度为 O(1)。
  - 平均时间复杂度为（ N-1+N）/N=O（1）

- 此外要注意特殊情况的单独处理


![](E:\我的坚果云\carbon.now.sh\链表T 18.1.png)

#### CS-Notes 18.2

------

##### 题目描述

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

##### 总结

- **认真看题目，不能想当然**，**排序的链表**说明链表的节点值是有序的，即重复的结点是连续出现的；**重复的结点不保留**,是全部删除，而不是留下其中一个值。

- 非递归思路中通过建立一个空的头节点，使得原链表的头节点一般化，无需单独处理。

- 递归思路：

  - 从头结点开始，若与下一节点值相同，则向下查找第一个不与头结点重复的节点next，递归调用return deleteDuplication（next）

  - 否则头结点是不重复的节点，应当保留，再重新对以头结点的下一节点next为头结点的链表进行删除重复节点操作，pHead.next=deleteDuplication（next）;这样可以将符合条件的头节点保留下来

- 非递归思路：

  - 首先，建立一个first头指针，指向pHead，使得pHead和pHead.next能够用同样的方法来处理
  - 设定p为工作指针，不断向后搜索；last指向当前确定不重复的指针
  - 如果p.val==p.next.val，则向后遍历查找第一个不等于p.val的节点q，令p=q，继续循环，判断p节点是否重复，直至p.next==null
  - 如果p.val！=p.next.val，则节点p为不重复节点，用next加入到以first为头指针的返回链表中，p继续指向下一节点，继续循环，判断p节点是否重复，直至p.next==null

![](E:\我的坚果云\carbon.now.sh\链表 递归 T 18.2.png)

#### CS-Notes 19

------

##### 题目描述

请实现一个函数用来匹配包括'.'和'\*'的正则表达式。模式中的字符'.'表示任意一个字符，而'\*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串(str)"aaa"与模式(pattern)"a.a"和"ab\*ac\*a"匹配，但是与"aa.a"和"ab\*a"均不匹配

##### 总结

- str[i]==pattern[j]||pattern[j]='.'时，当前字符是匹配的

- 先看'\*'，当pattern[j+1]=='*'时，

  再看匹配

  1. 若str[i]与pattern[j]匹配，则match(str,i+1,pattern,j)||match(str,i ,pattern,j+2)
  2. 若str[i]与pattern[j]不匹配，则match(str,i,pattern,j+2)

  当pattern[j+1]！='*'时，match(str,i+1,pattern,j+1)

- 先看匹配，用first_isMatch记录当前是否匹配

  再看下一个字符是否为 '*'

  1. 为'/*' ,match(str,i,pattern,j + 2)||(first_isMatch && match(str,i + 1,pattern,j))
  2. 不为'/*' ，first_isMatch && match(str,i + 1,pattern,j + 1)

- 以上是**递归法**，需要注意**数组的索引不能越界**

- 也可采用**动态规划**

  1. 建立一个数组boolean[][] dp=new boolean[str.length+1] [pattern.length+1];，从末尾开始匹配,str和pattern末尾都是空，dp[str.length] [pattern.length]=true

  2. 创建两重循环，遍历dp，在遍历过程中建立dp数字值之间的关系（参考递归调用），同样也有两种方式

     match(str,i,pattern,j+2)即为dp[i] [j]=dp[i] [j+2]

![](E:\我的坚果云\carbon.now.sh\动态规划 T19.png)

![递归 T19 ](E:\我的坚果云\carbon.now.sh\递归 T19 .png)

#### CS-Notes 20

------

##### 题目描述

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串 "+100" ,"5e2" ,"-123" ,"3.1416" 和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

##### 总结

- String.matchs()方法，需要将char[]转换为字符串

- 使用正则表达式进行匹配 [+-]?\\\\d*(\\\\.\\\\d+)?([eE]\[+-]?\\d+)?

  |        [+-]?        | 正或负符号出现与否                                           |      |
  | :-----------------: | :----------------------------------------------------------- | ---- |
  |        \\\d*        | 整数部分是否出现，如-.34 或 +3.34均符合                      |      |
  |   (\\\\.\\\\d+)?    | 如果出现小数点，那么小数点后面必须有数字；否则都不出现       |      |
  | ([eE]\[+-]?\\\\d+)? | 如果存在指数部分，那么e或E肯定出现，+或-可以不出现，紧接着必须跟着整数；或者整个部分都不出现 |      |

![](E:\我的坚果云\carbon.now.sh\正则表达式 T 20.png)

#### CS-Notes 21

------

##### 题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

##### 总结

- 注意要满足奇数之间、偶数之间相对位置不变，主要有两种方法：

  - 创建一个新数组，时间复杂度O(N)，空间复杂度 O(N)

  - 冒泡思想，每次将当前偶数上浮到当前右侧，时间复杂度 O(N2)，空间复杂度 O(1)

![数组 T21](E:\我的坚果云\carbon.now.sh\数组 T21.png)

#### CS-Notes 22

------

##### 题目描述

输入一个链表，输出该链表中倒数第k个结点。

##### 总结

- 查找倒数第k个结点，关键在于链表的长度未知，只能从头开始沿着链表向后查找
- 注意考虑特殊情况：链表为空；k值大于链表总长
- 两种方法
  1. 直接先求出链表总长，计算出倒数第k个结点的位置是len-k+1
  2. 设链表的长度为 N。设置两个指针 P1 和 P2，先让 P1 移动 K 个节点，则还有 N - K 个节点可以移动。此时让 P1 和 P2 同时移动，可以知道当 P1 移动到链表结尾时，P2 移动到第 N - K 个节点处，该位置就是倒数第 K 个节点。

![链表 T22](E:\我的坚果云\carbon.now.sh\链表 T22.png)

#### CS-Notes 23

---

##### 题目描述

一个链表中包含环，请找出该链表的环的入口结点。要求不能使用额外的空间。

##### 总结

- 参考CS-Notes，画出示意图，利用两个速度不一的指针，得到入口结点的位置。
- 特殊情况：链表为空或不包含环状链

![](E:\我的坚果云\carbon.now.sh\链表 T23.png)

#### CS-Notes 24

------

##### 题目描述

输入一个链表，反转链表后，输出新链表的表头。

##### 总结

- 能够比较快想到用**头插法**来解决，在插入过程要注意链表断裂连接过程和对结点的保存，避免丢失下一次操作所需的结点信息。写完之后可以举个简单的例子，走两次循环，看是否正确

- 对于CS-Notes给出的**递归**方法，看了较长时间才理解

  - 利用递归走到链表的末端，然后再更新每一个node的next 值 (ListNode next=head.next; next.next=head;)，实现链表的反转
  - head.next=null;主要是为了使得反转链表的尾节点（即原链表的头结点）的next为null

  - newhead 始终不变，是通过newHead=ReverseList(next)返回的，为该链表的最后一个结点

![链表 递归 迭代 T24](E:\我的坚果云\carbon.now.sh\链表 递归 迭代 T24.png)

#### CS-Notes 25

------

##### 题目描述

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

##### 总结

- **迭代**

  设定一个空的头结点，能够避免对第一次合并的单独处理。

- **递归**

  还是不知道递归的返回值要如何处理，如何和上一层衔接上，下次类似问题，先根据简单的实例，将循环过程写出来，多思考一下如何转换为递归，不要直接查看参考算法。

![链表 递归 迭代 T25](E:\我的坚果云\carbon.now.sh\链表 递归 迭代 T25.png)

#### CS-Notes 26

------

##### 题目描述

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

##### 总结

- **递归**，逻辑运算符'||'和'&&'的**短路特性**
- 参考思路：
  1. 在树A中找到和B的根节点的值一样的结点R
  2. 判断树A中以R为根结点的子树是不是包含和树B一样的结构

![二叉树 递归 T26](E:\我的坚果云\carbon.now.sh\二叉树 递归 T26.png)

#### CS-Notes 27

------

##### 题目描述

操作给定的二叉树，将其变换为源二叉树的镜像。

##### 总结

- **递归**，先交换根结点，再向下遍历交换所有的左右子结点
- 在swap方法中，互换的是左右结点，而每个子节点是与其根节点关联的，**交换节点对应子树也都对应交换**，但如果只是改变值，对应子节点由于绑定的地址未改变，所以位置不变，只是这个地址代表的值变了而已

![二叉树 递归 T27](E:\我的坚果云\carbon.now.sh\二叉树 递归 T27.png)

#### CS-Notes 28

------

##### 题目描述

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

##### 总结

- **递归**：

  1. 只要pRoot.left和pRoot.right是否对称即可
  2. 两结点的对称需要满足：左右节点的**值相等**&&**对称子树**（left.left，right.right; left.right, right.left） **也对称**，可以画出一个二叉树及其镜像树来理解。

- **栈**

  每次成对入栈，成对出栈，需要注意进栈的顺序，确保是每对互为镜像结点

- **队列**

  思路与栈的解法相同

  区别在于：

  - 栈：深度优先搜索（Depth First Search，DFS）
  - 队列：广度优先搜索（Breadth First Search，BFS），类似于树的层次遍历


![](E:\我的坚果云\carbon.now.sh\二叉树 递归 栈 队列 T28.png)

#### CS-Notes 29

------

##### 题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

##### 总结

- 每一圈遍历需要四个循环，确定循环的起始和终止点
- 同时在每一圈第二次打印一列或一行的时候要判断是否重复打印

![](E:\我的坚果云\carbon.now.sh\矩阵 T29.png)

#### CS-Notes 30

------

##### 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的 min 函数。

##### 总结

利用一个辅助栈来存放最小值 

​      栈  3，4，2，5，1

​      辅助栈 3，3，2，2，1

  每入栈一次，就与辅助栈顶比较大小，如果小就入栈，如果大就入栈当前的辅助栈顶 

  当出栈时，辅助栈也要出栈 

  这种做法可以保证辅助栈顶一定都当前栈的最小值

![栈 T31](E:\我的坚果云\carbon.now.sh\栈 T31.png)

#### CS-Notes 31

------

##### 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。

例如序列 1,2,3,4,5 是某栈的压入顺序，序列 4,5,3,2,1 是该压栈序列对应的一个弹出序列，但 4,3,5,1,2 就不可能是该压栈序列的弹出序列。

##### 总结

- 利用一个栈来模拟这个过程：

  - 每次压入判断当前栈顶元素是否与当前popA元素相等，相等则依次弹出，popA每次后移一位，直至栈顶元素与当前popA元素不等

  ```java
  while(s.peek()==popA[j]&&j<popA.length-1)
  ```

  - 不能将栈元素全部弹出，会出现栈空错误（java.util.EmptyStackException），所以限定j<popA.length-1，最后return s.peek()==popA[j];

- **EmptyStackException错误**：

  是由于**调用stackA.peek()时，如果stackA为空将会返回EmptyStackException异常**。

  ```java
     /**jdk中peek()源码
       * Looks at the object at the top of this stack without removing it
       * from the stack.
       *
       * @return  the object at the top of this stack (the last item
       *          of the <tt>Vector</tt> object).
       * @throws  EmptyStackException  if this stack is empty.
       */
      public synchronized E peek() {
          int len = size(); 
          if (len == 0)
              throw new EmptyStackException();
          return elementAt(len - 1);
      }
  
      public synchronized E elementAt(int index) {
          if (index >= elementCount) {
              throw new ArrayIndexOutOfBoundsException(index + " >= " + elementCount);
          }
          return elementData(index);
      }
  
       E elementData(int index) {
          return (E) elementData[index];
      }
  ```

  所以在while条件中要加入判断栈不为空的语句，而且由于**'&&'的运算顺序**，需要先判断栈不为空，再取s.peek()，判断与当前popA元素是否相等

  ```java
   while(!s.isEmpty()&&j<popA.length && s.peek()==popA[j]) //运行成功
  
   while(s.peek()==popA[j]&&j<popA.length &&!s.isEmpty() ) //EmptyStackException
  ```

  


#### CS-Notes 32.1

------

##### 题目描述

从上往下打印二叉树

##### 总结

考察的是二叉树的层次遍历（广度优先搜索 BFS）

借助一个队列实现层次遍历：

1. 每弹出一个结点，将该结点的左右子节点（若子节点不为空）压入栈，直至将栈全部弹空。
2. 设定一个值cnt，在开始遍历一层结点时，记录当前栈的大小，即为当前层的结点数。遍历cnt个结点后，再进行下一层结点遍历。（该方法是后面两道题的基础）

![队列 二叉树 层次遍历 BSF T32.1](E:\我的坚果云\carbon.now.sh\队列 二叉树 层次遍历 BSF T32.1.png)

#### CS-Notes 32.2

------

##### 题目描述

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

##### 总结

```java
ArrayList<ArrayList<Integer> > ret=new ArrayList<ArrayList<Integer> > ();
ArrayList<Integer> list=new ArrayList<Integer>();
```

将每一层遍历的结果list添加到ret中，即可

![队列 二叉树 层次遍历 BSF T32.2](E:\我的坚果云\carbon.now.sh\队列 二叉树 层次遍历 BSF T32.2.png)

#### CS-Notes 32.3

------

##### 题目描述

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

##### 总结

建立一个标志flag，用以判断当前层是否为偶数层数，再利用[Collections.reverse](<https://blog.csdn.net/u012062455/article/details/78087314>)方法实现对偶数层遍历的结果list进行顺序反转，实现从右至左的顺序打印。

```java
if(flag>0) Collections.reverse(list);
```

![](E:\我的坚果云\carbon.now.sh\队列 二叉树 层次遍历 BSF T32.3.png)

#### CS-Notes 33

------

##### 题目描述

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

##### 总结

采用**分治思想**：

1. 后序遍历，能首先确定的是，最后一个值为根节点
2. 找出第一个大于根节点的位置i，i的左侧为左子树，右侧（包括i）为右子树
3. 左、右子树也需符合该条件（递归）

**二叉排序树**，或者是一棵空树，或者是具有下列性质的二叉树

1. 空树，定义上是二叉排序树，但题设表示不为空，所以这种情况返回false
2. 若左子树不空，则左子树上所有结点的值均小于它的根结点的值
3. 若右子树不空，则右子树上所有结点的值均大于它的根结点的值
4. 左、右子树也分别为二叉排序树；

![二叉树 递归 T33](E:\我的坚果云\carbon.now.sh\二叉树 递归 T33.png)

#### CS-Notes 34

------

##### 题目描述

输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

##### 总结

- 题意：路径的定义是从根节点开始往下一直到**叶子节点**

- 递归方法：

  ```java
  if(target==0&&root.left==null&&root.right==null)
      ret.add(new ArrayList<Integer> (list));
  ```

直接添加的是一个引用，这个引用指向的是同一个对象，如果直接添加list，下面对list的操作还会改变ret中list的值，也就是添加到最后结果会是一样的。所以这里需要**重新创建一个对象，并用list帮其进行初始化**。

```
list.remove(list.size()-1);
```

因为一直使用了同一个list的空间，遍历完一条路径后回退去找别的路径一定要从list后面删掉回退的元素，不然list会越来越长。

**存在的问题**

1. 讨论区有人提出添加 

   ```java
   if(root == null || target < 0)
              return listAll;
   ```

   这里考虑到节点值有正有负，当target<0时，后续遍历的节点和若为-target也还是能够保证路径的节点总和为0，所以这个说法不正确。

2. 题目描述要求“ 在返回值的list中，数组长度大的数组靠前”，这一点在参考算法中确实没有实现，但由于测试用例的不全面，算法也能够通过。需要再对ret进行排序。

![](E:\我的坚果云\carbon.now.sh\T34 二叉树 递归.png)

```java
//T34 二叉树中和为某一值的路径
//递归
public class Solution {
    private ArrayList<ArrayList<Integer>> ret=new ArrayList<ArrayList<Integer>>();
    private ArrayList<Integer> list = new ArrayList<Integer>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        if(root==null) return ret;
        list.add(root.val);
        target-=root.val;
        //已经到达叶子节点，并且正好加出了target
        if(target==0&&root.left==null&&root.right==null)
       		//将该路径加入res结果集中
            ret.add(new ArrayList<Integer> (list));
        //递归左右子树
        FindPath(root.left,target);
        FindPath(root.right,target);
        //无论当前路径是否加出了target，必须去掉最后一个，然后返回父节点，去查找另一条路径，最终的path肯定为null
        list.remove(list.size()-1);
        return ret;
    }
}
```

#### CS-Notes 35

------

##### 题目描述

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

##### 总结

参考[CS-Notes](<https://cyc2018.github.io/CS-Notes/#/notes/%E5%89%91%E6%8C%87%20Offer%20%E9%A2%98%E8%A7%A3%20-%2030~39?id=_35-%e5%a4%8d%e6%9d%82%e9%93%be%e8%a1%a8%e7%9a%84%e5%a4%8d%e5%88%b6>)的解题思路

1. 先在每个节点的后面插入复制的节点
2. 对复制节点的random链接进行赋值（需要先完成第1步）
3. 拆分链表为原链表和复制链表两部分

整个过程需要遍历链表三次，需要注意while语句的条件，否则会出现NullPointerException异常。

```java
//T35 复杂链表的复制
public class Solution {
    public RandomListNode Clone(RandomListNode pHead)
    {
        if(pHead==null) return null;
        RandomListNode cur=pHead;
        //将节点复制一遍并插入对应节点后面，建立next指针
        while(cur!=null){
            RandomListNode clone = new RandomListNode(cur.label);
            clone.next=cur.next;
            cur.next=clone;
            cur=clone.next;
        }
        //链表顺序确定后，建立random指针
        cur=pHead;
        while(cur!=null){
            RandomListNode clone =cur.next;
            if(cur.random!=null){
                clone.random=cur.random.next;
            }
            cur=clone.next;
        }
        //拆分为两个链表
        cur=pHead;
        RandomListNode pCloneHead=pHead.next;
        while(cur.next!=null){
            RandomListNode next =cur.next;
            cur.next=next.next;
            cur=next;
        }
        return pCloneHead;
    }
}
```



#### CS-Notes 36

------

##### 题目描述

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

##### 总结

解题思路：

pre用于记录当前链表的末尾节点，head用于保存双向链表的头结点

1. 设定返回条件
2. 遍历左子树
3. 当前结点的左指针指向pre，pre的右指针指向当前结点（前提pre！=null）
4. 当前结点成为新的pre结点
5. 在第一次执行到此处时，将当前结点（即根节点的最左边叶子结点）保存为head
6. 遍历右子树

```java
//T36 二叉搜索树和双向链表
//递归方法
public class Solution {
    private TreeNode pre=null;//用于记录当前链表的末尾节点
    private TreeNode head=null;//用于保存双向链表的头结点
    public TreeNode Convert(TreeNode pRootOfTree) {
        inOrder(pRootOfTree);
        return head;
    }
    private void inOrder(TreeNode node){
        if(node==null) return;
        //递归，遍历左子树
        //最里面一层为中序遍历的第一个节点，即最左边的叶子节点
        inOrder(node.left);
        //连接当前节点node和当前链表的尾节点pre
        node.left=pre;
        if(pre!=null)
            pre.right=node;
        //当前节点node成为当前链表的尾节点pre
        pre=node;
        //第一次执行时，node为最左边的叶子节点，作为双向链表的头结点
        if(head==null)
            head=node;//只会执行一次，此后head!=null
        //遍历右子树
        inOrder(node.right);
    }
}
```



#### CS-Notes 37

------

##### 题目描述

请实现两个函数，分别用来序列化和反序列化二叉树

##### 总结

[二叉树的序列化和反序列化](<https://blog.csdn.net/qq_27703417/article/details/70958692>)

**二叉树的序列化**：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于 先序、中序、后序、按层 的二叉树遍历方式来进行修改。

 **二叉树的反序列化**：根据某种遍历顺序得到的序列化字符串结果 str，重构二叉树。

这里用"#"表示空结点，用" "（一个空格）表示一个结点的结束

序列化可以采用递归法，或者借助栈实现非递归法；非序列化采用递归法。具体解题思路参照算法注释。

```java
//T37 序列化和反序列化二叉树
public class Solution {
  //序列化 递归法
    String Serialize(TreeNode root) {
        if(root == null) return "#";
        return root.val + " " + Serialize(root.left) + 
            " " + Serialize(root.right);
  }
  
  //反序列化 递归法
    TreeNode Deserialize(String str) {
        //特殊输入
       if(str == null || str.length() <= 0) return null;
        //将字符串按照" "拆分为数组
        String[] strs = str.split(" ");
        //调用递归方法Deserialize（），重建二叉树，返回根节点
        return Deserialize(strs);
  }
    //设计一个成员变量index用于在每次递归调用时能够使用不同的字符串来建立根结点
    int index = 0;
    private TreeNode Deserialize(String[] strs){
        //如果遇到的是#表示空节点，不再建立子树，这个结点null就是子树的根结点返回
        //同时也要将index++，使字符串后移一位，作为下一次建立节点的对象
        //这里不能合并，直接用"#".equals(strs[index++])
        //否则对于不为“#”的情况下，会index++两次（包含else语句块中的）
        if("#".equals(strs[index])){
            index++;
            return null;
        }
        else{
            //如果不为空结点，则先恢复这个结点
            TreeNode newNode = new TreeNode(-1);
            //每次递归调用，都要将index++
            newNode.val = Integer.parseInt(strs[index++]);
            //恢复左子树
            newNode.left = Deserialize(strs);
            //恢复右子树
            newNode.right = Deserialize(strs);
            //重建二叉树完成，返回根节点
            return newNode;
        }
    }
}

//2 序列化的其他方法
//递归 借助StringBuilder
	String Serialize(TreeNode root) {
        //String是一种不可改变的对象，无法进行引用传递
        //因此使用StringBuilder或者StringBuffer来替代
        StringBuilder sb = new StringBuilder();
        //递归结束的边界条件
        if(root == null){
            sb.append("# ");
            return sb.toString();
        }
        //先遍历根结点
        sb.append(root.val + " ");
        //遍历左结点
        sb.append(Serialize(root.left));
        //遍历右节点
        sb.append(Serialize(root.right));
        return sb.toString();
 	 }

//3 序列化的其他方法
//非递归
	String Serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder("");
        Stack<TreeNode> s = new Stack<>();
        TreeNode cur = root;
        //根节点入栈
        s.push(cur);
        //直到栈为空，所有节点都已经加入sb
        while(!s.isEmpty()){
            //弹出节点
             cur = s.pop();
            //若为空，sb中追加"# "
            if(cur == null) sb.append("# ");
            //若不为空，sb中加入对应值，且将右结点、左结点压入栈
            //FILO，故左结点会先出栈，实现先序遍历
            else{
                sb.append(cur.val + " ");
                s.push(cur.right);
                s.push(cur.left);
            }
        }
        return sb.toString();
  	}
```



#### CS-Notes 38

------

##### 题目描述

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

##### 总结

[字符串的全排列](<https://www.weiweiblog.cn/permutation/>)

[字符串全排列算法](https://www.cnblogs.com/cxjchen/p/3932949.html>)

- 递归思路：

  1. 固定第一个字符，求后面所有字符的排列
  2. 同样把后面的所有字符拆成两部分（第一个字符和剩下的所有字符），采用递归调用 PermutationHelper（）方法

  这里借助一个 HashSet 保存之前遍历过的字符，用 charSet.contains( chars[j] ) 判断当前值 chars[j] 是否为重复的字符

  递归的过程参照下图

  ![字符串的排列](/C:/Users/dreamwhigh/Desktop/字符串的排列.png)

```java
//T38 字符串的排列
//递归法
//去重的全排列：从第一个字符起，每个字符分别与它后面非重复出现的字符交换
import java.util.*;
public class Solution {
    public ArrayList<String> Permutation(String str) {
        ArrayList<String> list = new ArrayList<>();
        if(str != null && str.length()>0){
            PermutationHelper(str.toCharArray(), 0, list);
            Collections.sort(list);
        }
        return list;
    }
    private void PermutationHelper(char[] chars,int i,ArrayList<String> list){
      	//递归的终止条件，就是i下标已经移到char数组的末尾，添加这一组字符串进入结果集中
        if(i == chars.length-1){
            list.add(String.valueOf(chars));
        }else{
            //建立charSet保存前面出现的字符，用于判断当前值是否为重复的字符
            //在不同的递归过程中，charSet会重建
            Set<Character> charSet = new HashSet<Character>();
            //求所有可能出现在第一个位置的字符
            //即把第一个字符和后面的所有字符交换(相同字符不交换)
            for(int j = i;j<chars.length;j++){
                if(i == j || !charSet.contains(chars[j])){
                    charSet.add(chars[j]);
                    swap(chars, i, j);
                    //固定第一个字符，求后面所有字符的排列
                    //同样把后面的所有字符拆成两部分（第一个字符和剩下的所有字符）
                    //采用递归调用PermutationHelper（）
                    PermutationHelper(chars, i+1, list);
                    //使字符串顺序返回到递归前的状态
                    swap(chars, i, j);
                }
            }
        }
    }
    //交换
    private void swap(char[] str, int i, int j){
        char t = str[i];
        str[i] = str[j];
        str[j] = t;
    }
}


```



#### CS-Notes 39

------

##### 题目描述

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

##### 总结

**Boyer-Moore 算法**
该算法时间复杂度为O(n)，空间复杂度为O(1)，只需要对原数组进行两趟扫描，并且简单易实现。

1. 第一趟扫描我们得到一个候选节点candidate，cnt设为1。对于数组中每一个元素，首先判断count是否为0，若为0，则把candidate设置为当前元素。之后判断candidate是否与当前元素相等，若相等则count+=1，否则count-=1

2. 第二趟扫描我们判断candidate出现的次数是否大于⌊ n/2 ⌋
   

```java
//T39 数组中出现次数超过一半的数字
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        int majority = array[0];
        int cnt=1;
        //第一次遍历
        for(int i=1; i<array.length; i++){
            //cnt为0时，说明当前设定的多数元素被非多数元素抵消掉
            //重新设定当前元素为多数元素，cnt置1
            if(cnt == 0){
                majority=array[i];
                cnt=1;
            }
            //遍历元素和统计元素相等时，cnt++,否则cnt--
            cnt = majority == array[i] ? cnt+1:cnt-1;
        }
        //第二次遍历
        //查看最后设定的多数元素出现次数是否超过一半
        cnt=0;
        for(int val : array){
            if(val == majority) cnt++;
        }
        return cnt > array.length/2 ? majority:0;
    }
}
```



#### CS-Notes 40

------

##### 题目描述

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

##### 总结

两种方法：快速排序中的切分和最大堆

1. 切分，参考 CS-Notes 算法部分对于快速排序的总结
2. 最大堆：
   - **优先队列 PriorityQueue **是使用**小顶堆**（升序）实现的优先队列的存储
   - 通过传入自定义的**Comparator函数**来实现大顶堆，**使用大顶堆（降序）来维护最小堆**
   - 用最大堆保存 k +1 个数，每次第 k+1 个数入堆，会自动进行降序排列（若大于第 k 个数，则继续留在堆顶，否则下沉到相应位置，保证堆顶为当前最大值）；再删除当前堆顶元素

```java
//T40 最小的 K 个数
//切分（快速排序）
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> ret = new ArrayList<Integer> ();
        int N = input.length;
        if(k <= 0 || k > N) return ret;//特殊情况
        int l = 0, h = N-1;
        while(l < h){
            int j = partition(input, l, h);//切分，返回切分元素位置 j
            //切分完成，刚好实现前 k 个位最小元素
            if(j == k-1) break;
            //若切分元素位置大于 k-1，继续对左侧较小数组段进行切分
            else if(j > k-1){
                h = j-1;
            //否则对右侧较大数组段进行切分
            }else{
                l = j+1;
            }
        }
        //将前 k 个最小元素返回
        for(int i = 0;i < k; i++){
            ret.add(input[i]);
        }
        return ret;
    }
    //切分
    private int partition(int [] nums, int l, int h){
        int i = l, j = h+1;
        int v =nums[l];
        while(true){
            while(less(nums[++i], v) && i < h);
            while(less(v, nums[--j]) && j > l);
            if(i >= j) break;
            swap(nums, i, j);
        }
        swap(nums, l, j);
        return j;
    }
    //交换
    private void swap(int [] nums, int i, int j){
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
    //比较
    private boolean less(int i, int j){
        return i < j;
    }
}

//最大堆，降序
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int[] nums, int k) {
        if (k > nums.length || k <= 0)
            return new ArrayList<>();
      	//也可用下方的 lambda 表达式 (o1, o2) -> o2 - o1，表示降序
        //PriorityQueue<Integer> maxHeap = new PriorityQueue<>(k, (o1, o2) -> o2 - o1);
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(k, new Comparator<Integer>(){
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        for (int num : nums) {
            maxHeap.add(num);//每次向最大堆中加入元素，最大堆会自动排序，将最大值放在堆顶
            if (maxHeap.size() > k)
                maxHeap.poll();
        }
        return new ArrayList<>(maxHeap);
    }
}
```



#### CS-Notes 41.1

------

##### 题目描述

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。

##### 总结

大顶堆，用于存储左半边（N为偶数）元素；小顶堆，用于存储右半边（N为奇数）元素

为了保证大顶堆存储的数都小于小顶堆，将偶数次读入的数据先放入小顶堆中，优先队列自动按升序调整，再取出队首的最小值加入大顶堆中。同理可将大值加入到小顶堆中。

```java
//T41 数据流中的中位数
import java.util.PriorityQueue;
public class Solution {
    //大顶堆，用于存储左半边（N为偶数）元素
    PriorityQueue<Integer> left = new PriorityQueue<>((o1, o2) -> o2 - o1);
    //小顶堆，用于存储右半边（N为奇数）元素
    PriorityQueue<Integer> right = new PriorityQueue<>();
    int N = 0;
    public void Insert(Integer num) {
        if(N % 2 == 0){
            right.add(num);//先加入右侧小顶堆中
            left.add(right.poll());//将小顶堆中的最小元素加入左侧
        }else{
            left.add(num);//先加入左侧大顶堆中
            right.add(left.poll());//将大顶堆中的最大元素加入右侧
        }
        N++;
    }

    public Double GetMedian() {
        if(N % 2 == 0){
            return (left.peek() + right.peek()) / 2.0;
        }else
            return (double) left.peek();
    }
}
```



#### CS-Notes 41.2

------

##### 题目描述

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符 "go" 时，第一个只出现一次的字符是 "g"。当从该字符流中读出前六个字符“google" 时，第一个只出现一次的字符是 "l"。

##### 总结

1. 一个字符不会超过 8 位，创建一个大小为256的整形数组，创建一个 queue ，存放只出现一次的字符。

2. insert 时先对该字符所在数组位置数量 +1
3. 判断该位置的值是否为 1，如果为 1，就添加到 queue 中，不为 1，则表示该字符已出现不止1次
4. 检查队首是否有元素已经重复出现，将多次出现的字符移出队列，直至队列符所在数组位置数量为 1（即只出现一次）
5. 取出操作，若队列为 0，则返回 ‘#‘；不为 0 直接返回当前队首字符

```java
//T41.2 字符流中第一个不重复的字符
import java.util.Queue;
import java.util.LinkedList;
public class Solution {
    private int[] cnts = new int [256];//默认初始化为0
    private Queue<Character> q = new LinkedList<>();
    //Insert one char from stringstream
    public void Insert(char ch)
    {
        cnts[ch]++;//ch所在数组位置的值加一
        if(cnts[ch] == 1)
            q.add(ch);//若字符是第一次出现，加入队尾
        while(!q.isEmpty() && cnts[q.peek()] != 1){
            q.poll();//弹出队首已经重复过的字符
        }
    }
  //return the first appearence once char in current stringstream
    public char FirstAppearingOnce()
    {
        //队列为空，返回'#'字符
        return q.isEmpty() ? '#' : q.peek();
    }
}
```



#### CS-Notes 42

------

##### 题目描述

在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和。

##### 总结

题意理解：这里并不要求从第 0 个开始的连续子序列

解题思路：遍历数组，若当前 sum 大于 0 ，则保留，继续加上后一个数；若当前 sum 小于 0 ，则丢弃，将后一个数单独赋值给 sum 。

```java
//T42 连续子数组的最大和
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        if(array == null || array.length == 0) return 0;
        int greatestSum = Integer.MIN_VALUE;//为-2^31
        int sum = 0;
        for(int val : array){
            //若当前 sum 小于0，sum = val
          	//即：sum = Math.max(val, val + sum);
          	//与下方语句同样的含义
            sum = sum < 0 ? val : sum + val;
            greatestSum = Math.max(greatestSum, sum);
        }
        return greatestSum;
    }
}
```



#### CS-Notes 43

------

##### 题目描述

求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）

例如：1~13中包含1的数字有1、10、11、12、13因此共出现6次

##### 总结

对于 n = 3101592

个位 2 ，当个位取 1 时，前 6 位可以取 0~310159 ，共 310160 种情况

十位 9 ，当十位取 1 时，前 5 位可以取 0~31015 ，共 31016 种情况，个位可以取 0~9 ，10 种情况，总共 31016 × 10 种情况

百位 5 ，当百位取 1 时，前 5 位可以取 0~3101 ，共 3102 种情况，个位可以取 0~99 ，100 种情况，总共 3102 × 100 种情况

千位 1 ，当千位取 1 时，前 4 位可以取 0~309 ，共 310 种情况，个位可以取 0~999 ，1000 种情况，其中还需要加上前 4 为取 310 的特殊情况 593 种（  3101000~3101592 ），总共 310 × 1000 + 593  种情况

万位 0 ，当万位取 1 时，前 4 位可以取 0~30 ，共 31 种情况，个位可以取 0~9999 ，10000 种情况，特殊情况为 0 个，总共 31 × 10000 + 0  种情况

...

所以当该位上为 0 或 1 时，前面位数的情况应该减少一种，单独分析这种情况下的后面位数的取值。

(a + 8) / 10 可以只将最低位为 2-9 的数进一位，用于记录高位能够取到的情况数量。

b + 1 用于保存特殊情况下，低位所能取到的情况数量。

```java
//T43 从 1 到 n 整数中 1 出现的次数
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) {
        int cnts = 0;
        for(int m =1; m <= n;m *= 10){
            int a = n/m, b = n%m;
            cnts += (a + 8) / 10 * m + (a % 10 == 1 ? b+1 : 0);
        }
        return cnts;
    }
}
```

#### CS-Notes 45

------

##### 题目描述

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

##### 总结

自定义 comparable 接口：

`Arrays.sort(nums, (s1, s2) -> (s1 + s2).compareTo(s2 + s1));`

根据 `(s1 + s2).compareTo(s2 + s1)` 的返回值，确定 s1, s2 的顺序：

- 返回 0 时，表示两者同样的顺序，不分先后；
- 返回 1 时，表示 s1 > s2，默认是递增排列，故按 s2, s1 顺序排列；
- 返回 -1 时，表示 s1 < s2，按 s1, s2 顺序排列。

```java
import java.util.ArrayList;
import java.util.Arrays;
public class Solution {
    public String PrintMinNumber(int [] numbers) {
        if(numbers == null || numbers.length == 0)
            return "";
        String[] nums = new String[numbers.length];
        for(int i = 0;i < numbers.length;i++){
            nums[i] = numbers[i] + "";//转化为 String 类型保存
        }
        //利用 lambda 表达式自定义 comparable 接口
        //若 s1 + s2 > s2 + s1,则 s2排在前面
        Arrays.sort(nums, (s1, s2) -> (s1 + s2).compareTo(s2 + s1));
        String ret = "";
        for(String s : nums){
            ret += s;
        }
        return ret;
    }
}
```

