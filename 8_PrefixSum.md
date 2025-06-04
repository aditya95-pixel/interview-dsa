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

### 5. Number of times graph cuts X-axis
Given an integer array arr[], where each arr[i] denotes the trajectory of the graph over the plane; i.e. arr[i]>0 means graph going above its current position by arr[i] value and arr[i]<0 means graph going down by arr[i] value. If initial position of the graph is at origin, determines the number of times graph crosses or touches the X-axis.

```cpp
class Solution {
  public:
    int touchedXaxis(vector<int>& arr) {
        int sum=arr[0],cnt=0;
        for(int i=1;i<arr.size();i++)
        {
            int prevsum=sum;
            sum+=arr[i];
            if(sum>prevsum && sum>=0 && prevsum<0)
            cnt++;
            else if(sum<prevsum && sum<=0 && prevsum>0)
            cnt++;
        }
        return cnt;
    }
};
```

### 6. Longest Subarray with Majority Greater than K
Given an array arr[] and an integer k, the task is to find the length of longest subarray in which the count of elements greater than k is more than the count of elements less than or equal to k.

```cpp
class Solution {
  public:
    int longestSubarray(vector<int> &arr, int k) {
        map<int,int>mp;
        int sum=0,maxlen=0;
        for(int i=0;i<arr.size();i++){
            if(arr[i]>k)
            sum++;
            else
            sum--;
            if(sum>0)
            maxlen=i+1;
            else{
                if(mp.find(sum-1)!=mp.end())
                maxlen=max(maxlen,i-mp[sum-1]);
            }
            if(mp.find(sum)==mp.end())
            mp[sum]=i;
        }
        return maxlen;
    }
};
```

### 7. Largest rectangular sub-matrix whose sum is 0
Given a matrix mat[][]. Find the size of the largest sub-matrix whose sum is equal to zero. The size of a matrix is the product of rows and columns. A sub-matrix is a matrix obtained from the given matrix by deletion of several (possibly, zero or all) rows/columns from the beginning and several (possibly, zero or all) rows/columns from the end.

```cpp
class Solution {
  public:
    int longestSubarray(vector<int>arr){
        map<int,int>mp;
        int sum=0,maxlen=0;
        for(int i=0;i<arr.size();i++)
        {
            sum+=arr[i];
            if(sum==0)
            maxlen=i+1;
            else if(mp.find(sum)!=mp.end())
            maxlen=max(maxlen,i-mp[sum]);
            else
            mp[sum]=i;
        }
        return maxlen;
    }
    int zeroSumSubmat(vector<vector<int>>& mat) {
        int maxArea=0;
        for(int i=0;i<mat.size();i++){
            vector<int>submatrix(mat[0].size(),0);
            for(int j=i;j<mat.size();j++){
                for(int k=0;k<mat[0].size();k++)
                    submatrix[k]+=mat[j][k];
                int len=longestSubarray(submatrix);
                maxArea=max(maxArea,(j-i+1)*len);
            }
        }
        return maxArea;
    }
};
```

### 8. Subarray Sum Divisible By K
You are given an integer array arr[] and a value k. The task is to find the count of all sub-arrays whose sum is divisible by k.

```cpp
class Solution {
  public:
    // Function to count the number of subarrays with a sum that is divisible by K
    int subCount(vector<int>& arr, int k) {
        int sum=0,cnt=0;
        map<int,int>mp;
        for(int i=0;i<arr.size();i++){
            sum=((sum+arr[i])%k+k)%k;
            if(sum==0)
            cnt++;
            cnt+=mp[sum];
            mp[sum]++;
        }
        return cnt;
    }
};
```

### 9. Longest subarray with sum divisible by K
Given an array arr[] and a positive integer k, find the length of the longest subarray with the sum of the elements divisible by k.
Note: If there is no subarray with sum divisible by k, then return 0.

```cpp
class Solution {
  public:
    int longestSubarrayDivK(vector<int>& arr, int k) {
        int maxlen=0,sum=0;
        map<int,int>mp;
        for(int i=0;i<arr.size();i++){
            sum=((sum+arr[i])%k+k)%k;
            if(sum==0)
            maxlen=i+1;
            else if(mp.find(sum)!=mp.end())
            maxlen=max(maxlen,i-mp[sum]);
            else
            mp[sum]=i;
        }
        return maxlen;
    }
};
```
