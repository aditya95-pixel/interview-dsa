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
