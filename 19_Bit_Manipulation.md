### 1. Find Only Repetitive Element from 1 to n-1
Given an array arr[] of size n, filled with numbers from 1 to n-1 in random order. The array has only one repetitive element. Your task is to find the repetitive element.

Note: It is guaranteed that there is a repeating element present in the array.

```cpp
class Solution {
  public:
    int findDuplicate(vector<int>& arr) {
        long long n=arr.size();
        long long apsum=n*(n-1)/2;
        long long sum=accumulate(arr.begin(),arr.end(),0);
        int diff=sum-apsum;
        return diff;
    }
};
```

### 2. Missing in Array
You are given an array arr[] of size n - 1 that contains distinct integers in the range from 1 to n (inclusive). This array represents a permutation of the integers from 1 to n with one element missing. Your task is to identify and return the missing element.

```cpp
class Solution {
  public:
    int missingNum(vector<int>& arr) {
        long long n=arr.size()+1;
        long long apsum=n*(n+1)/2;
        long long sum=accumulate(arr.begin(),arr.end(),0);
        int diff=apsum-sum;
        return diff;
    }
};
```

### 3. Unique Number I
Given a unsorted array arr[] of positive integers having all the numbers occurring exactly twice, except for one number which will occur only once. Find the number occurring only once.

```cpp
class Solution {
  public:
    int findUnique(vector<int> &arr) {
        int xoro=arr[0];
        for(int i=1;i<arr.size();i++)
        xoro^=arr[i];
        return xoro;
    }
};
```

### 4. Unique Number II
Given an array arr[] containing 2*n + 2 positive numbers, out of which 2*n numbers exist in pairs whereas only two number occur exactly once and are distinct. Find the other two numbers. Return the answer in increasing order.

```cpp
class Solution {
  public:
    vector<int> singleNum(vector<int>& arr) {
        int xoro=arr[0];
        for(int i=1;i<arr.size();i++)
        xoro^=arr[i];
        xoro&=(-xoro);
        vector<int>res(2,0);
        for(int i=0;i<arr.size();i++){
            if((arr[i]&xoro)==0)
            res[0]^=arr[i];
            else
            res[1]^=arr[i];
        }
        if(res[0]>res[1])
        swap(res[0],res[1]);
        return res;
    }
};
```

### 5. Total Hamming Distance
Given an integer array arr[], return the sum of Hamming distances between all the pairs of the integers in arr.

Note: The answer is guaranteed to fit within a 32-bit integer.

```cpp
class Solution {
  public:
    int totHammingDist(vector<int>& arr) {
        vector<int>cnt(32,0);
        for(int i=0;i<arr.size();i++){
            for(int j=0;j<32;j++){
                if(arr[i]&(1<<j))
                cnt[j]++;
            }
        }
        int res=0;
        for(int j=0;j<32;j++)
            res+=cnt[j]*(arr.size()-cnt[j]);
        return res;
    }
};
```

### 6. Subsets
Given an array arr[] of distinct positive integers, your task is to find all its subsets. The subsets should be returned in lexicographical order.

```cpp
class Solution {
  public:
    void solve(vector<int>&temp,vector<int>&arr,int idx,vector<vector<int>>&res){
        if(idx==arr.size()){
            res.push_back(temp);
            return ;
        }
        solve(temp,arr,idx+1,res);
        temp.push_back(arr[idx]);
        solve(temp,arr,idx+1,res);
        temp.pop_back();
    }
    vector<vector<int>> subsets(vector<int>& arr) {
        vector<vector<int>>res;
        vector<int>temp;
        solve(temp,arr,0,res);
        sort(res.begin(),res.end());
        return res;
    }
};
```
