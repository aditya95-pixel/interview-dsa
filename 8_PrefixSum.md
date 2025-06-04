### 1. Equilibrium Point
Given an array of integers arr[], the task is to find the first equilibrium point in the array.

The equilibrium point in an array is an index (0-based indexing) such that the sum of all elements before that index is the same as the sum of elements after it. Return -1 if no such point exists. 

```cpp
class Solution {
  public:
    // Function to find equilibrium point in the array.
    int findEquilibrium(vector<int> &arr) {
        int sum=accumulate(arr.begin(),arr.end(),0);
        int prefSum=0;
        for(int i=0;i<arr.size();i++){
            int sufSum=sum-prefSum-arr[i];
            if(sufSum==prefSum)
            return i;
            prefSum+=arr[i];
        }
        return -1;
    }
};
```

### 2. Longest Subarray with Sum K
Given an array arr[] containing integers and an integer k, your task is to find the length of the longest subarray where the sum of its elements is equal to the given value k. If there is no subarray with sum equal to k, return 0.

```cpp
class Solution {
  public:
    int longestSubarray(vector<int>& arr, int k) {
        map<int,int>mp;
        int sum=0,maxlen=0;
        for(int i=0;i<arr.size();i++){
            sum+=arr[i];
            if(sum==k)
                maxlen=max(maxlen,i+1);
            if(mp.find(sum-k)!=mp.end())
                maxlen=max(maxlen,i-mp[sum-k]);
            if(mp.find(sum)==mp.end())
                mp[sum]=i;
        }
        return maxlen;
    }
};
```

### 3. Largest subarray of 0's and 1's
Given an array arr of 0s and 1s. Find and return the length of the longest subarray with equal number of 0s and 1s.

```cpp
class Solution {
  public:
    int maxLen(vector<int> &arr) {
        int maxlen=0,sum=0;
        map<int,int>mp;
        for(int i=0;i<arr.size();i++){
            if(arr[i]==1)
            sum++;
            else
            sum--;
            if(sum==0)
            maxlen=max(maxlen,i+1);
            if(mp.find(sum)!=mp.end())
            maxlen=max(maxlen,i-mp[sum]);
            if(mp.find(sum)==mp.end())
            mp[sum]=i;
        }
        return maxlen;
    }
};
```

### 4. Product array puzzle
Given an array, arr[] construct a product array, res[] where each element in res[i] is the product of all elements in arr[] except arr[i]. Return this resultant array, res[].
Note: Each element is res[] lies inside the 32-bit integer range.

```cpp
class Solution {
  public:
    vector<int> productExceptSelf(vector<int>& arr) {
        vector<int>prefProd(arr.size()),sufProd(arr.size()),res(arr.size());
        prefProd[0]=1;
        for(int i=1;i<arr.size();i++)
            prefProd[i]=prefProd[i-1]*arr[i-1];
        sufProd[arr.size()-1]=1;
        for(int i=arr.size()-2;i>=0;i--)
            sufProd[i]=sufProd[i+1]*arr[i+1];
        for(int i=0;i<arr.size();i++)
            res[i]=prefProd[i]*sufProd[i];
        return res;
    }
};
```
