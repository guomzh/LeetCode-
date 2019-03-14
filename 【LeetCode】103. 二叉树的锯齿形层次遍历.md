#### 【LeetCode】103. 二叉树的锯齿形层次遍历

##### 题目：

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```

##### 思路：

​        层次遍历首先我们会想到用队列来做，这我们已经很熟悉了。题目还要求锯齿形，即“之”字型遍历，也就是奇数层从左到右打印，偶数层从右到左打印。这样只要在层次遍历的同时记录一下深度即可知道是奇数层还是偶数层了。

​        另一种做法是用递归来做，由于先序遍历的顺序是根->左->右，所以每一层都是左边的元素先被遍历到，这样我们也只需记录一下树的深度就可以知道是偶数层还是奇数层，就可以按要求“之”字型打印出这颗树。

​        结果证明，用递归来做击败99.93%的用户，时间效率更高。

##### 解答代码：

方法一（队列层次遍历）：

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        int depth = 1;
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            List<Integer> currentRes = new ArrayList<>();        
            int size = queue.size();
            //把该层的元素依次取出
            while(size > 0){
                TreeNode node = queue.poll();
                TreeNode left = node.left;
                TreeNode right = node.right;
                if(left != null){
                    queue.add(left);
                }
                if(right != null){
                    queue.add(right);
                }
                if(depth % 2 != 0){
                    currentRes.add(node.val);
                } else{
                    currentRes.add(0, node.val);
                }
                size--;
            }
            res.add(currentRes);
            depth++;
        }
        return res;
    }
}
```

方法二（递归）：

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        zigzagHelper(res, root, 0);
        return res;
    }
    public void zigzagHelper (List<List<Integer>> res, TreeNode root, int depth){
        if(root == null){
            return;
        }
        if(res.size() <= depth){
            List<Integer> currentRes = new LinkedList<>();
            res.add(currentRes);
        }
        if(depth %2 == 0){
            res.get(depth).add(root.val);
        } else{
            res.get(depth).add(0, root.val);
        }
        //遍历左子树
        zigzagHelper(res, root.left, depth+1);
        //遍历右子树
        zigzagHelper(res, root.right, depth+1);
    }
}
```

