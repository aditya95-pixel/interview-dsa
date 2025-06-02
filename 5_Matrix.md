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

### 4. Search in a row-wise sorted matrix
Given a row-wise sorted 2D matrix mat[][] of size n x m and an integer x, find whether element x is present in the matrix.
Note: In a row-wise sorted matrix, each row is sorted in itself, i.e. for any i, j within bounds, mat[i][j] <= mat[i][j+1].

```cpp
class Solution {
  public:
    // Function to search a given number in row-column sorted matrix.
    bool searchRowMatrix(vector<vector<int>> &mat, int x) {
        for(int i=0;i<mat.size();i++){
            vector<int>arr=mat[i];
            int l=0,h=arr.size()-1;
            while(l<=h){
                int mid=l+(h-l)/2;
                if(arr[mid]==x)
                return true;
                else if(arr[mid]>x)
                h=mid-1;
                else
                l=mid+1;
            }
        }
        return false;
    }
};
```

### 5. Search in a sorted Matrix
Given a strictly sorted 2D matrix mat[][] of size n x m and a number x. Find whether the number x is present in the matrix or not.
Note: In a strictly sorted matrix, each row is sorted in strictly increasing order, and the first element of the ith row (i!=0) is greater than the last element of the (i-1)th row.

```cpp
class Solution {
  public:
    // Function to search a given number in row-column sorted matrix.
    bool searchMatrix(vector<vector<int>> &mat, int x) {
        int i=0,j=0;
        while(i<mat.size() && j<mat[0].size())
        {
            if(mat[i][j]==x)
                return true;
            else if(i+1<mat.size() && mat[i+1][j]<=x)
                i++;
            else if(j+1<mat[0].size() && mat[i][j+1]<=x)
                j++;
            else
                return false;
        }
        return false;
    }
};
```

### 6. Set Matrix Zeroes
You are given a 2D matrix mat[][] of size n×m. The task is to modify the matrix such that if mat[i][j] is 0, all the elements in the i-th row and j-th column are set to 0 and do it in constant space complexity.

```cpp
class Solution {
  public:
    void setMatrixZeroes(vector<vector<int>> &mat) {
       bool c0=false;
       for(int i=0;i<mat.size();i++){
           for(int j=0;j<mat[0].size();j++){
               if(mat[i][j]==0)
               {
                   mat[i][0]=0;
                   if(j==0)
                   c0=true;
                   else
                   mat[0][j]=0;
               }
           }
       }
       for(int i=1;i<mat.size();i++){
           for(int j=1;j<mat[0].size();j++){
               if(mat[i][0]==0 || mat[0][j]==0)
               mat[i][j]=0;
           }
       }
       if(mat[0][0]==0){
           for(int j=0;j<mat[0].size();j++)
           mat[0][j]=0;
       }
       if(c0){
           for(int i=0;i<mat.size();i++)
           mat[i][0]=0;
       }
    }
};
```

### 7. Rotate a Matrix by 180 Counterclockwise
Given a 2D square matrix mat[][] of size n x n, turn it by 180 degrees without using extra space.

Note: You must rotate the matrix in place and modify the input matrix directly.

```cpp
class Solution {
  public:
    void rotateMatrix(vector<vector<int>>& mat) {
        reverse(mat.begin(),mat.end());
        for(int i=0;i<mat.size();i++)
        reverse(mat[i].begin(),mat[i].end());
    }
};
```

### 8. Create a spiral matrix of N*M size from given array
You are given two positive integers n and m, and an integer array arr[] containing total (n*m) elements. Return a 2D matrix of dimensions n x m by filling it in a clockwise spiral order using the elements from the given array.

```cpp
class Solution {
  public:
    vector<vector<int>> spiralFill(int n, int m, vector<int> &arr) {
        vector<vector<int>>mat(n,vector<int>(m,0));
        int rowup=0,rowdown=n-1,colleft=0,colright=m-1;
        int c=0;
        while(c<arr.size()){
            for(int i=colleft;i<=colright;i++)
            mat[rowup][i]=arr[c++];
            if(c==arr.size())
            break;
            rowup++;
            for(int i=rowup;i<=rowdown;i++)
            mat[i][colright]=arr[c++];
            if(c==arr.size())
            break;
            colright--;
            for(int i=colright;i>=colleft;i--)
            mat[rowdown][i]=arr[c++];
            if(c==arr.size())
            break;
            rowdown--;
            for(int i=rowdown;i>=rowup;i--)
            mat[i][colleft]=arr[c++];
            colleft++;
        }
        return mat;
    }
};
```
