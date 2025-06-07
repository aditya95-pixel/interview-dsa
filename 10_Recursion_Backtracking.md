### 1. Permutations of a String
Given a string s, which may contain duplicate characters, your task is to generate and return an array of all unique permutations of the string. You can return your answer in any order.

```cpp
class Solution {
  public:
    void generate(int size,map<char,int>&mp,string curr,vector<string>&res){
        if(curr.size()==size)
        {
            res.push_back(curr);
            return ;
        }
        for(auto item:mp){
            int cnt=item.second;
            char c=item.first;
            if(cnt>0){
                curr.push_back(c);
                mp[c]--;
                generate(size,mp,curr,res);
                mp[c]++;
                curr.pop_back();
            }
        }
    }
    vector<string> findPermutation(string &s) {
        vector<string>res;
        map<char,int>mp;
        for(auto c:s)
        mp[c]++;
        string curr;
        generate(s.size(),mp,curr,res);
        return res;
    }
};
```

### 2. Implement Pow
Implement the function power(b, e), which calculates b raised to the power of e (i.e. b^e).

```cpp
class Solution {
  public:
    double power(double b, int e) {
        if(e<0)
        return 1/power(b,-e);
        else if(e==0)
        return 1;
        else if(e%2==0)
        return power(b*b,e/2);
        else
        return b*power(b*b,e/2);
    }
};
```

### 3. N-Queen Problem
The n-queens puzzle is the problem of placing n queens on a (n × n) chessboard such that no two queens can attack each other. Note that two queens attack each other if they are placed on the same row, the same column, or the same diagonal.

Given an integer n, find all distinct solutions to the n-queens puzzle.
You can return your answer in any order but each solution should represent a distinct board configuration of the queen placements, where the solutions are represented as permutations of [1, 2, 3, ..., n]. In this representation, the number in the ith position denotes the row in which the queen is placed in the ith column.
For eg. below figure represents a chessboard [3 1 4 2].

```cpp
class Solution {
  public:
    bool check(vector<vector<int>>&board,int row,int col){
        for(int i=0;i<row;i++){
            if(board[i][col]==1)
            return false;
        }
        int col1=col-1,row1=row-1;
        while(col1>=0 && row1>=0){
            if(board[row1][col1]==1)
            return false;
            row1--;
            col1--;
        }
        col1=col+1,row1=row-1;
        while(col1<board.size() && row1>=0){
            if(board[row1][col1]==1)
            return false;
            row1--;
            col1++;
        }
        return true;
    } 
    void solve(vector<vector<int>>&res,vector<vector<int>>&board,int row){
        if(row==board.size())
        {
            vector<int>temp;
            for(int j=0;j<board.size();j++){
                for(int i=0;i<board.size();i++){
                    if(board[i][j]==1)
                    {
                        temp.push_back(i+1);
                        break;
                    }
                }
            }
            res.push_back(temp);
            return ;
        }
        for(int j=0;j<board.size();j++){
            board[row][j]=1;
            if(check(board,row,j))
            solve(res,board,row+1);
            board[row][j]=0;
        }
    }
    vector<vector<int>> nQueen(int n) {
        vector<vector<int>>board(n,vector<int>(n,0));
        vector<vector<int>>res;
        solve(res,board,0);
        return res;
    }
};
```

### 4. Solve the Sudoku
Given an incomplete Sudoku configuration in terms of a 9x9  2-D interger square matrix, mat[][], the task is to solve the Sudoku. It is guaranteed that the input Sudoku will have exactly one solution.

A sudoku solution must satisfy all of the following rules:

Each of the digits 1-9 must occur exactly once in each row.
Each of the digits 1-9 must occur exactly once in each column.
Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
Note: Zeros represent blanks to be filled with numbers 1-9, while non-zero cells are fixed and cannot be changed.

```cpp
class Solution {
  public:
    // Function to find a solved Sudoku.
    bool check(vector<vector<int>>&mat,int row,int col){
        for(int i=0;i<mat.size();i++)
        {
            if(i!=row && mat[i][col]==mat[row][col])
            return false;
        }
        for(int i=0;i<mat.size();i++)
        {
            if(i!=col && mat[row][i]==mat[row][col])
            return false;
        }
        if(row<3 && col<3){
            for(int i=0;i<3;i++){
                for(int j=0;j<3;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }else if(row<3 && col<6){
            for(int i=0;i<3;i++){
                for(int j=3;j<6;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }else if(row<3 && col<9){
            for(int i=0;i<3;i++){
                for(int j=6;j<9;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }else if(row<6 && col<3){
            for(int i=3;i<6;i++){
                for(int j=0;j<3;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }else if(row<6 && col<6){
            for(int i=3;i<6;i++){
                for(int j=3;j<6;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }else if(row<6 && col<9){
            for(int i=3;i<6;i++){
                for(int j=6;j<9;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }else if(row<9 && col<3){
            for(int i=6;i<9;i++){
                for(int j=0;j<3;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }else if(row<9 && col<6){
            for(int i=6;i<9;i++){
                for(int j=3;j<6;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }else if(row<9 && col<9){
            for(int i=6;i<9;i++){
                for(int j=6;j<9;j++){
                    if((i!=row || j!=col) && mat[i][j]==mat[row][col])
                    return false;
                }
            }
        }
        return true;
    }
    bool solve(vector<vector<int>>&mat,int row,int col){
        if(row==mat.size()-1 && col==mat.size())
        return true;
        if(col==mat.size())
        return solve(mat,row+1,0);
        if(mat[row][col]!=0)
        return solve(mat,row,col+1);
        for(int i=1;i<=9;i++)
        {
            mat[row][col]=i;
            if(check(mat,row,col))
            {
                if(solve(mat,row,col+1))
                return true;
            }
            mat[row][col]=0;
        }
        return false;
    }
    void solveSudoku(vector<vector<int>> &mat) {
        solve(mat,0,0);
    }
};
```
