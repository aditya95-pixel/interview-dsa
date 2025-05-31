### 1. Number of occurrence
Given a sorted array, arr[] and a number target, you need to find the number of occurrences of target in arr[].  

```cpp
class Solution {
  public:
    int countFreq(vector<int>& arr, int target) {
        int l=lower_bound(arr.begin(),arr.end(),target)-arr.begin();
        int h=upper_bound(arr.begin(),arr.end(),target)-arr.begin();
        return h-l;
    }
};
```
