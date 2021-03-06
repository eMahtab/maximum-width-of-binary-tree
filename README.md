# Maximum Width of Binary Tree
## https://leetcode.com/problems/maximum-width-of-binary-tree

Given a binary tree, write a function to get the maximum width of the given tree. The width of a tree is the maximum width among all levels. The binary tree has the same structure as a full binary tree, but some nodes are null.

The width of one level is defined as the length between the end-nodes (the leftmost and right most non-null nodes in the level, where the null nodes between the end-nodes are also counted into the length calculation.
```
Example 1:

Input: 

           1
         /   \
        3     2
       / \     \  
      5   3     9 

Output: 4
Explanation: The maximum width existing in the third level with the length 4 (5,3,null,9).

Example 2:

Input: 

          1
         /  
        3    
       / \       
      5   3     

Output: 2
Explanation: The maximum width existing in the third level with the length 2 (5,3).

Example 3:

Input: 

          1
         / \
        3   2 
       /        
      5      

Output: 2
Explanation: The maximum width existing in the second level with the length 2 (3,2).

Example 4:

Input: 

          1
         / \
        3   2
       /     \  
      5       9 
     /         \
    6           7
Output: 8
Explanation:
The maximum width existing in the fourth level with the length 8 (6,null,null,null,null,null,null,7).
```

**Note: Answer will be in the range of 32-bit signed integer.**

## Implementation 1 : Iterative , Time : O(nodes in tree) , Space : O(nodes in tree)
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    class Node {
        TreeNode treeNode;
        int position;
        Node(TreeNode treeNode, int position) {
            this.treeNode = treeNode; this.position = position;
        }
    }
    public int widthOfBinaryTree(TreeNode root) {
        if(root == null)
            return 0;
        Queue<Node> q = new LinkedList<>();
        int maxWidth = 1;
        q.add(new Node(root, 0));
        while(!q.isEmpty()) {
            int size = q.size();
            int left = 0, right = 0;
            for(int i = 0; i < size; i++) {
                Node node = q.remove();
                if(i == 0)
                    left = node.position;
                if(i == size-1)
                    right = node.position;
                if(node.treeNode.left != null)
                    q.add(new Node(node.treeNode.left, 2 * node.position));
                if(node.treeNode.right != null)
                    q.add(new Node(node.treeNode.right, 2 * node.position + 1));
            }
            int width = (right - left) + 1;
            maxWidth = Math.max(maxWidth, width);
        }
       return maxWidth; 
    }
}
```

## Implementation 2 : Recursive , Time : O(nodes in tree) , Space : O(nodes in tree)

```java

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
    int maxWidth = 0;
    Map<Integer, Integer> leftmostNodePosition = new HashMap<>();
    
    public int widthOfBinaryTree(TreeNode root) {
        if(root == null)
            return 0;
        dfs(root, 0, 0);
        return maxWidth;
    }
    public void dfs(TreeNode root, int depth, int position) {
        if (root == null) return;
        leftmostNodePosition.putIfAbsent(depth, position);
        maxWidth = Math.max(maxWidth, position - leftmostNodePosition.get(depth) + 1);
        dfs(root.left, depth + 1, 2 * position);
        dfs(root.right, depth + 1, 2 * position + 1);
    }
}

```

# References :
https://www.youtube.com/watch?v=sm4UdCO2868
