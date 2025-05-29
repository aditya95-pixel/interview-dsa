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

### 2. Add Binary Strings
Given two binary strings s1 and s2 consisting of only 0s and 1s. Find the resultant string after adding the two Binary Strings.
Note: The input strings may contain leading zeros but the output string should not have any leading zeros.

```cpp
// User function template for C++
class Solution {
  public:
    string addBinary(string& s1, string& s2) {
        int carry=0;
        string res;
        int i=0,j=0;
        reverse(s1.begin(),s1.end());
        reverse(s2.begin(),s2.end());
        while(i<s1.size() && j<s2.size()){
            if(s1[i]=='0' && s2[j]=='0'){
                if(carry==0)
                res+='0';
                else
                {
                    res+='1';
                    carry=0;
                }
            }
            else if(s1[i]=='0' && s2[j]=='1'){
                if(carry==0)
                res+='1';
                else
                res+='0';
            }
            else if(s1[i]=='1' && s2[j]=='0'){
                if(carry==0)
                res+='1';
                else
                res+='0';
            }
            else{
                if(carry==0){
                    carry=1;
                    res+='0';
                }
                else
                    res+='1';
            }
            i++;
            j++;
        }
        while(i<s1.size()){
            if(s1[i]=='0'){
                if(carry==0){
                    res+='0';
                }
                else{
                    res+='1';
                    carry=0;
                }
            }
            else{
                if(carry==0){
                    res+='1';
                }
                else{
                    res+='0';
                    carry=1;
                }
            }
            i++;
        }
        while(j<s2.size()){
            if(s2[j]=='0'){
                if(carry==0){
                    res+='0';
                }
                else{
                    res+='1';
                    carry=0;
                }
            }
             else{
                if(carry==0){
                    res+='1';
                }
                else{
                    res+='0';
                    carry=1;
                }
            }
            j++;
        }
        if(carry==1)
        res+='1';
        reverse(res.begin(),res.end());
        string res1=res.substr(res.find('1'));
        return res1;
    }
};
```

### 3. Anagram
Given two strings s1 and s2 consisting of lowercase characters. The task is to check whether two given strings are an anagram of each other or not. An anagram of a string is another string that contains the same characters, only the order of characters can be different. For example, "act" and "tac" are an anagram of each other. Strings s1 and s2 can only contain lowercase alphabets.

Note: You can assume both the strings s1 & s2 are non-empty.

```cpp
class Solution {
  public:
    // Function is to check whether two strings are anagram of each other or not.
    bool areAnagrams(string& s1, string& s2) {
        map<char,int>mp1,mp2;
        for(auto i:s1)
        mp1[i]++;
        for(auto i:s2)
        mp2[i]++;
        return mp1==mp2;
    }
};
```

### 4. Non Repeating Character
Given a string s consisting of lowercase English Letters. Return the first non-repeating character in s.
If there is no non-repeating character, return '$'.
Note: When you return '$' driver code will output -1.

```cpp
class Solution {
  public:
    char nonRepeatingChar(string &s) {
        map<char,int>mp;
        for(auto i:s)
        mp[i]++;
        for(auto i:s){
            if(mp[i]==1)
            return i;
        }
        return '$';
    }
};
```

### 5. Search Pattern (KMP-Algorithm)
Given two strings, one is a text string txt and the other is a pattern string pat. The task is to print the indexes of all the occurrences of the pattern string in the text string. Use 0-based indexing while returning the indices. 
Note: Return an empty list in case of no occurrences of pattern.

```cpp
class Solution {
  public:
    vector<int> prefix_function(string& s){
        vector<int>pi(s.size(),0);
        for(int i=1;i<s.size();i++){
            int j=pi[i-1];
            while(j>0 && s[i]!=s[j])
            j=pi[j-1];
            if(s[i]==s[j])
            j++;
            pi[i]=j;
        }
        return pi;
    }
    vector<int> search(string& pat, string& txt) {
        vector<int>res;
        vector<int>pi=prefix_function(pat);
        int i=0,j=0;
        while(i<txt.size()){
            if(pat[j]==txt[i]){
                i++;
                j++;
            }else{
                if(j>0){
                    j=pi[j-1];
                }else{
                    i++;
                }
            } 
            if(j==pat.size()){
                res.push_back(i-j);
                j=pi[j-1];
            }
        }
        return res;
    }
};
```
