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

### 6. Min Chars to Add for Palindrome
Given a string s, the task is to find the minimum characters to be added at the front to make the string palindrome.

Note: A palindrome string is a sequence of characters that reads the same forward and backward.

```cpp
class Solution {
  public:
    vector<int>prefix_function(string s){
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
    int minChar(string& s) {
        int n=s.size();
        string rev=s;
        reverse(rev.begin(),rev.end());
        s=s+'$'+rev;
        vector<int>pi=prefix_function(s);
        return n-pi.back();
    }
};
```

### 7. Strings Rotations of Each Other
You are given two strings of equal lengths, s1 and s2. The task is to check if s2 is a rotated version of the string s1.

Note: The characters in the strings are in lowercase.

```cpp

class Solution {
  public:
    // Function to check if two strings are rotations of each other or not.
    vector<int>prefix_function(string s){
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
    bool areRotations(string &s1, string &s2) {
        string txt=s1+s1;
        string pat=s2;
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
            if(j==pat.size())
            return true;
        }
        return false;
    }
};
```

### 8. Fizz Buzz
Fizz Buzz Problem involves that given an integer n, for every integer 0 < i <= n, the task is to output,

"FizzBuzz" if i is divisible by 3 and 5,
"Fizz" if i is divisible by 3,
"Buzz" if i is divisible by 5
"i" as a string, if none of the conditions are true.
Return an array of strings.

```cpp
class Solution {
  public:
    vector<string> fizzBuzz(int n) {
        vector<string>res;
        for(int i=1;i<=n;i++){
            if(i%3==0 && i%5==0)
            res.push_back("FizzBuzz");
            else if(i%3==0)
            res.push_back("Fizz");
            else if(i%5==0)
            res.push_back("Buzz");
            else
            res.push_back(to_string(i));
        }
        return res;
    }
};
```

### 9. CamelCase Pattern Matching
Given a dictionary of words arr[] where each word follows CamelCase notation, print all words in the dictionary that match with a given pattern pat consisting of uppercase characters only.

CamelCase is the practice of writing compound words or phrases such that each word or abbreviation begins with a capital letter. Common examples include PowerPoint and Wikipedia, GeeksForGeeks, CodeBlocks, etc.

Example: "GeeksForGeeks" matches the pattern "GFG", because if we combine all the capital letters in "GeeksForGeeks" they become "GFG". Also note "GeeksForGeeks" matches with the pattern "GFG" and also "GF", but does not match with "FG".

Note: The driver code will sort your answer before checking and return the answer in any order.

```cpp
class Solution {
  public:
    vector<string> camelCase(vector<string> &arr, string &pat) {
        vector<string>res;
        for(auto s:arr){
            string temp;
            int i=0,j=0;
            while(i<s.size() && j<pat.size()){
                if(s[i]>='A' && s[i]<='Z' && s[i]==pat[j])
                j++;
                else if(s[i]>='A' && s[i]<='Z' && s[i]!=pat[j])
                break;
                i++;
            }
            if(j==pat.size())
            res.push_back(s);
        }
        return res;
    }
};
```
