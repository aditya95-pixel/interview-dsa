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

### 4. Peak element
Given an array arr[] where no two adjacent elements are same, find the index of a peak element. An element is considered to be a peak if it is greater than its adjacent elements (if they exist). If there are multiple peak elements, return index of any one of them. The output will be "true" if the index returned by your function is correct; otherwise, it will be "false".

Note: Consider the element before the first element and the element after the last element to be negative infinity.

```cpp
class Solution {
  public:
    int peakElement(vector<int> &arr) {
        if(arr.size()==1)
        return 0;
        if(arr[0]>arr[1])
        return 0;
        if(arr[arr.size()-1]>arr[arr.size()-2])
        return arr.size()-1;
        int l=1,h=arr.size()-2;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(arr[mid]>arr[mid+1] && arr[mid]>arr[mid-1])
            return mid;
            else if(arr[mid]<arr[mid+1])
            l=mid+1;
            else
            h=mid-1;
        }
        return -1;
    }
};
```

### 5. K-th element of two Arrays
Given two sorted arrays a[] and b[] and an element k, the task is to find the element that would be at the kth position of the combined sorted array.

```cpp
class Solution {
  public:
    int kthElement(vector<int>& a, vector<int>& b, int k) {
        int c=0,i=0,j=0,ele;
        while(i<a.size() && j<b.size()){
            if(a[i]<b[j])
            ele=a[i++];
            else
            ele=b[j++];
            c++;
            if(c==k)
            return ele;
        }
        while(i<a.size()){
            c++;
            if(c==k)
            return a[i];
            i++;
        }
        while(j<b.size()){
            c++;
            if(c==k)
            return b[j];
            j++;
        }
        return -1;
    }
};
```

### 6. Aggressive Cows
You are given an array with unique elements of stalls[], which denote the position of a stall. You are also given an integer k which denotes the number of aggressive cows. Your task is to assign stalls to k cows such that the minimum distance between any two of them is the maximum possible.

```cpp
class Solution {
  public:
    bool check(vector<int>stalls,int k,int gap){
        int cnt=1;
        int last=stalls[0];
        for(int i=1;i<stalls.size();i++){
            if(stalls[i]-last>=gap)
            {
                last=stalls[i];
                cnt++;
            }
        }
        return cnt>=k;
    }
    int aggressiveCows(vector<int> &stalls, int k) {
        sort(stalls.begin(),stalls.end());
        int l=1,h=stalls[stalls.size()-1]-stalls[0];
        int gap=1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(check(stalls,k,mid)){
                gap=mid;
                l=mid+1;
            }else{
                h=mid-1;
            }
        }
        return gap;
    }
};
```

### 7. Allocate Minimum Pages
You are given an array arr[] of integers, where each element arr[i] represents the number of pages in the ith book. You also have an integer k representing the number of students. The task is to allocate books to each student such that:

Each student receives atleast one book.
Each student is assigned a contiguous sequence of books.
No book is assigned to more than one student.
The objective is to minimize the maximum number of pages assigned to any student. In other words, out of all possible allocations, find the arrangement where the student who receives the most pages still has the smallest possible maximum.

Note: Return -1 if a valid assignment is not possible, and allotment should be in contiguous order (see the explanation for better understanding).

```cpp
class Solution {
  public:
    bool check(vector<int>arr,int k,int pages){
        int cnt=1;
        int sum=0;
        for(int i=0;i<arr.size();i++){
            sum+=arr[i];
            if(sum>pages){
                cnt++;
                sum=arr[i];
            }
        }
        return (cnt<=k);
    }
    int findPages(vector<int> &arr, int k) {
        if(k>arr.size())
        return -1;
        int l=*max_element(arr.begin(),arr.end());
        int h=accumulate(arr.begin(),arr.end(),0);
        int maxpages=h;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(check(arr,k,mid))
            {
                maxpages=mid;
                h=mid-1;
            }
            else{
                l=mid+1;
            }
        }
        return maxpages;
    }
};
```

### 8. Kth Missing Positive Number in a Sorted Array
Given a sorted array of distinct positive integers arr[], we need to find the kth positive number that is missing from arr[].  

```cpp
class Solution {
  public:
    int kthMissing(vector<int> &arr, int k) {
        int l=0,h=arr.size()-1,res=arr.size()+k;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(arr[mid]>mid+k)
            {
                h=mid-1;
                res=mid+k;
            }else
                l=mid+1;
        }
        return res;
    }
};
```

### 9. Implement Lower Bound
Given a sorted array arr[] and a number target, the task is to find the lower bound of the target in this given array. The lower bound of a number is defined as the smallest index in the sorted array where the element is greater than or equal to the given number.

Note: If all the elements in the given array are smaller than the target, the lower bound will be the length of the array. 

```cpp
class Solution {
  public:
    int lowerBound(vector<int>& arr, int target) {
        int l=0,h=arr.size()-1;
        int res=arr.size();
        while(l<=h){
            int mid=l+(h-l)/2;
            if(arr[mid]>=target){
                res=mid;
                h=mid-1;
            }
            else{
                l=mid+1;
            }
        }
        return res;
    }
};
```

