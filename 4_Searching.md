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

### 3. Search in Rotated Sorted Array
Given a sorted and rotated array arr[] of distinct elements, the task is to find the index of a target key. Return -1 if the key is not found.

```cpp
class Solution {
  public:
    int search(vector<int>& arr, int key) {
        int l=0,h=arr.size()-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(arr[mid]==key)
            return mid;
            else if(arr[mid]<=arr[h]){
                if(key>arr[mid] && key<=arr[h])
                l=mid+1;
                else
                h=mid-1;
            }
            else if(arr[mid]>=arr[l]){
                if(key<arr[mid] && key>=arr[l])
                h=mid-1;
                else
                l=mid+1;
            }
        }
        return -1;
    }
};
```
