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

### 2. Count pairs with given sum
Given an array arr[] and an integer target. You have to find numbers of pairs in array arr[] which sums up to given target.

```cpp
class Solution {
  public:
    int countPairs(vector<int> &arr, int target) {
        int cnt=0;
        map<int,int>mp;
        for(auto x:arr)
        mp[x]++;
        for(auto item:mp){
            if(mp.find(target-item.first)!=mp.end() &&
            item.first!=target-item.first)
            cnt+=item.second*mp[target-item.first];
            else if(mp.find(target-item.first)!=mp.end() &&
            item.first==target-item.first)
            cnt+=item.second*(item.second-1);
        }
        return cnt/2;
    }
};
```
