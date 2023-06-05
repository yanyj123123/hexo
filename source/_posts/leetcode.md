---
title: leetcode
date: 2023-06-02 09:37:35
tags: 
 -leetcode
categories: 
 -leetcode刷题笔记
---

___

___

___

<!--more-->

# 数组

# 链表

# 哈希表

# 字符串

# 双指针法

# 栈和队列

# 二叉树

## 二叉树基本知识

详情见  [二叉树基础](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%A7%8D%E7%B1%BB)

二叉树的构建

<!--more-->

```java
public class treenode{
    int val;
    treenode left;
    treenode right;
    treenode(){};
    treenode(int val){this.val=val;}
    treenode(int val,treenode left,treenode right){
        this.val=val;
        this.left=left;
        this.right=right;
    }
}
```

## 小技巧

一.递归的写法（递归三要素）

1. **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。
2. **确定终止条件：** 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。
3. **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

## 二叉树的遍历

1. 递归法

   1.1 前序遍历   [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

   <!--more-->

   ```java
   /**
    * @Author: 滚筒洗衣机
    * @Date: 2023-06-02 11:11:42
    * @description: 前序递归遍历
    * @return {*}
    */
   class Solution {
       public void preOrder(List<Integer>list,TreeNode root){
           if(root==null){
               return;
           }
           list.add(root.val);
           preOrder(list,root.left);
           preOrder(list, root.right);
       }
       public List<Integer> preorderTraversal(TreeNode root) {
           List<Integer>list=new ArrayList<>();
           preOrder(list, root);
           return list;  
   
       }
   }
   ```

   1.2 中序遍历  [94.二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

   <!--more-->

   ```java
   /**
    * @Author: 滚筒洗衣机
    * @Date: 2023-06-02 11:27:08
    * @description: 二叉树的中序递归遍历
    * @return {*}
    */
   class Solution {
       public void inorder(TreeNode root,List<Integer>list){
           if(root==null){
               return;
           }
           inorder(root.left,list);
           list.add(root.val);
           inorder(root.right, list);
       }
       public List<Integer> inorderTraversal(TreeNode root) {
           List<Integer>list=new ArrayList<>();
           inorder(root, list);
           return list;
       }
   }
   ```

   1.3 后序遍历  [******145.二叉树的后序遍历******](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

   <!--more-->

   ```java
   /**
    * @Author: 滚筒洗衣机
    * @Date: 2023-06-02 11:31:31
    * @description: 二叉树的后序遍历2
    * @return {*}
    */
   class Solution {
       public void postorder(TreeNode root,List<Integer>list){
           if(root==null){
               return;
           }
           postorder(root.left,list);
           postorder(root.right, list);
           list.add(root.val);
           
       }
       public List<Integer> postorderTraversal(TreeNode root) {
           List<Integer>list=new ArrayList<>();
           postorder(root, list);
           return list;
       }
   }
   ```

   

2. 迭代法

   通常是用**栈**来模拟递归

   2.1 前序遍历

   <!--more-->

   ```java
   /**
    * @Author: 滚筒洗衣机
    * @Date: 2023-06-02 16:16:26
    * @description: 使用栈加迭代法进行前序遍历
    * @return {*}
    */
   class Solution {
       public List<Integer> preorderTraversal(TreeNode root) {
           List<Integer> result = new ArrayList<>();
           if (root == null){
               return result;
           }
           Stack<TreeNode> stack = new Stack<>();
           stack.push(root);
           while (!stack.isEmpty()){
               TreeNode node = stack.pop();
               result.add(node.val);
               //因为栈是先进后出，所以需要右孩子先入栈，这样左孩子才能先弹出
               if (node.right != null){
                   stack.push(node.right);
               }
               if (node.left != null){
                   stack.push(node.left);
               }
           }
           return result;
       }
   }
   ```

   2.2 中序遍历

   <!--more-->

   2.3 后序遍历

   前序是左中右，后序是左右中，所以只需在前序遍历代码稍作修改，然后对列表进行反转即可

   对列表进行翻转

   ```java
   Collections.reverse(result);
   ```

   <!--more-->

   ```java
   class Solution {
       public List<Integer> postorderTraversal(TreeNode root) {
           List<Integer> result = new ArrayList<>();
           if (root == null){
               return result;
           }
           Stack<TreeNode> stack = new Stack<>();
           stack.push(root);
           while (!stack.isEmpty()){
               TreeNode node = stack.pop();
               result.add(node.val);
               //由于后序是左右中，所以仅需在前序的基础上调整即可，然后对集合进行翻转
               if (node.left != null){
                   stack.push(node.left);
               }
               if (node.right != null){
                   stack.push(node.right);
               }
           }
           Collections.reverse(result);
           return result;
       }
   }
   ```

   

# 回溯算法

# 贪心算法

# 动态规划

# 单调栈

# 其他

# leetcode周赛

## 第348场

##### 第一题 [最小化字符串长度](https://leetcode.cn/problems/minimize-string-length/)

此题难度不大，其实就是消除相同字母，然后只留一个。可以使用数组或者hashmap作为数据结构来进行存储。

```java
/**
* @Author: 滚筒洗衣机
* @Description: leetcode周赛第三百348场 2716
* @DateTime: 2023-6-5 10:53
* @Params: 
* @Return 
*/
class Solution {
    public int minimizedStringLength(String s) {
        int[]hash=new int[128];
        int result=0;
        for(int i=0;i<s.length();i++){
            if(hash[s.charAt(i)]==0 ){
                hash[s.charAt(i)]++;
            }
            else if(hash[s.charAt(i)]==1 ){
            }
            else{
                hash[s.charAt(i)]--;
            }
        }
        for(int i=0;i<hash.length;i++){
            if(hash[i]==1){
                result++;
            }
        }
        return result;
    }
}
```

##### 第二题 [半有序化排列](https://leetcode.cn/problems/semi-ordered-permutation/)

此题也是简单题，不涉及任何算法，主要就是数学推导公式，需要注意的是分类讨论，即最大的数和最小的数字的位置先后不同，计算方式也不同

```java
/**
* @Author: 滚筒洗衣机
* @Description: leetcode周赛第三百348场 2717
* @DateTime: 2023-6-5 10:54
* @Params: 
* @Return 
*/    
class Solution {
    public int semiOrderedPermutation(int[] nums) {
        int max=nums[nums.length-1];
        int min=nums[0];
        int maxTag=nums.length-1,minTag=0;
        int result=0;
        //找到最大的数及其下标
        for(int i=0;i<nums.length;i++){
            if(nums[i]>max){
                max=nums[i];
                maxTag=i;
            }
        } 
        //找到最小的数及其下标
        for(int i=0;i<nums.length;i++){
            if(nums[i]<min){
                min=nums[i];
                minTag=i;
            }
        }
        //大数在小数前
        if(maxTag>minTag){
            result=minTag+ (nums.length-1-maxTag);
        }
         //大数在小数后
        else {
            result=minTag+ (nums.length-1-maxTag)-1;
        }
        return  result;
    }
}
```

##### 第二题 [查询后矩阵的和](https://leetcode.cn/problems/sum-of-matrix-after-queries/)

此题难度较大，通过率较低，但是题目相对简单，主要是按照普通暴力破解法会导致提交用例超时。

- 首先第一个需要注意的地方是由于后面的数组会覆盖前面的数字，所以只需要从后向前遍历，对于已经赋值的位置不再赋值（因为从后向前，第一次赋值极为最终的值）

- 第二点就是由于只需要计算矩阵的和，不需要关心每个位置的数字，并且每次赋值都是对行或列赋一样的值，所以只需要直接在赋值时进行加法计算

  ```java
  sum+=row*queries[i][2];
  sum+=col*queries[i][2];
  ```

- 第三个注意点就是如何保证已经赋值的位置不再被重新赋值，所以需要使用hashset来记录已经被赋值的行和列

  ```java
  HashSet<Integer>colset=new HashSet<>();
  HashSet<Integer>rowset=new HashSet<>();
  ```

代码如下

```java
   /**
    * @Author: 滚筒洗衣机
    * @Description: leetcode周赛第348场 2718
    * @DateTime: 2023-6-5 11:21
    * @Params: 
    * @Return 
    */
class Solution {
        public long matrixSumQueries(int n, int[][] queries) {
            long sum=0;
            int col=n;
            int row=n;
            HashSet<Integer>colset=new HashSet<>();
            HashSet<Integer>rowset=new HashSet<>();
            for(int i= queries.length-1;i>=0;i--){
                if((queries[i][0]==0)&&!rowset.contains(queries[i][1])){
                    sum+=row*queries[i][2];
                    //代表此位置已经相加，当列需要加此位置时候，忽略此位置
                    col--;
                    rowset.add(queries[i][1]);
                }
                if((queries[i][0]==1)&&!colset.contains(queries[i][1])){
                    sum+=col*queries[i][2];
                    row--;
                    colset.add(queries[i][1]);
                }
            }
            return sum;
        }
}
```
