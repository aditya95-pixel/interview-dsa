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

### 4. Overlapping Intervals
Given an array of Intervals arr[][], where arr[i] = [starti, endi]. The task is to merge all of the overlapping Intervals.

```cpp
class Solution {
  public:
    vector<vector<int>> mergeOverlap(vector<vector<int>>& arr) {
        sort(arr.begin(),arr.end());
        vector<vector<int>>res;
        res.push_back(arr[0]);
        for(int i=1;i<arr.size();i++){
            vector<int>&last=res.back();
            if(arr[i][0]<=last[1])
            last[1]=max(arr[i][1],last[1]);
            else
            res.push_back(arr[i]);
        }
        return res;
    }
};
```

### 5. Insert Interval
Geek has an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith event and intervals is sorted in ascending order by starti. He wants to add a new interval newInterval= [newStart, newEnd] where newStart and newEnd represent the start and end of this interval.

Help Geek to insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

```cpp
class Solution {
  public:
    vector<vector<int>> insertInterval(vector<vector<int>> &intervals,
                                       vector<int> &newInterval) {
        int i=0;
        while(i<intervals.size() && newInterval[0]>intervals[i][0])
        i++;
        intervals.insert(intervals.begin()+i,newInterval);
        vector<vector<int>>res;
        res.push_back(intervals[0]);
        for(int i=1;i<intervals.size();i++){
            vector<int>&last=res.back();
            if(intervals[i][0]<=last[1])
            last[1]=max(last[1],intervals[i][1]);
            else
            res.push_back(intervals[i]);
        }
        return res;
    }
};
```

### 6. Non-overlapping Intervals
Given a 2D array intervals[][] of representing intervals where intervals [i] = [starti, endi ]. Return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

```cpp
class Solution {
  public:
    static bool compare(vector<int>&a,vector<int>&b){
        return a[1]<b[1];
    }
    int minRemoval(vector<vector<int>> &intervals) {
        sort(intervals.begin(),intervals.end(),compare);
        int last=intervals[0][1],cnt=0;
        for(int i=1;i<intervals.size();i++){
            if(intervals[i][0]<last)
            cnt++;
            else
            last=intervals[i][1];
        }
        return cnt;
    }
};
```

### 7. Merge Without Extra Space
Given two sorted arrays a[] and b[] of size n and m respectively, the task is to merge them in sorted order without using any extra space. Modify a[] so that it contains the first n elements and modify b[] so that it contains the last m elements.

```cpp
class Solution {
  public:
    void mergeArrays(vector<int>& a, vector<int>& b) {
        int i=a.size()-1,j=0;
        while(i>=0 && j<b.size() && a[i]>b[j])
        swap(a[i--],b[j++]);
        sort(a.begin(),a.end());
        sort(b.begin(),b.end());
    }
};
```

### 8. Minimum sum
Given an array arr[] such that each element is in the range [0, 9] find the minimum possible sum of two numbers formed using the elements of the array. All digits in the given array must be used to form the two numbers. Return a string without leading zeroes.

```cpp
class Solution {
  public:
    string minSum(vector<int> &arr) {
        map<int,int>freq;
        for(auto i:arr)
        freq[i]++;
        string s1,s2;
        bool chk=true;
        for(int dig=0;dig<=9;dig++){
            while(freq[dig]--){
                if(chk){
                    if(!(s1.empty() && dig==0))
                    s1+=(char)(dig+48);
                    chk=false;
                }else{
                    if(!(s2.empty() && dig==0))
                    s2+=(char)(dig+48);
                    chk=true;
                }
            }
        }
        reverse(s1.begin(),s1.end());
        reverse(s2.begin(),s2.end());
        int i=0,j=0,k=0;
        string res;
        int carry=0;
        while(i<s1.size() && j<s2.size()){
            int sum=(s1[i]-48)+(s2[j]-48)+carry;
            res+=(char)(sum%10+48);
            carry=sum/10;
            i++;
            j++;
        }
        while(i<s1.size()){
            int sum=(s1[i]-48)+carry;
            res+=(char)(sum%10+48);
            carry=sum/10;
            i++;
        }
        while(j<s2.size()){
            int sum=(s2[j]-48)+carry;
            res+=(char)(sum%10+48);
            carry=sum/10;
            j++;
        }
        if(carry!=0)
        res+=(char)(carry+48);
        reverse(res.begin(),res.end());
        return res;
    }
};
```

### 9. Meeting Rooms
Given an array arr[][] such that arr[i][0] is the starting time of ith meeting and arr[i][1] is the ending time of ith meeting, the task is to check if it is possible for a person to attend all the meetings such that he can attend only one meeting at a particular time.

Note: A person can attend a meeting if its starting time is greater than or equal to the previous meeting's ending time.

```cpp
class Solution {
  public:
    static bool compare(vector<int>&a,vector<int>&b){
        return a[1]<b[1];
    }
    bool canAttend(vector<vector<int>> &arr) {
        sort(arr.begin(),arr.end(),compare);
        int last=arr[0][1];
        for(int i=1;i<arr.size();i++){
            if(arr[i][0]<last)
            return false;
            else
            last=arr[i][1];
        }
        return true;
    }
};
```

### 10.Form the Largest Number
Given an array of integers arr[] representing non-negative integers, arrange them so that after concatenating all of them in order, it results in the largest possible number. Since the result may be very large, return it as a string.

```cpp
class Solution {
  public:
    static bool compare(const int &a,const int &b){
        string a1=to_string(a);
        string b1=to_string(b);
        return (a1+b1)>=(b1+a1);
    }
    string findLargest(vector<int> &arr) {
        sort(arr.begin(),arr.end(),compare);
        string res;
        if(arr[0]==0)
        return "0";
        for(auto i:arr)
        res+=to_string(i);
        return res;
    }
};
```
