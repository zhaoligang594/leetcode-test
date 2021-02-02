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

