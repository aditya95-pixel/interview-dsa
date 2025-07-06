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
