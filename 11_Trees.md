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

### 7. Tree Boundary Traversal
Given a Binary Tree, find its Boundary Traversal. The traversal should be in the following order: 

Left Boundary: This includes all the nodes on the path from the root to the leftmost leaf node. You must prefer the left child over the right child when traversing. Do not include leaf nodes in this section.

Leaf Nodes: All leaf nodes, in left-to-right order, that are not part of the left or right boundary.

Reverse Right Boundary: This includes all the nodes on the path from the rightmost leaf node to the root, traversed in reverse order. You must prefer the right child over the left child when traversing. Do not include the root in this section if it was already included in the left boundary.

Note: If the root doesn't have a left subtree or right subtree, then the root itself is the left or right boundary. 

```cpp
class Solution {
  public:
    bool isLeaf(Node*root){
        return (!root->left && !root->right);
    }
    void collectLeaves(Node*root,vector<int>&res){
        if(!root)
        return ;
        if(isLeaf(root))
        {
            res.push_back(root->data);
            return ;
        }
        collectLeaves(root->left,res);
        collectLeaves(root->right,res);
    }
    void collectRight(Node*root,vector<int>&res){
        if(!root)
        return ;
        int idx=res.size();
        while(root && !isLeaf(root)){
            res.push_back(root->data);
            if(root->right)
            root=root->right;
            else
            root=root->left;
        }
        reverse(res.begin()+idx,res.end());
    }
    void collectLeft(Node*root,vector<int>&res){
        if(!root)
        return ;
        while(root && !isLeaf(root)){
            res.push_back(root->data);
            if(root->left)
            root=root->left;
            else
            root=root->right;
        }
    }
    vector<int> boundaryTraversal(Node *root) {
        vector<int>res;
        if(!root)
        return res;
        if(!isLeaf(root))
        res.push_back(root->data);
        collectLeft(root->left,res);
        collectLeaves(root,res);
        collectRight(root->right,res);
        return res;
    }
};
```

### 8. Maximum path sum from any node
Given a binary tree, the task is to find the maximum path sum. The path may start and end at any node in the tree.

```cpp
class Solution {
  public:
    // Function to return maximum path sum from any node in a tree.
    int res=INT_MIN;
    int solve(Node* root){
        if(!root)
        return 0;
        int l=max(0,solve(root->left));
        int r=max(0,solve(root->right));
        res=max(res,l+r+root->data);
        return root->data+max(l,r);
    }
    int findMaxSum(Node *root) {
        solve(root);
        return res;
    }
};
```

### 9. K Sum Paths
Given a binary tree and an integer k, determine the number of downward-only paths where the sum of the node values in the path equals k. A path can start and end at any node within the tree but must always move downward (from parent to child).

```cpp
class Solution {
  public:
    int solve(Node*root,int currsum,int k,map<int,int>&mp){
        if(!root)
        return 0;
        int cnt=0;
        currsum+=root->data;
        if(currsum==k)
        cnt++;
        cnt+=mp[currsum-k];
        mp[currsum]++;
        cnt+=solve(root->left,currsum,k,mp);
        cnt+=solve(root->right,currsum,k,mp);
        mp[currsum]--;
        return cnt;
    }
    int sumK(Node *root, int k) {
        int currsum=0;
        map<int,int>mp;
        return solve(root,currsum,k,mp);
    }
};
```

### 10. Check for BST
Given the root of a binary tree. Check whether it is a BST or not.
Note: We are considering that BSTs can not contain duplicate Nodes.
A BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

```cpp
class Solution {
  public:
    // Function to check whether a Binary Tree is BST or not.
    void inorder(Node*root,vector<int>&res){
        if(!root)
        return ;
        inorder(root->left,res);
        res.push_back(root->data);
        inorder(root->right,res);
    }
    bool isBST(Node* root) {
        vector<int>res;
        inorder(root,res);
        for(int i=0;i<res.size()-1;i++)
        {
            if(res[i]>=res[i+1])
            return false;
        }
        return true;
    }
};
```

### 11. k-th Smallest in BST
Given a BST and an integer k, the task is to find the kth smallest element in the BST. If there is no kth smallest element present then return -1.

