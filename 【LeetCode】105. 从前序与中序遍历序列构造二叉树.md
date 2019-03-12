#### 【LeetCode】105. 从前序与中序遍历序列构造二叉树

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

**思路：**

​        树的前序遍历顺序为：根->左->右，中序遍历顺序为：左->中->右，因此，前序遍历的第一个元素必为这棵树的根，既然知道中序遍历中根的位置，则中序遍历中根左边的所有元素构成左子树，右边所有元素构成右子树。则可以对左右子树递归地执行该过程，即可构造出整棵树。

​        这道题的关键是确定前序和中序遍历的开始和结束坐标，假设前序遍历的开始和结束坐标为preStart、preEnd，中序遍历的开始和结束坐标为inStart、inEnd， 则：

​        1、左子树的前序遍历开始结束坐标分别为：preStart+1 、preStart + 1 + (flag-inStart) -1

​        2、左子树的中序遍历开始结束坐标分别为：inStart、flag-1

​        3、右子树的前序遍历开始结束坐标为：preStart + 1 + (flag-inStart)、preEnd

​        4、右子树的中序遍历开始结束坐标为：flag + 1, inEnd

​        本题换成知道知道中序遍历和后序遍历求构建树一样可以用此递归思路解答出来。

**完整解答代码：**

```
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return buildTree(preorder, inorder, 0, preorder.length-1, 0, inorder.length-1);
    }
    public TreeNode buildTree(int [] preorder, int [] inorder, int preStart, int preEnd, int inStart, int inEnd){
        if(preStart > preEnd || inStart > inEnd ){
            return null;
        }
        TreeNode root = new TreeNode(preorder[preStart]);
        int flag = 0;
        for(int i = inStart; i <= inEnd; i++){
            if(inorder[i] == preorder[preStart]){
                flag = i;
                break;
            }
        }
        root.left = buildTree(preorder, inorder, preStart+1, preStart + 1 + (flag-inStart) -1 , inStart, flag-1);
        root.right = buildTree(preorder, inorder, preStart + 1 + (flag-inStart), preEnd, flag + 1, inEnd);
        return root;
    }
}
```

