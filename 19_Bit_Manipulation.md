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
