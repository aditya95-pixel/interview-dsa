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

### 4. Mirror Tree
Given a binary tree, convert the binary tree to its Mirror tree.

Mirror of a Binary Tree T is another Binary Tree M(T) with left and right children of all non-leaf nodes interchanged.

```cpp
class Solution {
  public:
    // Function to convert a binary tree into its mirror tree.
    Node* solve(Node* node){
        if(!node)
        return NULL;
        Node* dummy=node->left;
        node->left=solve(node->right);
        node->right=solve(dummy);
        return node;
    }
    void mirror(Node* node) {
        solve(node);
    }
};
```

### 5. Construct Tree from Inorder & Preorder
Given two arrays representing the inorder and preorder traversals of a binary tree, construct the tree and return the root node of the constructed tree.

Note: The output is written in postorder traversal.

```cpp
class Solution {
  public:
    // Function to build the tree from given inorder and preorder traversals
    map<int,int>mp;
    Node* solve(vector<int>&preorder,int &preIndex,int left,int right){
        if(left>right)
        return NULL;
        int rootVal=preorder[preIndex];
        preIndex++;
        int index=mp[rootVal];
        Node* root=new Node(rootVal);
        root->left=solve(preorder,preIndex,left,index-1);
        root->right=solve(preorder,preIndex,index+1,right);
        return root;
    }
    Node *buildTree(vector<int> &inorder, vector<int> &preorder) {
        for(int i=0;i<inorder.size();i++)
        mp[inorder[i]]=i;
        int preIndex=0;
        return solve(preorder,preIndex,0,inorder.size()-1);
    }
};
```

### 6. Inorder Traversal
Given a Binary Tree, your task is to return its In-Order Traversal.

An inorder traversal first visits the left child (including its entire subtree), then visits the node, and finally visits the right child (including its entire subtree).

Follow Up: Try solving this with O(1) auxiliary space.

```cpp
class Solution {
  public:
    // Function to return a list containing the inorder traversal of the tree.
    void inorder(Node* root,vector<int>&v){
        if(!root)
        return ;
        inorder(root->left,v);
        v.push_back(root->data);
        inorder(root->right,v);
    }
    vector<int> inOrder(Node* root) {
        vector<int>v;
        inorder(root,v);
        return v;
    }
};
```