```cpp
class Solution {
  public:
    // Return the Kth smallest element in the given BST
    int ele=-1;
    void inorder(Node*root,int k,int& cnt){
        if(!root)
        return ;
        inorder(root->left,k,cnt);
        cnt++;
        if(cnt==k)
        {
            ele=root->data;
            return ;
        }
        inorder(root->right,k,cnt);
    }
    int kthSmallest(Node *root, int k) {
        int cnt=0;
        inorder(root,k,cnt);
        return ele;
    }
};
```

### 12. Pair Sum in BST
Given a Binary Search Tree(BST) and a target. Check whether there's a pair of Nodes in the BST with value summing up to the target. 

```cpp
class Solution {
  public:   
    bool solve(Node*root,int target,map<int,int>&mp){
        if(!root)
        return false;
        bool left=solve(root->left,target,mp);
        if(left)
        return true;
        if(mp.find(target-root->data)!=mp.end())
        return true;
        mp[root->data]++;
        bool right=solve(root->right,target,mp);
        if(right)
        return true;
        return false;
    }
    bool findTarget(Node *root, int target) {
        map<int,int>mp;
        return solve(root,target,mp);
    }
};
```

### 13. Fixing Two nodes of a BST
Given the root of a Binary search tree(BST), where exactly two nodes were swapped by mistake. Your task is to fix (or correct) the BST by swapping them back. Do not change the structure of the tree.
Note: It is guaranteed that the given input will form BST, except for 2 nodes that will be wrong. All changes must be reflected in the original Binary search tree(BST).

```cpp
class Solution {
  public:
    void solve(Node*root,Node*&first,Node*&middle,Node*&last,Node*&prev){
        if(!root)
        return ;
        solve(root->left,first,middle,last,prev);
        if(prev && prev->data>root->data){
            if(!first)
            {
                first=prev;
                middle=root;
            }
            else
                last=root;
        }
        prev=root;
        solve(root->right,first,middle,last,prev);
    }
    void correctBST(Node* root) {
        Node*first=NULL,*middle=NULL,*last=NULL,*prev=NULL;
        solve(root,first,middle,last,prev);
        if(first && last)
            swap(first->data,last->data);
        else if(first && middle)
            swap(first->data,middle->data);
    }
};
```

### 14. Lowest Common Ancestor in a BST
Given a Binary Search Tree (with all values unique) and two nodes n1 and n2 (n1 != n2). You may assume that both nodes exist in the tree. Find the Lowest Common Ancestor (LCA) of the given two nodes in the BST.

LCA between two nodes n1 and n2 is defined as the lowest node that has both n1 and n2 as descendants (where we allow a node to be a descendant of itself).

```cpp
class Solution {
  public:
    Node* LCA(Node* root, Node* n1, Node* n2) {
        if(root->data>n1->data && root->data>n2->data)
        return LCA(root->left,n1,n2);
        else if(root->data<n1->data && root->data<n2->data)
        return LCA(root->right,n1,n2);
        else
        return root;
    }
};
```

### 15. Serialize and deserialize a binary tree
Serialization is to store a tree in an array so that it can be later restored and deserialization is reading tree back from the array. Complete the functions

serialize() : stores the tree into an array a and returns the array.
deSerialize() : deserializes the array to the tree and returns the root of the tree.
Note: Multiple nodes can have the same data and the node values are always positive integers. Your code will be correct if the tree returned by deSerialize(serialize(input_tree)) is same as the input tree. Driver code will print the in-order traversal of the tree returned by deSerialize(serialize(input_tree)).

```cpp
class Solution {
  public:
    // Function to serialize a tree and return a list containing nodes of tree
    vector<int> serialize(Node *root) {
        vector<int>res;
        queue<Node*>q;
        q.push(root);
        while(!q.empty()){
            Node*node=q.front();
            q.pop();
            res.push_back(node->data);
            if(node->data==-1)
            continue;
            if(node->left)
            q.push(node->left);
            else
            q.push(new Node(-1));
            if(node->right)
            q.push(node->right);
            else
            q.push(new Node(-1));
        }
        return res;
    }

    // Function to deserialize a list and construct the tree.
    Node *deSerialize(vector<int> &arr) {
        if(arr[0]==-1)
        return NULL;
        Node*root=new Node(arr[0]);
        queue<Node*>q;
        q.push(root);
        int i=1;
        while(!q.empty()){
            Node*node=q.front();
            q.pop();
            if(arr[i]!=-1){
                node->left=new Node(arr[i++]);
                q.push(node->left);
            }
            else
            i++;
            if(arr[i]!=-1){
                node->right=new Node(arr[i++]);
                q.push(node->right);
            }
            else
            i++;
        }
        return root;
    }
};
```

