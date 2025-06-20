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
