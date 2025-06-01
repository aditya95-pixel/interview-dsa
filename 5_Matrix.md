### 1. Spirally traversing a matrix
You are given a rectangular matrix mat[][] of size n x m, and your task is to return an array while traversing the matrix in spiral form.

```cpp
class Solution {
  public:
    vector<int> spirallyTraverse(vector<vector<int> > &mat) {
        vector<int>res;
        int c=0;
        int rowup=0,rowdown=mat.size()-1,colleft=0,colright=mat[0].size()-1;
        while(c<mat.size()*mat[0].size()){
            for(int i=colleft;i<=colright;i++)
            {res.push_back(mat[rowup][i]);c++;}
            if(c==mat.size()*mat[0].size())
            break;
            rowup++;
            for(int i=rowup;i<=rowdown;i++)
            {res.push_back(mat[i][colright]);c++;}
            if(c==mat.size()*mat[0].size())
            break;
            colright--;
            for(int i=colright;i>=colleft;i--)
            {res.push_back(mat[rowdown][i]);c++;}
            if(c==mat.size()*mat[0].size())
            break;
            rowdown--;
            for(int i=rowdown;i>=rowup;i--)
            {res.push_back(mat[i][colleft]);c++;}
            if(c==mat.size()*mat[0].size())
            break;
            colleft++;
        }
        return res;
    }
};
```

### 2. Rotate by 90 degree
Given a square matrix mat[][] of size n x n. The task is to rotate it by 90 degrees in an anti-clockwise direction without using any extra space.

```cpp
class Solution {
  public:
    // Function to rotate matrix anticlockwise by 90 degrees.
    void rotateby90(vector<vector<int>>& mat) {
        for(int i=0;i<mat.size();i++){
            for(int j=0;j<i;j++)
            swap(mat[i][j],mat[j][i]);
        }
        reverse(mat.begin(),mat.end());
    }
};
```

### 3. Search in a Row-Column sorted matrix
Given a 2D integer matrix mat[][] of size n x m, where every row and column is sorted in increasing order and a number x, the task is to find whether element x is present in the matrix.

```cpp
class Solution {
  public:
    bool matSearch(vector<vector<int>> &mat, int x) {
        int i=0,j=mat[0].size()-1;
        while(i<mat.size() && j>=0){
            if(mat[i][j]==x)
            return true;
            else if(mat[i][j]<x)
            i++;
            else
            j--;
        }
        return false;
    }
};
```
