### 1. Parenthesis Checker
Given a string s, composed of different combinations of '(' , ')', '{', '}', '[', ']', verify the validity of the arrangement.
An input string is valid if:

         1. Open brackets must be closed by the same type of brackets.
         2. Open brackets must be closed in the correct order.

```cpp
class Solution {
  public:
    bool isBalanced(string& k) {
        stack<char>stk;
        for(auto c:k){
            if(c=='(' || c=='{' || c=='[')
            stk.push(c);
            else if((c==')' || c=='}' || c==']') && stk.empty())
            return false;
            else if(c==')' && stk.top()!='(')
            return false;
            else if(c==')' && stk.top()=='(')
            stk.pop();
            else if(c=='}' && stk.top()!='{')
            return false;
            else if(c=='}' && stk.top()=='{')
            stk.pop();
            else if(c==']' && stk.top()!='[')
            return false;
            else if(c==']' && stk.top()=='[')
            stk.pop();
        }
        return stk.empty();
    }
};
```

### 2. Longest valid Parentheses
Given a string s consisting of opening and closing parenthesis '(' and ')'. Find the length of the longest valid parenthesis substring.

A parenthesis string is valid if:

For every opening parenthesis, there is a closing parenthesis.
The closing parenthesis must be after its opening parenthesis.

```cpp
class Solution {
  public:
    int maxLength(string& s) {
        stack<int>stk;
        stk.push(-1);
        int maxlen=0;
        for(int i=0;i<s.size();i++){
            if(s[i]=='(')
            stk.push(i);
            else{
                stk.pop();
                if(stk.empty())
                stk.push(i);
                else
                maxlen=max(maxlen,i-stk.top());
            }
        }
        return maxlen;
    }
};
```

### 3. Next Greater Element
Given an array arr[ ] of integers, the task is to find the next greater element for each element of the array in order of their appearance in the array. Next greater element of an element in the array is the nearest element on the right which is greater than the current element.
If there does not exist next greater of current element, then next greater element for current element is -1. For example, next greater of the last element is always -1.

```cpp
class Solution {
  public:
    vector<int> nextLargerElement(vector<int>& arr) {
        vector<int>res(arr.size(),-1);
        stack<int>stk;
        for(int i=arr.size()-1;i>=0;i--){
            while(!stk.empty() && stk.top()<=arr[i])
                stk.pop();
            if(!stk.empty())
                res[i]=stk.top();
            stk.push(arr[i]);
        }
        return res;
    }
};
```

### 4. Stock span problem
The stock span problem is a financial problem where we have a series of daily price quotes for a stock and we need to calculate the span of stock price for all days. The span arr[i] of the stocks price on a given day i is defined as the maximum number of consecutive days just before the given day, for which the price of the stock on the given day is less than or equal to its price on the current day.

```cpp
class Solution {
  public:
    vector<int> calculateSpan(vector<int>& arr) {
        vector<int>res;
        stack<int>stk;
        for(int i=0;i<arr.size();i++){
            while(!stk.empty() && arr[stk.top()]<=arr[i])
            stk.pop();
            if(stk.empty())
            res.push_back(i+1);
            else
            res.push_back(i-stk.top());
            stk.push(i);
        }
        return res;
    }
};
```

### 5. Histogram Max Rectangular Area
You are given a histogram represented by an array arr, where each element of the array denotes the height of the bars in the histogram. All bars have the same width of 1 unit.

Your task is to find the largest rectangular area possible in the given histogram, where the rectangle can be formed using a number of contiguous bars.

```cpp
class Solution {
  public:
    int getMaxArea(vector<int> &arr) {
        int res=0;
        stack<int>stk;
        for(int i=0;i<arr.size();i++){
            while(!stk.empty() && arr[stk.top()]>=arr[i]){
                int top=stk.top();
                stk.pop();
                int width=stk.empty()?i:i-stk.top()-1;
                res=max(res,arr[top]*width);
            }
            stk.push(i);
        }
        while(!stk.empty()){
            int top=stk.top();
            stk.pop();
            int width=stk.empty()?arr.size():arr.size()-stk.top()-1;
            res=max(res,arr[top]*width);
        }
        return res;
    }
};
```

### 6. Max of min for every window size
Given an array of integers arr[], the task is to find the maximum of the minimum values for every possible window size in the array, where the window size ranges from 1 to arr.size().

More formally, for each window size k, determine the smallest element in all windows of size k, and then find the largest value among these minimums where 1<=k<=arr.size().

```cpp
class Solution {
  public:
    vector<int> maxOfMins(vector<int>& arr) {
        vector<int>res(arr.size(),0);
        stack<int>stk;
        vector<int>len(arr.size(),0);
        for(int i=0;i<arr.size();i++){
            while(!stk.empty() && arr[stk.top()]>=arr[i]){
                int top=stk.top();
                stk.pop();
                int window=stk.empty()?i:i-stk.top()-1;
                len[top]=window;
            }
            stk.push(i);
        }
        while(!stk.empty()){
            int top=stk.top();
            stk.pop();
            int window=stk.empty()?arr.size():arr.size()-stk.top()-1;
            len[top]=window;
        }
        for(int i=0;i<arr.size();i++){
            int window=len[i]-1;
            res[window]=max(res[window],arr[i]);
        }
        for(int i=arr.size()-2;i>=0;i--)
            res[i]=max(res[i],res[i+1]);
        return res;
    }
};
```

### 7. Get Min from Stack
Given q queries, You task is to implement the following four functions for a stack:

push(x) – Insert an integer x onto the stack.
pop() – Remove the top element from the stack.
peek() - Return the top element from the stack. If the stack is empty, return -1.
getMin() – Retrieve the minimum element from the stack in O(1) time. If the stack is empty, return -1.
Each query can be one of the following:

