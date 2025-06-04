### 1. Count all triplets with given sum in sorted array
Difficulty: MediumAccuracy: 48.57%Submissions: 55K+Points: 4
Given a sorted array arr[] and a target value, the task is to count triplets (i, j, k) of valid indices, such that arr[i] + arr[j] + arr[k] = target and i < j < k.

```cpp
class Solution {
  public:
    int countTriplets(vector<int> &arr, int target) {
        int cnt=0;
        for(int i=0;i<arr.size()-2;i++){
            int j=i+1,k=arr.size()-1;
            while(j<k){
                if(arr[i]+arr[j]+arr[k]==target){
                    cnt++;
                    int j1=j;
                    while(j+1<k && arr[j]==arr[j+1]){
                        j++;
                        cnt++;
                    }
                    j=j1;
                    int k1=k;
                    while(k-1>j && arr[k]==arr[k-1]){
                        k--;
                        cnt++;
                    }
                    k=k1;
                    j++;
                    k--;
                }else if(arr[i]+arr[j]+arr[k]>target)
                k--;
                else
                j++;
            }
        }
        return cnt;
    }
};
```

### 2. Count Pairs whose sum is less than target
Given an array arr[] and an integer target. You have to find the number of pairs in the array whose sum is strictly less than the target.

```cpp
class Solution {
  public:
    int countPairs(vector<int> &arr, int target) {
        sort(arr.begin(),arr.end());
        int l=0,h=arr.size()-1,cnt=0;
        while(l<h){
            if(arr[l]+arr[h]<target)
            {cnt+=h-l;l++;}
            else
            h--;
        }
        return cnt;
    }
};
```

### 3. Sum Pair closest to target
Given an array arr[] and a number target, find a pair of elements (a, b) in arr[], where a<=b whose sum is closest to target.
Note: Return the pair in sorted order and if there are multiple such pairs return the pair with maximum absolute difference. If no such pair exists return an empty array.

```cpp
class Solution {
  public:
    vector<int> sumClosest(vector<int>& arr, int target) {
        sort(arr.begin(),arr.end());
        vector<int>res;
        if(arr.size()<=1)
        return res;
        res.push_back(-1);
        res.push_back(-1);
        int l=0,h=arr.size()-1,dist=INT_MAX;
        while(l<h){
            if(abs(arr[l]+arr[h]-target)<dist && arr[l]+arr[h]==target)
            {
                res[0]=arr[l];
                res[1]=arr[h];
                return res;
            }else if(abs(arr[l]+arr[h]-target)<dist && arr[l]+arr[h]>target)
            {
                dist=abs(arr[l]+arr[h]-target);
                res[0]=arr[l];
                res[1]=arr[h];
                h--;
            }
            else if(abs(arr[l]+arr[h]-target)<dist && arr[l]+arr[h]<target)
            {
                dist=abs(arr[l]+arr[h]-target);
                res[0]=arr[l];
                res[1]=arr[h];
                l++;
            }else if(arr[l]+arr[h]>target)
            h--;
            else
            l++;
        }
        return res;
    }
};
```

### 4. Pair with given sum in a sorted array
You are given an integer target and an array arr[]. You have to find number of pairs in arr[] which sums up to target. It is given that the elements of the arr[] are in sorted order.
Note: pairs should have elements of distinct indexes. 

```cpp
class Solution {
  public:
    int countPairs(vector<int> &arr, int target) {
        int cnt=0;
        int l=0,h=arr.size()-1;
        while(l<h){
            if(arr[l]+arr[h]==target)
            {
                cnt++;
                int l1=l;
                while(l+1<h && arr[l]==arr[l+1])
                {
                    cnt++;
                    l++;
                }
                l=l1;
                int h1=h;
                while(h-1>l && arr[h]==arr[h-1]){
                    cnt++;
                    h--;
                }
                h=h1;
                l++;
                h--;
            }else if(arr[l]+arr[h]>target)
                h--;
            else
                l++;
        }
        return cnt;
    }
};
```

