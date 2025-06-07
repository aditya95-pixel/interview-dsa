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
