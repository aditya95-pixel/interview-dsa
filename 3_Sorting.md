### 1. Sort 0s, 1s and 2s
Given an array arr[] containing only 0s, 1s, and 2s. Sort the array in ascending order.

You need to solve this problem without utilizing the built-in sort function.

```cpp
class Solution {
  public:
    void sort012(vector<int>& arr) {
        int l=0,h=arr.size()-1,mid=0;
        while(mid<=h){
            if(arr[mid]==0)
            swap(arr[mid++],arr[l++]);
            else if(arr[mid]==1)
            mid++;
            else
            swap(arr[mid],arr[h--]);
        }
    }
};
```