### 16. Symmetric Tree
Given the root of a binary tree, check whether it is symmetric, i.e., whether the tree is a mirror image of itself.


A binary tree is symmetric if the left subtree is a mirror reflection of the right subtree.

```cpp
class Solution {
  public:
    bool solve(Node*leftSub,Node*rightSub){
        if(!leftSub && !rightSub)
        return true;
        else if(!leftSub)
        return false;
        else if(!rightSub)
        return false;
        else if(rightSub->data!=leftSub->data)
        return false;
        else
        return solve(leftSub->right,rightSub->left)
        && solve(leftSub->left,rightSub->right);
    }
    bool isSymmetric(Node* root) {
        return solve(root->left,root->right);
    }
};
```

### 17. Balanced Tree Check
Given a binary tree, determine if it is height-balanced. A binary tree is considered height-balanced if the absolute difference in heights of the left and right subtrees is at most 1 for every node in the tree.

```cpp
class Solution {
  public:
    int solve(Node*root){
        if(!root)
        return 0;
        int lHeight=solve(root->left);
        int rHeight=solve(root->right);
        if(lHeight==-1 || rHeight==-1 || abs(lHeight-rHeight)>1)
        return -1;
        else
        return max(lHeight,rHeight)+1;
    }
    bool isBalanced(Node* root) {
        return solve(root)>-1;
    }
};
```

### 18. Vertical Tree Traversal
Given a root of a Binary Tree, find the vertical traversal of it starting from the leftmost level to the rightmost level.
If there are multiple nodes passing through a vertical line, then they should be printed as they appear in level order traversal of the tree.

```cpp
class Solution {
  public:
    vector<vector<int>> verticalOrder(Node *root) {
        map<int,vector<int>>mp;
        int mino=0;
        vector<vector<int>>res;
        queue<pair<Node*,int>>q;
        q.push({root,0});
        while(!q.empty()){
            pair<Node*,int>pi=q.front();
            q.pop();
            mino=min(mino,pi.second);
            mp[pi.second].push_back(pi.first->data);
            if(pi.first->left)
                q.push({pi.first->left,pi.second-1});
            if(pi.first->right)
                q.push({pi.first->right,pi.second+1});
        }
        int i=mino;
        while(mp.find(i)!=mp.end()){
            res.push_back(mp[i]);
            i++;
        }
        return res;
    }
};
```

### 19. Distribute candies in a binary tree
You are given a binary tree with n nodes, where each node contains a certain number of candies, and the total number of candies across all nodes is n. In one move, we can select two adjacent nodes and transfer one candy from one node to the other. The transfer can occur between a parent and child in either direction.

The task is to determine the minimum number of moves required to ensure that every node in the tree has exactly one candy.

Note: The testcases are framed such that it is always possible to achieve a configuration in which every node has exactly one candy, after some moves.

```cpp
class Solution {
  public:
    int solve(Node*root,int&ans){
        if(!root)
        return 0;
        int l=solve(root->left,ans);
        int r=solve(root->right,ans);
        ans+=abs(l)+abs(r);
        return root->data+l+r-1;
    }
    int distributeCandy(Node* root) {
        int ans=0;
        solve(root,ans);
        return ans;
    }
};
```

### 20. Normal BST to Balanced BST
Given a root of a Binary Search Tree, modify and return the given BST such that it is balanced and has minimum possible height. If there is more than one answer, return any of them.

Note: The height of balanced BST returned by you will be compared with the expected height of the balanced tree.

```cpp
class Solution {
  public:
    void inorder(Node*root,vector<int>&nodes){
        if(!root)
        return ;
        inorder(root->left,nodes);
        nodes.push_back(root->data);
        inorder(root->right,nodes);
    }
    Node* balance(vector<int>&nodes,int left,int right){
        if(left>right)
        return NULL;
        int mid=(right+left)/2;
        Node* root=new Node(nodes[mid]);
        root->left=balance(nodes,left,mid-1);
        root->right=balance(nodes,mid+1,right);
        return root;
    }
    Node* balanceBST(Node* root) {
        vector<int>nodes;
        inorder(root,nodes);
        Node* newRoot=balance(nodes,0,nodes.size()-1);
        return newRoot;
    }
};
```
