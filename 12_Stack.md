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