### 5. Count the number of possible triangles
Given an integer array arr[]. Find the number of triangles that can be formed with three different array elements as lengths of three sides of the triangle. 

A triangle with three given sides is only possible if sum of any two sides is always greater than the third side.

```cpp
class Solution {
  public:
    // Function to count the number of possible triangles.
    int countTriangles(vector<int>& arr) {
        int cnt=0;
        sort(arr.begin(),arr.end());
        for(int i=2;i<arr.size();i++){
           int j=0,k=i-1;
           while(j<k){
               if(arr[i]<arr[j]+arr[k])
               {
                   cnt+=k-j;
                   k--;
               }else
                   j++;
           }
        }
        return cnt;
    }
};
```

### 6. Indexes of Subarray Sum
Given an array arr[] containing only non-negative integers, your task is to find a continuous subarray (a contiguous sequence of elements) whose sum equals a specified value target. You need to return the 1-based indices of the leftmost and rightmost elements of this subarray. You need to find the first subarray whose sum is equal to the target.

Note: If no such array is possible then, return [-1].

```cpp
class Solution {
  public:
    vector<int> subarraySum(vector<int> &arr, int target) {
        vector<int>res;
        int l=0,h=0,sum=0;
        while(h<arr.size()){
            if(sum>target){
                sum-=arr[l++];
                if(sum==target){
                    res.push_back(l+1);
                    res.push_back(h);
                    return res;
                }
            }else{
                sum+=arr[h++];
                if(sum==target){
                    res.push_back(l+1);
                    res.push_back(h);
                    return res;
                }
            }
        }
        while(l<h){
            if(sum>target){
                sum-=arr[l++];
                if(sum==target){
                    res.push_back(l+1);
                    res.push_back(h);
                    return res;
                }
            }else
                break;
        }
        res.push_back(-1);
        return res;
    }
};
```

### 7. Count distinct elements in every window
Given an integer array arr[] and a number k. Find the count of distinct elements in every window of size k in the array.

```cpp
class Solution {
  public:
    vector<int> countDistinct(vector<int> &arr, int k) {
        map<int,int>mp;
        vector<int>res;
        for(int i=0;i<k;i++)
        mp[arr[i]]++;
        res.push_back(mp.size());
        for(int i=0;i<arr.size()-k;i++){
            mp[arr[i]]--;
            if(mp[arr[i]]==0)
            mp.erase(arr[i]);
            mp[arr[i+k]]++;
            res.push_back(mp.size());
        }
        return res;
    }
};
```

### 8. Longest substring with distinct characters
Given a string s, find the length of the longest substring with all distinct characters. 

```cpp
class Solution {
  public:
    int longestUniqueSubstr(string &s) {
        map<char,int>mp;
        int maxlen=0,l=0,h=0;
        while(h<s.size()){
            if(mp.find(s[h])==mp.end())
            {mp[s[h]]=h;h++;}
            else
            {
                int idx=mp[s[h]]+1;
                for(;l<idx;l++)
                mp.erase(s[l]);
            }
            maxlen=max(maxlen,h-l);
        }
        return maxlen;
    }
};
```

### 9. Trapping Rain Water
Given an array arr[] with non-negative integers representing the height of blocks. If the width of each block is 1, compute how much water can be trapped between the blocks during the rainy season. 

```cpp
class Solution {
  public:
    int maxWater(vector<int> &arr) {
        stack<int>stk;
        int vol=0;
        for(int i=0;i<arr.size();i++){
            while(!stk.empty() && arr[stk.top()]<arr[i]){
                int curr=stk.top();
                stk.pop();
                if(stk.empty())
                break;
                int l=i-stk.top()-1;
                vol+=(min(arr[stk.top()],arr[i])-arr[curr])*l;
            }
            stk.push(i);
        }
        return vol;
    }
};
```
