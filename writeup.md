
## 1457. Pseudo-Palindromic Paths in a Binary Tree

### Description

Given a binary tree where node values are digits from 1 to 9. A path in the binary tree is said to be **pseudo-palindromic** if at least one permutation of the node values in the path is a palindrome.

*Return the number of pseudo-palindromic paths going from the root node to leaf nodes.*

**Constraints:**
- The given binary tree will have between 1 and 10^5 nodes.
- Node values are digits from 1 to 9.

For examples and illustrations visit: https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree/

### Solution

The definition of `TreeNode` used for this problem is the following:

```cpp
 struct TreeNode {
     int val;
     TreeNode *left;
     TreeNode *right;
     TreeNode() : val(0), left(nullptr), right(nullptr) {}
     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 };
```

A *palindromic sequence* is a sequence that remains unchanged when its elements are displayed in reversed order (e.g. abaaba, 12321). If the elements of a sequence can be rearranged in such a way that the resulting sequence is palindromic, we say the original sequence is pseudo-palindromic (e.g. 02250 is pseudo-palindromic because it is a permutation of 02520). In this problem we are required to find all pseudo-palindromic paths going from the root node to the leaf nodes of a binary tree, where the nodes' values are digits from 1 to 9.

One possible solution comes from the observation that in a palindrome, there can only exist one element with an odd number of occurrences. If there are two or more elements with an odd frequency in the path, it is impossible to form a palindrome out of it, such as in 192991. We can thus perform a Depth First Search (DFS) traversal of the tree, keeping track of the digit frequencies to verify the above condition for all possible root-to-leaf paths. To do this we define a vector `frequency` such that `frequency[k]` gives the number of occurrences of digit `k` (`0 <= k <= 9`) in the path (see figure below). The traversed path is pseudo-palindromic if `frequency[k]` is odd for a unique `k`. Counting all paths for which this property holds yields the desired result.

![](https://i.imgur.com/zkvwIqI.jpg)

```cpp
class Solution {
public:
  static void DFS(TreeNode* root, vector<int> &frequency, int &path_count) {
    if (root == nullptr) 
      return ;
    
    frequency[root -> val]++;
    if (root -> left == nullptr && root -> right == nullptr) {
      int odd = 0;
      for (int k = 0; k < 10; k++) {
        if (frequency[k] % 2) {
          odd++;
        }
      }
      if (odd <= 1) {
        path_count++;
      }
    }
    if (root -> left != nullptr) {
      DFS(root -> left, frequency, path_count);
    }
    if (root -> right != nullptr) {
      DFS(root -> right, frequency, path_count);
    }
    frequency[root -> val]--;
  }
  
  int pseudoPalindromicPaths (TreeNode* root) {
    int path_count = 0;
    vector<int> frequency(10);
    DFS(root, frequency, path_count);
    return path_count;
  }
};

```

**Complexity Analysis**

- Time complexity: $O(n)$, because we perform $O(1)$ operations on $n$ nodes.
- Space complexity: $O(h+d)$, where $h$ is the height of the tree and $d$ is the number of digits (here $d = 10$).
