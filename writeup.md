## 508. Most Frequent Subtree Sum

### Description

Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.

**Note :**
You may assume the sum of values in any subtree is in the range of 32-bit signed integer.

For examples and illustrations visit: https://leetcode.com/problems/most-frequent-subtree-sum/

### Solution
A `subtree` of a tree T is a tree S consisting of a node in T and all of its descendants in T. The subtree corresponding to the root node is the entire tree.

One possible solution comes from the fact that a `postorder traversal` of a tree can be used to compute sums of both the left and the right subtrees ( one or both of them can be empty, in that case we take its sum to be zero) and then these values can be added to the current node's value to form the subtree sum of the current node. This process can be performed recursively as in the code to obtain the sums of all the subtrees.

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
**Formally**,

**Subtree sum of current node = Subtree sum of node's left subtree + Subtree sum of node's right subtree + current node's value**
We run a postorder traversal on the tree, as the postorder traversal runs on the subtrees first, so the subtree sum of both the left and the right subtrees can be calculated first and it is stored in `leftSum` and `rightsum`, and then current node's subtree sum `currentSum` can be calculated as ` currentSum = leftSum + rightSum + root->val`, where root is the current node at any point during the postorder traversal. All these subtee sums are stored in a hashmap ( unordred_map in c++) `freq` and frequency of these sums can be updated in the `freq` hashmap itself.

After the postorder traversal, we iterate over all the values in the hashmap and get the maximum freqency in the hashmap in variable `fmax`. On second iteration we insert all the subtree sum values from the `freq` map into a `result` vector.

![Imgur](https://i.imgur.com/EryWDvg.png)

In the above example, the frequency of subtree sum `4` was maximum that is 2 times and subtree with subtree sum 4 are subtrees of nodes with values `4 and 3`.
```cpp
class Solution {
public:
    unordered_map<int,int> freq;
    int postorder(TreeNode* root){
        if(!root) return 0;
        int leftSum = 0, rightSum = 0;
        leftSum = postorder(root->left);
        rightSum = postorder(root->right);
        int currentSum = leftSum + rightSum + root->val;
        freq[currentSum]++;
        return currentSum;
        
    }
    vector<int> findFrequentTreeSum(TreeNode* root) {
        freq.clear();
        postorder(root);
        int fmax = 0;
        for(auto pr:freq){
            fmax = max(fmax,pr.second);
        }
        vector<int> result;
        for(auto pr:freq){
            if(pr.second == fmax){
                result.push_back(pr.first);
            }
        }
        return result;
    }
};
```

**Complexity Analysis**

- Time complexity: `O(n)`, because we perform `O(1)` operations on `n` nodes.
- Space complexity: `O(n)`, because in worst case, there can be `n` different subtree sums, thus `freq` hashmap will be size of `n`.
Here `n` is the number of nodes in the binary tree.
