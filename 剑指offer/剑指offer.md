<center><b>剑指offer</b></center>

[TOC]

# **JZ1** 二维数组中的查找(可用折半查找)

## 描述
在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
[[1,2,8,9],
 [2,4,9,12],
 [4,7,10,13],
 [6,8,11,15]]
给定 target = 7，返回 true

给定 target = 3，返回 false。



## 思路1

假设arr数组，val，tar如下图所示：
如果我们把二分值定在右上角或者左下角，就可以进行二分。这里以右上角为例，左下角可自行分析：
![图片说明](https://uploadfiles.nowcoder.com/images/20200324/2071677_1585021381982_89033DB5EFA905C7F9FCCA6E59C9BB2C)
1）我么设初始值为右上角元素，arr[0][5] = val，目标tar = arr[3][1]
2）接下来进行二分操作：
3）如果val == target,直接返回
4）如果 tar > val, 说明target在更大的位置，val左边的元素显然都是 < val，间接 < tar，说明第 0 行都是无效的，所以val下移到arr[1][5]
5）如果 tar < val, 说明target在更小的位置，val下边的元素显然都是 > val，间接 > tar，说明第 5 列都是无效的，所以val左移到arr[0][4]
6）继续步骤2)

1. 复杂度分析
   时间复杂度：O(m+n) ，其中m为行数，n为列数，最坏情况下，需要遍历m+n次。
   空间复杂度：O(1)

## 代码1

```java
public boolean Find(int target, int[][] matrix) {
        // 判断数组是否为空
        if(matrix.length == 1 && matrix[0].length == 0){
            return false;
        }
        int rowNum = matrix.length;
        int colNum = matrix[0].length;
        if (colNum == 0) return false;
        int row = 0, col = colNum-1; // 右上角元素
        while (row<rowNum && col>=0) {
            if (target == matrix[row][col]) {
                return true;
            }
            else if (target > matrix[row][col]) {
                ++row;
            }
            else {
                --col;
            }
        }
        return false;
    }
```

## 折半查找算法

### 非递归实现

```java
public static int binarySearch(int[] arr, int key){
        int low = 0;
        int high = arr.length-1;
        int mid = (low+high)/2;
        while (low<=high){
            if (key < arr[mid]){
                high = mid - 1; //为什么减一，因为mid已经用过了
            }else if(key > arr[mid]){
                low = mid + 1;
            }else {
                return mid;
            }
            mid = (high + low) / 2;
        }
        return -1;
    }
```



## 递归实现

```java
public static int binarySearch(int[] dataset, int data, int beginIndex, int endIndex) {
        int midIndex = (beginIndex + endIndex) / 2;
    	// 判断是否不满足
        if (data < dataset[beginIndex] || data > dataset[endIndex] || beginIndex > endIndex) {
            return -1;
        }
        if (data < dataset[midIndex]) {
            return binarySearch(dataset, data, beginIndex, midIndex - 1);
        } else if (data > dataset[midIndex]) {
            return binarySearch(dataset, data, midIndex + 1, endIndex);
        } else {
            return midIndex;
        }
    }
```



# **JZ2** **替换空格**

## 描述

`请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。`

## 思路

采用replace()函数

## 代码

```java
public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param s string字符串 
     * @return string字符串
     */
    public String replaceSpace (String s) {
        // write code here
        String s1 = s.replace(" ","%20");
        return s1;
    }
}
```



# JZ3 从尾到头打印链表

## 描述

输入一个链表的头节点，按链表从尾到头的顺序返回每个节点的值（用数组返回）。

如输入{1,2,3}的链表如下图:

![img](https://uploadfiles.nowcoder.com/images/20210717/557336_1626506480516/103D87B58E565E87DEFA9DD0B822C55F)

返回一个数组为[3,2,1]

0 <= 链表长度 <= 1000

## 方法一（非递归）

### 思路

用两个ArrayList列表

### 代码

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> l = new ArrayList();
        ListNode h = listNode;
        while (h != null){
            l.add(h.val);
            h = h.next;
        }
        ArrayList<Integer> c = new ArrayList();
        for (int i = 0; i < l.size(); i++) {
            c.add(l.get(l.size()-1-i));
        }
        return c;
    }
}
```

### 改进

ArrayList每次add一个数的时候从第一个位置add

```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<>();
        ListNode tmp = listNode;
        while(tmp!=null){
            list.add(0,tmp.val);
            tmp = tmp.next;
        }
        return list;
    }
}
```

## 方法二（递归）

### 思路

因为是倒序打印，可以采用栈的思想，先进后出，而递归就是用到了栈，所以可以采用递归的方法

### 代码

```java
import java.util.ArrayList;
public class Solution {
    ArrayList<Integer> list = new ArrayList();
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if(listNode!=null){
            printListFromTailToHead(listNode.next);
            list.add(listNode.val);
        }
        return list;
    }
}
```



