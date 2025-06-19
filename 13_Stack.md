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