### 10. Bitonic Point
Given an array of integers arr[] that is first strictly increasing and then maybe strictly decreasing, find the bitonic point, that is the maximum element in the array.
Bitonic Point is a point before which elements are strictly increasing and after which elements are strictly decreasing.

Note: It is guaranteed that the array contains exactly one bitonic point.

```cpp
class Solution {
  public:
    int findMaximum(vector<int> &arr) {
        if(arr.size()==1)
        return arr[0];
        if(arr[arr.size()-1]>arr[arr.size()-2])
        return arr[arr.size()-1];
        if(arr[0]>arr[1])
        return arr[0];
        int l=1,h=arr.size()-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(arr[mid]>arr[mid-1] && arr[mid]>arr[mid+1])
            return arr[mid];
            else if(arr[mid]<arr[mid+1])
            l=mid+1;
            else
            h=mid-1;
        }
        return -1;
    }
};
```

### 11. Median of 2 Sorted Arrays of Different Sizes
Given two sorted arrays a[] and b[], find and return the median of the combined array after merging them into a single sorted array.

```cpp
class Solution {
  public:
    double medianOf2(vector<int>& a, vector<int>& b) {
        if(a.size()>b.size())
        return medianOf2(b,a);
        int l=0,h=a.size();
        while(l<=h){
            int mid1=l+(h-l)/2;
            int mid2=(a.size()+b.size()+1)/2-mid1;
            int l1=(mid1==0 ? INT_MIN:a[mid1-1]);
            int r1=(mid1==a.size() ? INT_MAX:a[mid1]);
            int l2=(mid2==0 ? INT_MIN:b[mid2-1]);
            int r2=(mid2==b.size() ? INT_MAX:b[mid2]);
            if(l1<=r2 && l2<=r1){
                if((a.size()+b.size())%2==0)
                return (max(l1,l2)+min(r1,r2))/2.0;
                else
                return max(l1,l2);
            }
            if(l1>r2)
            h=mid1-1;
            else
            l=mid1+1;
        }
        return 0;
    }
};
```

### 12. Square Root
Given a positive integer n, find the square root of n. If n is not a perfect square, then return the floor value.

Floor value of any number is the greatest Integer which is less than or equal to that number

```cpp
class Solution {
  public:
    int floorSqrt(int n) {
        if(n==1)
        return 1;
        int l=1,h=n/2;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(mid*mid==n)
            return mid;
            else if(mid*mid>n)
            h=mid-1;
            else
            l=mid+1;
        }
        return h;
    }
};
```

### 13. Koko Eating Bananas
Given an array arr[] of integers where each element represents a pile of bananas, and Koko has k hours to finish all the piles, find the minimum number of bananas (s) Koko must eat per hour to finish all the bananas within k hours. Each hour, Koko chooses a pile and eats s bananas from it. If the pile has fewer than s bananas, she consumes the entire pile for that hour and won't eat any other banana during that hour.

```cpp
class Solution {
  public:
    bool check(vector<int>arr,int k,int bananas){
        int cnt=0;
        for(int i=0;i<arr.size();i++){
            if(arr[i]%bananas==0)
            cnt+=arr[i]/bananas;
            else
            cnt+=arr[i]/bananas+1;
        }
        return cnt<=k;
    }
    int kokoEat(vector<int>& arr, int k) {
        if(k==arr.size())
        return *max_element(arr.begin(),arr.end());
        int l=1,h=*max_element(arr.begin(),arr.end());
        int res=h;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(check(arr,k,mid)){
                res=mid;
                h=mid-1;
            }
            else{
                l=mid+1;
            }
        }
        return res;
    }
};
```

### 14. Minimum days to make M bouquets
You have a row of flowers, where each flower blooms after a specific day. The array arr represents the blooming schedule: arr[i] is the day the flower at position i will bloom. To create a bouquet, you need to collect k adjacent bloomed flowers. Each flower can only be used in one bouquet.

Your goal is to find the minimum number of days required to make exactly m bouquets. If it is not possible to make m bouquets with the given arrangement, return -1.

```cpp
class Solution {
  public:
    bool check(int m,int k,vector<int>arr,int days){
        int cnt=0;
        int flcnt=0;
        for(int i=0;i<arr.size();i++){
            if(arr[i]>days)
            flcnt=0;
            else{
                flcnt++;
                if(flcnt==k)
                {cnt++;flcnt=0;}
            }
        }
        return cnt>=m;
    }
    int minDaysBloom(int m, int k, vector<int> &arr) {
        if(m*k>arr.size())
        return -1;
        int l=1,h=*max_element(arr.begin(),arr.end());
        int res=h;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(check(m,k,arr,mid)){
                res=mid;
                h=mid-1;
            }
            else{
                l=mid+1;
            }
        }
        return res;
    }
};
```
