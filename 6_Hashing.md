### 1. Two Sum - Pair with Given Sum
Given an array arr[] of positive integers and another integer target. Determine if there exist two distinct indices such that the sum of their elements is equal to the target.

```cpp
class Solution {
  public:
    bool twoSum(vector<int>& arr, int target) {
        map<int,int>mp;
        for(auto x:arr){
            mp[x]++;
            if(mp.find(target-x)!=mp.end() && x!=target-x)
            return true;
            else if(mp.find(target-x)!=mp.end() && x==target-x && mp[x]>1)
            return true;
        }
        return false;
    }
};
```
