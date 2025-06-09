### 1. Level order traversal
Given a root of a binary tree with n nodes, the task is to find its level order traversal. Level order traversal of a tree is breadth-first traversal for the tree.

```cpp
class Solution {
  public:
    vector<vector<int>> levelOrder(Node *root) {
        queue<Node*>q;
        q.push(root);
        vector<vector<int>>res;
        while(!q.empty())
        {
            int n=q.size();
            vector<int>temp;
            while(n--){
                Node* node=q.front();
                q.pop();
                temp.push_back(node->data);
                if(node->left)
                q.push(node->left);
                if(node->right)
                q.push(node->right);
            }
            res.push_back(temp);
        }
        return res;
    }
};
```

### 2. Height of Binary Tree
Given a binary tree, find its height.

The height of a tree is defined as the number of edges on the longest path from the root to a leaf node. A leaf node is a node that does not have any children.

```cpp
class Solution {
  public:
    void inorder(Node*root,int cnt,int &h){
        if(!root)
        return ;
        if(root->left==NULL && root->right==NULL){
            h=max(h,cnt);
            return ;
        }
        inorder(root->left,cnt+1,h);
        inorder(root->right,cnt+1,h);
    }
    int height(Node* node) {
        int cnt=0,h=0;
        inorder(node,cnt,h);
        return h;
    }
};
```

### 3. Diameter of a Binary Tree
Given a binary tree, the diameter (also known as the width) is defined as the number of edges on the longest path between two leaf nodes in the tree. This path may or may not pass through the root. Your task is to find the diameter of the tree.

```cpp
class Solution {
    int res=0;
  public:
    int solve(Node* root) {
        if(!root)
        return 0;
        int lHeight=solve(root->left);
        int rHeight=solve(root->right);
        res=max(res,lHeight+rHeight);
        return 1+max(lHeight,rHeight);
    }
    int diameter(Node* root) {
        solve(root);
        return res;
    }
};
```
