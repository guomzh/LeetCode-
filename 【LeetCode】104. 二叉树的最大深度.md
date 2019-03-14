#### 【LeetCode】104. 二叉树的最大深度

##### 题目：

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

**示例：**
给定二叉树 `[3,9,20,null,null,15,7]`，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

##### 思路：

1、解法一：由求深度想到二叉树的层次遍历，用bfs广度优先搜索即可，用队列保存需遍历的每层元素，每遍历一层深度加1。

2、解法二：一棵树的最大深度等于它的左子树最大深度和右子树最大深度中的较大者加1，用递归即可解决，暴力简单。

##### 解答代码：

解法一（层次遍历）：

```
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int depth = 0;
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            //该层的元素个数
            int size = queue.size();
            while(size>0){
                TreeNode node = queue.poll();
                if(node.left != null){
                    queue.add(node.left);
                }
                if(node.right!=null){
                    queue.add(node.right);
                }
                size--;
            }
            //遍历完一层了，深度加1
            depth++;
        }
        return depth;
    }

}
```

解法二（递归）：

```
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        return Math.max(maxDepth(root.left),maxDepth(root.right))+1;
    }
}
```

