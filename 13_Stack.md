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