1 x : Push x onto the stack.
2 : Pop the top element from the stack.
3: Return the top element from the stack. If the stack is empty, return -1.
4: Return the minimum element from the stack.

```cpp
class Solution {
    stack<pair<int,int>>stk;
  public:
    Solution() {
        
    }

    // Add an element to the top of Stack
    void push(int x) {
        if(stk.empty() || x<stk.top().second)
            stk.push({x,x});
        else
        {
            int mino=stk.top().second;
            stk.push({x,mino});
        }
    }

    // Remove the top element from the Stack
        
    void pop() {
        if(stk.empty())
        return ;
        else
        stk.pop();
    }

        
    // Returns top element of the Stack
    int peek() {
        if(stk.empty())
        return -1;
        else
        return stk.top().first;
    }
        

    // Finds minimum element of Stack
    int getMin() {
         if(stk.empty())
        return -1;
        else
        return stk.top().second;
    }
};
```

### 8. Postfix Evaluation
You are given an array of strings arr that represents a valid arithmetic expression written in Reverse Polish Notation (Postfix Notation). Your task is to evaluate the expression and return an integer representing its value.

Key Details:

The valid operators are '+', '-', '*', and '/'.
Each operand is guaranteed to be a valid integer or another expression.
The division operation between two integers always rounds the result towards zero, discarding any fractional part.
No division by zero will occur in the input.
The input is a valid arithmetic expression in Reverse Polish Notation.
The result of the expression and all intermediate calculations will fit in a 32-bit signed integer.

```cpp
class Solution {
  public:
    int evaluate(vector<string>& arr) {
        stack<int>stk;
        for(int i=0;i<arr.size();i++){
            if(arr[i]!="+" && arr[i]!="-" && arr[i]!="*" && arr[i]!="/")
                stk.push(stoi(arr[i]));
            else if(arr[i]=="+"){
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1+num2);
            }
            else if(arr[i]=="-"){
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1-num2);
            }
            else if(arr[i]=="*"){
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1*num2);
            }
            else{
                int num2=stk.top();
                stk.pop();
                int num1=stk.top();
                stk.pop();
                stk.push(num1/num2);
            }
        }
        return stk.top();
    }
};
```

### 9. Decode the string
Given an encoded string s, the task is to decode it. The encoding rule is :

k[encodedString], where the encodedString inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer, and encodedString contains only lowercase english alphabets.
Note: The test cases are generated so that the length of the output string will never exceed 10^5 .

```cpp
class Solution {
  public:
    string decodedString(string &s) {
        stack<string>stk;
        for(int i=0;i<s.size();i++){
            if(s[i]!=']'){
                string temp;
                temp+=s[i];
                stk.push(temp);
            }else{
                string temp;
                stack<string>stk_temp;
                while(stk.top()!="["){
                    stk_temp.push(stk.top());
                    stk.pop();
                }
                while(!stk_temp.empty()){
                    temp+=stk_temp.top();
                    stk_temp.pop();
                }
                stk.pop();
                string c;
                while(!stk.empty() && stk.top()>="0" && stk.top()<="9"){
                    c+=stk.top();
                    stk.pop();
                }
                reverse(c.begin(),c.end());
                int cnt=stoi(c);
                string str;
                while(cnt--)
                str+=temp;
                stk.push(str);
            }
        }
        string res;
        stack<string>stk_temp;
        while(!stk.empty()){
            stk_temp.push(stk.top());
            stk.pop();
        }
        while(!stk_temp.empty()){
            res+=stk_temp.top();
            stk_temp.pop();
        }
        return res;
    }
};
```

### 10. Get Max from Stack
Given q queries, You task is to implement the following three functions for a stack:

push(x) – Insert an integer x onto the stack.
pop() – Remove the top element from the stack.
peek() - Return the top element from the stack. If the stack is empty, return -1.
getMax() – Retrieve the maximum element from the stack in O(1) time. If the stack is empty, return -1.
Each query can be one of the following:

1 x : Push x onto the stack.
2 : Pop the top element from the stack.
3: Return the top element from the stack. If the stack is empty, return -1.
4: Return the maximum element from the stack.

```cpp
class Solution {
    stack<pair<int,int>>stk;
  public:
    Solution() {
        
    }

    // Add an element to the top of Stack
    void push(int x) {
        if(stk.empty() || x>stk.top().second)
        stk.push({x,x});
        else
        {
            int maxo=stk.top().second;
            stk.push({x,maxo});
        }
    }

    // Remove the top element from the Stack
    void pop() {
        if(stk.empty())
        return ;
        else
        stk.pop();
    }

        
    // Returns top element of the Stack
    int peek() {
        if(stk.empty())
        return -1;
        else
        return stk.top().first;
    }
        

    // Finds maximum element of Stack
    int getMax() {
        if(stk.empty())
        return -1;
        else
        return stk.top().second;
    }
};
```

### 11. The Celebrity Problem
A celebrity is a person who is known to all but does not know anyone at a party. A party is being organized by some people. A square matrix mat[][] (n*n) is used to represent people at the party such that if an element of row i and column j is set to 1 it means ith person knows jth person. You need to return the index of the celebrity in the party, if the celebrity does not exist, return -1.

Note: Follow 0-based indexing.

```cpp
class Solution {
  public:
    int celebrity(vector<vector<int> >& mat) {
        for(int i=0;i<mat.size();i++){
            bool flag=true;
            for(int j=0;j<mat.size();j++){
                if(i!=j && (mat[i][j] || !mat[j][i]))
                    flag=false;
            }
            if(flag)
            return i;
        }
        return -1;
    }
};
```
