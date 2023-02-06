### 输入方式
#### Scanner 输入
```java
import java.util.*;

public class Main{
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt(); // String: next(), double: nextDouble()
        int[] nums = new int[n];
        for (int i = 0; i < n; i++)
            nums[i] = scan.nextInt();
        // ...
    }
}
```
#### BufferedReader输入
BufferedReader可以读一行，速度比Scanner快，所以数据较多的时候使用。注意BufferedReader用完记得关。
```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(reader.readLine());
        int[] nums = new int[n];
        String[] strs = reader.readLine().split(" ");
        for (int i = 0; i < n; i++)
            nums[i] = Integer.parseInt(strs[i]);
        // ...
        reader.close(); // 记得关闭
    }
}
```

### 多行输入
#### 题目
小v今年有n门课，每门都有考试，为了拿到奖学金，小v必须让自己的平均成绩至少为avg。
每门课由平时成绩和考试成绩组成，满分为r。
现在他知道每门课的平时成绩为ai ,若想让这门课的考试成绩多拿一分的话，小v要花bi 的时间复习，不复习的话当然就是0分。
同时我们显然可以发现复习得再多也不会拿到超过满分的分数。为了拿到奖学金，小v至少要花多少时间复习。
```shell
输入描述:
第一行三个整数n,r,avg(n大于等于1小于等于1e5，r大于等于1小于等于1e9,avg大于等于1小于等于1e6)，
接下来n行，每行两个整数ai和bi，均小于等于1e6大于等于1
示例1
 输入
5 10 9
0 5
9 1
8 1
0 1
9 100
//Scanner类默认的分隔符就是空格
```
#### 题解
```java
import java.util.Arrays;
import java.util.Scanner;

public class MultilineInput {
	public static void main(String[] args) {
		Scanner sc=new Scanner(System.in);
        while(sc.hasNext()){
            int n=sc.nextInt();
            int full=sc.nextInt();
            int avg=sc.nextInt();
            int[][] nums=new int[n][2];
            for(int i=0;i<n;i++){
                nums[i][0]=sc.nextInt();
                nums[i][1]=sc.nextInt();
            }
            //假定不会出现拿不到奖学金的情况
            if (n==1){
                System.out.println((avg-nums[0][0])*nums[0][1]);
                continue;
            }
            Arrays.sort(nums, (o1, o2) -> o1[1] - o2[1]);//按复习代价从小到大排序
            long sum=0;
            for(int[] a:nums) {
                sum+=a[0];
            }
            long limit=avg*n;
            int index=0;
            long time=0;
            while(sum<limit){
                int tmp=full-nums[index][0];
                //如果一门课程复习到满分，小于限制
                if(tmp+sum<=limit){                
                    time+=tmp*nums[index][1];
                    sum+=tmp;
                    index++;
                }
                //如果一门课程复习到满分，大于限制
                else{
                    time+=(limit-sum)*nums[index][1];
                    sum=limit;
                }
            }
            // 输出描述:
            // 一行输出答案。
            // 输出
            // 43
            System.out.println(time);
        }
	}
}
```

### 数组输入（分隔符为空格）
#### 题目
一条长l的笔直的街道上有n个路灯，若这条街的起点为0，终点为l，第i个路灯坐标为ai ，
每盏灯可以覆盖到的最远距离为d，为了照明需求，所有灯的灯光必须覆盖整条街，
 但是为了省电，要使这个d最小，请找到这个最小的d。
  输入描述:
  每组数据第一行两个整数n和l（n大于0小于等于1000，l小于等于1000000000大于0）。
第二行有n个整数(均大于等于0小于等于l)，为每盏灯的坐标，多个路灯可以在同一点。
输入
```shell
7 15
15 5 3 7 9 14 0
```

#### 题解
```java
public class ArrayInput {
	public static void main(String[] args){
		Scanner sc=new Scanner(System.in);
        while(sc.hasNext()){
            int n=sc.nextInt();
            long l=sc.nextLong();
            long[] nums=new long[n];
            for(int i=0;i<n;i++){
                nums[i]=sc.nextLong();
            }
            Arrays.sort(nums);
            long gap=nums[1]-nums[0];
            for(int i=1;i<n;i++){
                gap=Math.max(gap,nums[i]-nums[i-1]);
            }
            //下标最小和最大位置的路灯需要单独判断
            //如下标为3是最小，那么0-3这一段必须被覆盖到，所以最小下标必须单独判断
            gap=Math.max(gap,nums[0]*2);
            gap=Math.max(gap,(l-nums[n-1])*2);
            // 输出描述:
            // 输出答案，保留两位小数。
            // 输出
            // 2.50
            System.out.println(String.format("%.2f",gap/2.0));
        }
    }
}
```

