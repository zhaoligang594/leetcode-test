### 二叉树定义以及相关操作

#### 1. 二叉树的定义

> 二叉树（binary tree）是指树中节点的度不大于2的有序树，它是一种最简单且最重要的树。二叉树的递归定义为：二叉树是一棵空树，或者是一棵由一个根节点和两棵互不相交的，分别称作根的左子树和右子树组成的非空树；左子树和右子树又同样都是二叉树。
>
> 满二叉树：如果一棵二叉树只有度为0的结点和度为2的结点，并且度为0的结点在同一层上，则这棵二叉树为满二叉树。
>
> 完全二叉树：深度为k，有n个结点的二叉树当且仅当其每一个结点都与深度为k的满二叉树中编号从1到n的结点一一对应时，称为完全二叉树。完全二叉树的特点是叶子结点只可能出现在层序最大的两层上，并且某个结点的左分支下子孙的最大层序与右分支下子孙的最大层序相等或大1。

#### 2. 二叉树的性质

* 性质1：二叉树的第i层上至多有2i-1（i≥1）个节点 。

* 性质2：深度为h的二叉树中至多含有2h-1个节点。

* 性质3：若在任意一棵二叉树中，有n0个叶子节点，有n2个度为2的节点，则必有n0=n2+1。

* 性质4:：具有n个节点的完全二叉树深为

$$
log_2x+1
$$

（其中x表示不大于n的最大整数）

* 性质5：若对一棵有n个节点的完全二叉树进行顺序编号（1≤i≤n），那么，对于编号为i（i≥1）的节点：

  当i=1时，该节点为根，它无双亲节点 。

  当i>1时，该节点的双亲节点的编号为i/2 。

  若2i≤n，则有编号为2i的左节点，否则没有左节点。

  若2i+1≤n，则有编号为2i+1的右节点，否则没有右节点。

#### 3. 定义二叉树的节点

> 根据上面的节点的定义，二叉树的节点内部属性分为左子树、右子树以及当前节点的值。

* 节点定义

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

* 构建二叉树

> 输入的数组是水平遍历的序列

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

#### 4. 二叉树的遍历

> 二叉树的遍历分为 深度优先遍历 和 广度优先遍历。同时也分为 递归遍历 以及 迭代遍历。

访问节点函数：

```java
    // 访问节点
    private static void visit(TreeNode node) {
        // to do something
        System.out.println(node.val);
    }
```



* 深度优先遍历

```java
    /**
     * 深度优先 先序遍历
     *
     * @param root 根节点
     */
    public static void preOrderDfs(TreeNode root) {
        if (null != root) {
            // 访问节点信息 处理相关操作
            visit(root);
            preOrderDfs(root.left);
            preOrderDfs(root.right);
        }
    }
    
```



```java
    /**
     * 深度优先 中序遍历
     *
     * @param root 根节点
     */
    public static void inOrderDfs(TreeNode root) {
        if (null != root) {
            inOrderDfs(root.left);
            // 访问节点信息 处理相关操作
            visit(root);
            inOrderDfs(root.right);
        }
    }
```



```java
    /**
     * 深度优先 后序遍历
     *
     * @param root 根节点
     */
    public static void postOrderDfs(TreeNode root) {
        if (null != root) {
            postOrderDfs(root.left);
            postOrderDfs(root.right);
            // 访问节点信息 处理相关操作
            visit(root);
        }
    }
```



> 上面分别介绍了树的深度优先遍历的3种遍历的方式，整体上说，也就是，什么时候访问节点信息，结果导致了 是 先序、中序或者是后序的遍历。

* 广度优先遍历

> 广度优先遍历也称为水平遍历，也就是按照从上到下、从左到右的依次访问遍历。在遍历期间，我们要使用队列这个数据结构。帮助我们保存前面所遍历的信息。



```java
    /**
     * 广度优先 水平遍历
     *
     * @param root 跟节点
     */
    public static void lfs(TreeNode root) {
        // 创建队列对象
        Deque<TreeNode> queue = new ArrayDeque<>();
        if (null != root) {
            queue.addLast(root);
            while (!queue.isEmpty()) {
                TreeNode node = queue.pollFirst();
                visit(node);
                if (null != node.left) queue.addLast(node.left);
                if (null != node.right) queue.addLast(node.right);
            }
        }
    }
```



* 迭代遍历

> 在我们深度遍历的过程中，我们使用递归实现遍历的算法。递归的本质是 操作系统使用堆栈来实现上下文信息的保存，所以要实现迭代的算法，我们需要使用栈的数据结构。

```java 
    // 先序遍历的操作
    public static void proOrderUnRecur(TreeNode head) {
        System.out.println("pro-order");
        if (null != head) {
            Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
            stack.addLast(head);
            while (!stack.isEmpty()) {
                head = stack.pollLast();
                System.out.print(head.val + " ");
                if (head.right != null) stack.addLast(head.right);
                if (head.left != null) stack.addLast(head.left);
            }
        }
        System.out.println();
    }
```



```java
    // 中序遍历
    public static void inOrderUnRecur(TreeNode head) {
        System.out.println("in-order");
        if (null != head) {
            Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
            while (!stack.isEmpty() || head != null) {
                if (null != head) {
                    stack.addLast(head);
                    head = head.left;
                } else {
                    head = stack.pollLast();
                    System.out.print(head.val + " ");
                    head = head.right;
                }
            }
        }
        System.out.println();
    }
```



```java 
    // 后序遍历
    public static void posOrderUnRecur(TreeNode head) {
        System.out.println("pos-order");
        if (null != head) {
            Deque<TreeNode> stack1 = new ArrayDeque<TreeNode>();
            Deque<TreeNode> stack2 = new ArrayDeque<TreeNode>();
            stack1.addLast(head);
            while (!stack1.isEmpty()) {
                head = stack1.pollLast();
                stack2.addLast(head);
                if (head.left != null) stack1.addLast(head.left);
                if (head.right != null) stack1.addLast(head.right);

            }
            while (!stack2.isEmpty()) {
                System.out.print(stack2.pollLast().val + " ");
            }
        }
        System.out.println();
    }
```

> 后续遍历，使用了2个栈来进行实现的。因为 先序遍历是NLR  后续遍历是 LRN 那么就可以看成 NRL 也就是 特殊的先序遍历 NRL 也就是 在遍历的过程中 先访问 右节点 就可以满足我们的遍历的要求。

#### 5. 二叉树的序列化与反序列化

> 序列话与反序列化在平时的工作中是经常用到的，因为可以临时的帮助我们存储某些结果，防止我们重新的计算。同时也是克服两个服务端不同的实现的预言进行通信的桥梁。



```java
    /**
     * 序列化的操作
     *
     * @param root 二叉树的根结点
     * @return
     */
    public static String serialTree(TreeNode root) {
        if (null == root) return "#_";
        return root.val + "_" + serialTree(root.left) + serialTree(root.right);
    }
```



```java
    /**
     * 反序列化树结构
     *
     * @param data
     * @return
     */
    public static TreeNode reConTree(String data) {
        if (null == data || "".equals(data)) return null;
        String[] split = data.split("_");
        Deque<String> queue = new ArrayDeque<>();
        for (String str : split) {
            queue.addLast(str);
        }
        return reConTree(queue);
    }

    private static TreeNode reConTree(Deque<String> queue) {
        TreeNode res = null;
        if (!queue.isEmpty()) {
            String s = queue.pollFirst();
            if ("#".equals(s)) return null;
            res = new TreeNode(Integer.valueOf(s));
            res.left = reConTree(queue);
            res.right = reConTree(queue);
        }
        return res;
    }
```

