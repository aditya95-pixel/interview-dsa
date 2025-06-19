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
