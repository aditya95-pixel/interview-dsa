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
    bool check(vector<vector<int>>board,int row,int col){
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
