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

### 3. Find All Triplets with Zero Sum
Given an array arr[], find all possible triplets i, j, k in the arr[] whose sum of elements is equals to zero. 
Returned triplet should also be internally sorted i.e. i<j<k.

```cpp
class Solution {
  public:
    vector<vector<int>> findTriplets(vector<int> &arr) {
        vector<vector<int>>res;
        map<int,vector<int>>mp;
        for(int j=0;j<arr.size();j++){
            for(int k=j+1;k<arr.size();k++){
                int val=-1*(arr[j]+arr[k]);
                if(mp.find(val)!=mp.end())
                {
                    for(auto i:mp[val])
                    res.push_back({i,j,k});
                }
            }
            mp[arr[j]].push_back(j);
        }
        return res;
    }
};
```

### 4. Intersection of Two arrays with Duplicate Elements
Given two integer arrays a[] and b[], you have to find the intersection of the two arrays. Intersection of two arrays is said to be elements that are common in both arrays. The intersection should not have duplicate elements and the result should contain items in any order.

Note: The driver code will sort the resulting array in increasing order before printing.

```cpp
class Solution {
  public:
    vector<int> intersectionWithDuplicates(vector<int>& a, vector<int>& b) {
        vector<int>res;
        map<int,int>mp;
        for(auto x:a)
        {
            if(mp.find(x)==mp.end())
            mp[x]++;
        }
        for(auto x:b)
        {
            if(mp.find(x)!=mp.end() && mp[x]>0)
            mp[x]--;
        }
        for(auto item:mp)
        {
            if(item.second==0)
            res.push_back(item.first);
        }
        return res;
    }
};
```
