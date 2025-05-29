### 1. Implement Atoi
Given a string s, the objective is to convert it into integer format without utilizing any built-in functions. Refer the below steps to know about atoi() function.

Cases for atoi() conversion:

Skip any leading whitespaces.
Check for a sign (‘+’ or ‘-‘), default to positive if no sign is present.
Read the integer by ignoring leading zeros until a non-digit character is encountered or end of the string is reached. If no digits are present, return 0.
If the integer is greater than 231 – 1, then return 231 – 1 and if the integer is smaller than -231, then return -231.

```cpp
class Solution {
  public:
    int myAtoi(char *s) {
        bool sign=true,check=false;
        long long num=0;
        for(int i=0;i<strlen(s);i++){
            if(s[i]!='+' && s[i]!='-' && s[i]!=' ' && (s[i]<'0' || s[i]>'9'))
            break;
            if(s[i]=='+'){
                if(check)
                break;
                else
                check=true;
            }
            else if(s[i]=='-')
            {
                if(check)
                break;
                else{
                sign=false;check=true;}
            }else if(s[i]==' '){
                continue;
            }else{
                check=true;
                num=num*10+(s[i]-'0');
            }
        }
        if(sign==false)
        num=-num;
        if(num>pow(2,31)-1)
        return (int)(pow(2,31)-1);
        else if(num<-pow(2,31))
        return (int)(-pow(2,31));
        return (int)num;
    }
};
```
