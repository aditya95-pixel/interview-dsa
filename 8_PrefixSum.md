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