### 输入为一个链表
#### 题目
对于一个链表 L: L0→L1→…→Ln-1→Ln,
将其翻转成 L0→Ln→L1→Ln-1→L2→Ln-2→…
先构建一个节点类，用于链表构建
#### 题解
```java
import java.util.Scanner;
import java.util.Stack;

public class LinkListInput {
    //题目描述
    //对于一个链表 L: L0→L1→…→Ln-1→Ln,
    //将其翻转成 L0→Ln→L1→Ln-1→L2→Ln-2→…

    //先构建一个节点类，用于链表构建
    static class LinkNode {
        int val;
        LinkNode next;
        public LinkNode(int val){
            this.val = val;
        }
    }

    public static void main(String[] args){
        //输入是一串数字，请将其转换成单链表格式之后，再进行操作
        //输入描述:
        //一串数字，用逗号分隔
        //输入
        //1,2,3,4,5
        Scanner scanner = new Scanner(System.in);
        //以字符串形式作为输入
        String str = scanner.next().toString();
        //通过分隔符将其转为字符串数组
        String[] arr  = str.split(",");
        //初始化一个整数数组
        int[] ints = new int[arr.length];
        //给整数数组赋值
        for(int j = 0; j<ints.length;j++) {
            ints[j] = Integer.parseInt(arr[j]);
        }
        Stack<LinkNode> stack = new Stack<>();
        LinkNode head = new LinkNode(0);
        LinkNode p = head;
        //链表初始化并放入stack中
        for(int i = 0; i < ints.length; i++){
            p.next = new LinkNode(ints[i]);
            p = p.next;
            stack.add(p);
        }
        head = head.next;
        //开始链表转换
        p = head;
        LinkNode q = stack.peek();
        while ((!p.equals(q)) && (!p.next.equals(q))) {
            q = stack.pop();
            q.next = p.next;
            p.next = q;
            p = p.next.next;
            q = stack.peek();
        }
        q.next = null;
        //输出
        //1,5,2,4,3
        //打印
        while (head != null) {
            if(head.next == null){
                System.out.print(head.val);
            }else{
                System.out.print(head.val + ",");
            }
            head = head.next;
        }
    }
}

```

### 树的输入
#### 题目
输入描述:
第一行两个数n,root，分别表示二叉树有n个节点，第root个节点时二叉树的根
接下来共n行，第i行三个数val_i,left_i,right_i，
分别表示第i个节点的值val是val_i,左儿子left是第left_i个节点，右儿子right是第right_i个节点。节点0表示空。
1<=n<=100000,保证是合法的二叉树
```shell
输入
5 1
5 2 3
1 0 0
3 4 5
4 0 0
6 0 0
```
#### 题解
```java
package learnACM;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Stack;

//题目描述
//给定一个二叉树，判断其是否是一个有效的二叉搜索树。
//假设一个二叉搜索树具有如下特征：
//节点的左子树只包含小于当前节点的数。
//节点的右子树只包含大于当前节点的数。
//所有左子树和右子树自身必须也是二叉搜索树。
//例如：
//输入：
//    5
//   / \
//  1   3
//     / \
//    4   6
//输出: false

//构造树需要的结点类
class TreeNode {
    TreeNode left, right;
    int val;

    public TreeNode(int val) {
        this.val = val;
    }
}

public class TreeInput {
    public static void main(String[] args) throws IOException {

        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        String[] s = reader.readLine().split(" ");
        int n = Integer.parseInt(s[0]);
        int root = Integer.parseInt(s[1]);
        TreeNode[] tree = new TreeNode[n + 1];
        int[][] leaf = new int[n + 1][2];
        for (int i = 1; i <= n; i++) {
            String[] ss = reader.readLine().split(" ");
            int val_i = Integer.parseInt(ss[0]);
            int left_i = Integer.parseInt(ss[1]);
            int right_i = Integer.parseInt(ss[2]);
            TreeNode node = new TreeNode(val_i);
            leaf[i][0] = left_i;
            leaf[i][1] = right_i;
            tree[i] = node;
        }
        for (int i = 1; i <= n; i++) {
            int left = leaf[i][0];
            if (left != 0) {
                tree[i].left = tree[left];
            } else {
                tree[i].left = null;
            }
            int right = leaf[i][1];
            if (right != 0) {
                tree[i].right = tree[right];
            } else {
                tree[i].right = null;
            }
        }
        TreeNode head = tree[root];
        boolean flag = isBinarySearchTree(head);
        System.out.println(flag);

    }

    private static boolean isBinarySearchTree(TreeNode node) {
        if(node == null){
            return true;
        }
        int pre = Integer.MIN_VALUE;
        Stack<TreeNode> s = new Stack<>();

        while(!s.isEmpty() || node != null){
            while(node != null){
                s.push(node);
                node = node.left;
            }
            node = s.pop();
            if(node == null){
                break;
            }
            if(pre > node.val){
                return false;
            }
            pre = node.val;
            node = node.right;
        }
        return true;
    }
}
```