# Interview important DSA questions

## Array

### 1. Second Largest

Given an array of positive integers arr[], return the second largest element from the array. If the second largest element doesn't exist then return -1.

Note: The second largest element should not be equal to the largest element.

```cpp
class Solution {
  public:
    int getSecondLargest(vector<int> &arr) {
        int largest=-1,second_largest=-1;
        for(auto num:arr){
            if(num>largest){
                second_largest=largest;
                largest=num;
            }
            else if(num<largest && num>second_largest)
            second_largest=num;
        }
        return second_largest;
    }
};
```

### 2. Move All Zeroes to End

You are given an array arr[] of non-negative integers. Your task is to move all the zeros in the array to the right end while maintaining the relative order of the non-zero elements. The operation must be performed in place, meaning you should not use extra space for another array.

```cpp
class Solution {
  public:
    void pushZerosToEnd(vector<int>& arr) {
        int k=0;
        for(int i=0;i<arr.size();i++){
            if(arr[i]!=0)
            {
                swap(arr[k],arr[i]);
                k++;
            }
        }
    }
};
```
