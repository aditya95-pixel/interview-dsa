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

### 2. Sorted and Rotated Minimum
A sorted array of distinct elements arr[] is rotated at some unknown point, the task is to find the minimum element in it. 

```cpp
class Solution {
  public:
    int findMin(vector<int>& arr) {
        int l=0,h=arr.size()-1;
        while(l<=h){
            if(arr[l]<=arr[h])
            return arr[l];
            int mid=l+(h-l)/2;
            if(arr[mid]>arr[h])
            l=mid+1;
            else
            h=mid;
        }
        return l;
    }
};
```
