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

### 2. Find H-Index
Given an integer array citations[], where citations[i] is the number of citations a researcher received for the ith paper. The task is to find the H-index.

H-Index is the largest value such that the researcher has at least H papers that have been cited at least H times.

```cpp
class Solution {
  public:
    // Function to find hIndex
    int hIndex(vector<int>& citations) {
        vector<int>freq(citations.size()+1,0);
        for(int i=0;i<citations.size();i++)
        {
            if(citations[i]>citations.size())
            freq[citations.size()]++;
            else
            freq[citations[i]]++;
        }
        int sum=0;
        for(int i=citations.size();i>=0;i--){
            sum+=freq[i];
            if(sum>=i)
            return i;
        }
        return 0;
    }
};
```
