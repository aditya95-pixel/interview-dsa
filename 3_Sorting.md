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

### 3. Count Inversions
Given an array of integers arr[]. Find the Inversion Count in the array.
Two elements arr[i] and arr[j] form an inversion if arr[i] > arr[j] and i < j.

Inversion Count: For an array, inversion count indicates how far (or close) the array is from being sorted. If the array is already sorted then the inversion count is 0.
If an array is sorted in the reverse order then the inversion count is the maximum. 

```cpp
class Solution {
  public:
    // Function to count inversions in the array.
    int merge(vector<int>&arr,int l,int mid,int h){
        int res=0;
        int n1=mid-l+1,n2=h-mid;
        vector<int>arr1(n1),arr2(n2);
        for(int i=0;i<n1;i++)
            arr1[i]=arr[l+i];
        for(int i=0;i<n2;i++)
            arr2[i]=arr[mid+i+1];
        int i=0,j=0,k=l;
        while(i<n1 && j<n2){
            if(arr1[i]<=arr2[j])
            arr[k++]=arr1[i++];
            else
            {
                arr[k++]=arr2[j++];
                res+=(n1-i);
            }
        }
        while(i<n1)
        arr[k++]=arr1[i++];
        while(j<n2)
        arr[k++]=arr2[j++];
        return res;
    }
    int mergesort(vector<int>&arr,int l,int h){
        int res=0;
        if(l<h){
            int mid=l+(h-l)/2;
            res+=mergesort(arr,l,mid);
            res+=mergesort(arr,mid+1,h);
            res+=merge(arr,l,mid,h);
        }
        return res;
    }
    int inversionCount(vector<int> &arr) {
        return mergesort(arr,0,arr.size()-1);
    }
};
```
