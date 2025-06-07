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

### 5. Word Search
You are given a two-dimensional mat[][] of size n*m containing English alphabets and a string word. Check if the word exists on the mat. The word can be constructed by using letters from adjacent cells, either horizontally or vertically. The same cell cannot be used more than once.

```cpp
class Solution {
  public:
    bool solve(vector<vector<char>>& mat, string& word,string &curr,int row
    ,int col){
        if(curr.size()==word.size()){
            if(curr==word)
            return true;
            else
            return false;
        }else{
            if(row+1<mat.size() && mat[row+1][col]==word[curr.size()])
            {
                curr+=word[curr.size()];
                mat[row+1][col]='.';
                if(solve(mat,word,curr,row+1,col))
                return true;
                curr.pop_back();
                mat[row+1][col]=word[curr.size()];
            }
            if(row-1>=0 && mat[row-1][col]==word[curr.size()])
            {
                curr+=word[curr.size()];
                mat[row-1][col]='.';
                if(solve(mat,word,curr,row-1,col))
                return true;
                curr.pop_back();
                mat[row-1][col]=word[curr.size()];
            }
            if(col+1<mat[0].size() && mat[row][col+1]==word[curr.size()])
            {
                curr+=word[curr.size()];
                mat[row][col+1]='.';
                if(solve(mat,word,curr,row,col+1))
                return true;
                curr.pop_back();
                mat[row][col+1]=word[curr.size()];
            }
            if(col-1>=0 && mat[row][col-1]==word[curr.size()])
            {
                curr+=word[curr.size()];
                mat[row][col-1]='.';
                if(solve(mat,word,curr,row,col-1))
                return true;
                curr.pop_back();
                mat[row][col-1]=word[curr.size()];
            }
            return false;
        }
        
    }
    bool isWordExist(vector<vector<char>>& mat, string& word) {
        for(int i=0;i<mat.size();i++){
            for(int j=0;j<mat[0].size();j++){
                if(mat[i][j]==word[0])
                {
                    string curr;
                    curr+=word[0];
                    mat[i][j]='.';
                    if(solve(mat,word,curr,i,j))
                    return true;
                    mat[i][j]=word[0];
                    curr.pop_back();
                }
            }
        }
        return false;
    }
};
```

### 6. Possible Words From Phone Digits
Given a keypad as shown in the diagram, and an array arr[ ], your task is to list all possible words in any order which can be generated by pressing numbers from array.

```cpp
class Solution {
  public:
    void solve(vector<int>&arr,string &curr,vector<string>&res,
    map<int,vector<char>>&mp){
        if(curr.size()==arr.size())
        {
            res.push_back(curr);
            return ;
        }
        for(auto c:mp[arr[curr.size()]]){
            curr.push_back(c);
            solve(arr,curr,res,mp);
            curr.pop_back();
        }
    }
    vector<string> possibleWords(vector<int> &arr) {
        vector<string>res;
        map<int,vector<char>>mp={{2,{'a','b','c'}},{3,{'d','e','f'}},
        {4,{'g','h','i'}},{5,{'j','k','l'}},{6,{'m','n','o'}},
        {7,{'p','q','r','s'}},{8,{'t','u','v'}},{9,{'w','x','y','z'}}};
        string curr;
        solve(arr,curr,res,mp);
        return res;
    }
};
```

### 7. Generate IP Addresses
Given a string s containing only digits, your task is to restore it by returning all possible valid IP address combinations. You can return your answer in any order.

A valid IP address must be in the form of A.B.C.D, where A, B, C, and D are numbers from 0-255(inclusive).

Note: The numbers cannot be 0 prefixed unless they are 0. For example, 1.1.2.11 and 0.11.21.1 are valid IP addresses while 01.1.2.11 and 00.11.21.1 are not.

```cpp
class Solution {
    public:
    bool check(string &temp){
        if(temp.size()==0)
        return false;
        if(temp.size()>3)
        return false;
        if(temp.size()==1)
        return true;
        int val=stoi(temp);
        if(temp[0]=='0' || val>255)
        return false;
        return true;
    }
    void generate(string&s,int idx,string curr,int cnt,vector<string>&res){
        string temp;
        if(cnt==3){
            temp=s.substr(idx);
            if(check(temp))
            {
                curr+=temp;
                res.push_back(curr);
            }
            return ;
        }
        for(int i=idx;i<min(idx+3,(int)s.size());i++){
            temp+=s[i];
            if(check(temp))
            generate(s,i+1,curr+temp+'.',cnt+1,res);
        }
    }
    vector<string> generateIp(string s) {
        vector<string>res;
        string curr;
        generate(s,0,curr,0,res);
        return res;
    }
};
```

### 8. Combination Sum
Given an array arr[] and a target, your task is to find all unique combinations in the array where the sum is equal to target. The same number may be chosen from the array any number of times to make target.

You can return your answer in any order.

```cpp
class Solution {
  public:
    // Function to find all combinations of elements
    // in array arr that sum to target.
    void solve(vector<int>&arr,int target,vector<int>&temp,int sum,
    vector<vector<int>>&res,int idx){
        if(sum==target)
        {
            res.push_back(temp);
            return ;
        }
        if(sum>target)
            return ;
        else{
            for(int i=idx;i<arr.size();i++){
                temp.push_back(arr[i]);
                solve(arr,target,temp,sum+arr[i],res,i);
                temp.pop_back();
            }
            return ;
        }
    }
    vector<vector<int>> combinationSum(vector<int> &arr, int target) {
        vector<vector<int>>res;
        vector<int>temp;
        solve(arr,target,temp,0,res,0);
        return res;
    }
};
```

### 9. Combination Sum II
Given an array arr[] and a target, your task is to find all unique combinations in the array where the sum is equal to target. Each number in arr[] may only be used once in the combination.

You can return your answer in any order.

```cpp
class Solution {
  public:
    // Function to find all combinations of elements
    // in array arr that sum to target
    void solve(vector<int>&arr,int target,vector<int>&temp,int sum,
    vector<vector<int>>&res,int idx){
        if(sum==target){
            res.push_back(temp);
            return ;
        }
        if(sum>target)
        return ;
        for(int i=idx;i<arr.size();i++){
            if(i>idx && arr[i]==arr[i-1])
            continue;
            temp.push_back(arr[i]);
            solve(arr,target,temp,sum+arr[i],res,i+1);
            temp.pop_back();
        }
    }
    vector<vector<int>> uniqueCombinations(vector<int> &arr, int target) {
        sort(arr.begin(),arr.end());
        vector<vector<int>>res;
        vector<int>temp;
        solve(arr,target,temp,0,res,0);
        return res;
    }
};
```
