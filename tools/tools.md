### 编程基本工具

#### 1. 本地编写代码对象简介

为了可以在自己的本地创建并且运行leetcode的代码，我在这里定义了一些公用的类信息，供调试使用：

* 链路节点：

```java
/**
 * 链表的节点
 */
public class ListNode {
    public int val;
    public ListNode next;

    public ListNode(int x) {
        val = x;
    }
}
```

* 树节点

```java
/**
 * 二叉树的节点
 */
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode(int x) {
        val = x;
    }
}
```

#### 2. 头插法创建链表

```java
/**
     * 头插法创建链表
     *
     * @param array 创建的链表信息 [1,2,3,4,5]
     * @return 1->2->3->4->5
     */
    public static ListNode getListByInsertHead(int[] array) {
        ListNode head = null;
        for (int i =array.length-1; i >=0; i--) {
            ListNode node = new ListNode(array[i]);
            node.next = head;
            head = node;
        }
        return head;
    }
```

#### 3. 尾插法创建链表

```java
    /**
     * 尾插法创建链表
     */
    public static ListNode getListNodeByArray(int[] array) {
        ListNode head, cur;
        if (array.length > 0) {
            head = new ListNode(array[0]);
        } else {
            head = null;
        }
        if (null != head) {
            cur = head;
            for (int i = 1; i < array.length; i++) {
                cur.next = new ListNode(array[i]);
                cur = cur.next;
            }
        }
        return head;
    }
```

#### 4 创建带有环路的链表

```java
    // get list with cycle
    public static ListNode getListNodeByArray(int[] array, int pos) {
        if (-1 == pos) {
            return getListNodeByArray(array);
        } else {
            //
            ListNode head, cur = null, posNode = null;
            if (array.length > 0) {
                head = new ListNode(array[0]);
            } else {
                return null;
            }
            if (0 == pos) {
                posNode = head;
            }
            if (null != head) {
                cur = head;
                for (int i = 1; i < array.length; i++) {
                    cur.next = new ListNode(array[i]);
                    cur = cur.next;
                    if (pos == i) {
                        posNode = cur;
                    }
                }
            }
            cur.next = posNode;
            return head;
        }
    }
```

#### 5. 打印链表

```java
    public static void printLinkList(ListNode root) {
        while (null != root) {
            System.out.print(root.val + " ");
            root = root.next;
        }
    }
```

#### 6. 创建二叉树

传入的数组为水平遍历的结果

```java
import java.util.ArrayDeque;

public abstract class TreeUtils {

    /**
     * 创建我们的二叉树
     */
    public static TreeNode createTree(Integer[] treeNodes) {
        if (null == treeNodes || treeNodes.length == 0) return null;
        ArrayDeque<TreeNode> queue = new ArrayDeque<>(16);
        TreeNode returnTreeNode = new TreeNode(treeNodes[0]);
        queue.push(returnTreeNode);
        int i = 1;
        while (!queue.isEmpty()) {
            TreeNode pop = queue.removeLast();
            if (i < treeNodes.length && null != treeNodes[i]) {
                TreeNode left = new TreeNode(treeNodes[i]);
                pop.left = left;
                queue.push(left);
                //i++;
            }
            i++;
            if (i < treeNodes.length && null != treeNodes[i]) {
                TreeNode right = new TreeNode(treeNodes[i]);
                pop.right = right;
                queue.push(right);

            }
            i++;
        }
        return returnTreeNode;
    }
}
```

#### 7 . 创建数组

有的时候，我们需要直接使用数组，但是自己构造比较费事费力。下面的方法，实现了一个可以快速的创建数组的工具：

```java
    // eg: "[[0,6,0],[5,8,7],[0,9,0]]"
    // 返回二维矩阵
    public static int[][] getGridByString(String data) {
        //data = data.substring(2, data.length() - 2);
        String[] items = data.split("],\\[");
        int[][] res = new int[items.length][];
        for (int i = 0; i < items.length; i++) {
            if (i == 0) {
                items[i] = items[i].substring(2);
            }
            if (i == items.length - 1) {
                items[i] = items[i].substring(0, items[i].length() - 2);
            }
            String[] split = items[i].split(",");
            if (split.length == 1 && "".equals(split[0])) {
                res[i] = new int[0];
            } else {
                res[i] = new int[split.length];
                if (split.length > 0) {
                    for (int j = 0; j < split.length; j++) {
                        if (!"".equals(split[j])) {
                            res[i][j] = Integer.valueOf(split[j]);
                        }
                    }
                }
            }
        }
        return res;
    }
```

