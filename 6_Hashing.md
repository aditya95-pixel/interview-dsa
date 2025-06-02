### 1. Two Sum - Pair with Given Sum
Given an array arr[] of positive integers and another integer target. Determine if there exist two distinct indices such that the sum of their elements is equal to the target.

```cpp
class Solution {
  public:
    bool twoSum(vector<int>& arr, int target) {
        map<int,int>mp;
        for(auto x:arr){
            mp[x]++;
            if(mp.find(target-x)!=mp.end() && x!=target-x)
            return true;
            else if(mp.find(target-x)!=mp.end() && x==target-x && mp[x]>1)
            return true;
        }
        return false;
    }
};
```

### 2. Count pairs with given sum
Given an array arr[] and an integer target. You have to find numbers of pairs in array arr[] which sums up to given target.

```cpp
class Solution {
  public:
    int countPairs(vector<int> &arr, int target) {
        int cnt=0;
        map<int,int>mp;
        for(auto x:arr)
        mp[x]++;
        for(auto item:mp){
            if(mp.find(target-item.first)!=mp.end() &&
            item.first!=target-item.first)
            cnt+=item.second*mp[target-item.first];
            else if(mp.find(target-item.first)!=mp.end() &&
            item.first==target-item.first)
            cnt+=item.second*(item.second-1);
        }
        return cnt/2;
    }
};
```

### 3. Find All Triplets with Zero Sum
Given an array arr[], find all possible triplets i, j, k in the arr[] whose sum of elements is equals to zero. 
Returned triplet should also be internally sorted i.e. i<j<k.

```cpp
class Solution {
  public:
    vector<vector<int>> findTriplets(vector<int> &arr) {
        vector<vector<int>>res;
        map<int,vector<int>>mp;
        for(int j=0;j<arr.size();j++){
            for(int k=j+1;k<arr.size();k++){
                int val=-1*(arr[j]+arr[k]);
                if(mp.find(val)!=mp.end())
                {
                    for(auto i:mp[val])
                    res.push_back({i,j,k});
                }
            }
            mp[arr[j]].push_back(j);
        }
        return res;
    }
};
```

### 4. Intersection of Two arrays with Duplicate Elements
Given two integer arrays a[] and b[], you have to find the intersection of the two arrays. Intersection of two arrays is said to be elements that are common in both arrays. The intersection should not have duplicate elements and the result should contain items in any order.

Note: The driver code will sort the resulting array in increasing order before printing.

```cpp
class Solution {
  public:
    vector<int> intersectionWithDuplicates(vector<int>& a, vector<int>& b) {
        vector<int>res;
        map<int,int>mp;
        for(auto x:a)
        {
            if(mp.find(x)==mp.end())
            mp[x]++;
        }
        for(auto x:b)
        {
            if(mp.find(x)!=mp.end() && mp[x]>0)
            mp[x]--;
        }
        for(auto item:mp)
        {
            if(item.second==0)
            res.push_back(item.first);
        }
        return res;
    }
};
```

### 5. Union of Arrays with Duplicates
Given two arrays a[] and b[], the task is to find the number of elements in the union between these two arrays.

The Union of the two arrays can be defined as the set containing distinct elements from both arrays. If there are repetitions, then only one element occurrence should be there in the union.

Note: Elements of a[] and b[] are not necessarily distinct.

```cpp
class Solution {
  public:
    // Function to return the count of number of elements in union of two arrays.
    int findUnion(vector<int>& a, vector<int>& b) {
        set<int>s;
        for(auto x:a)
        s.insert(x);
        for(auto x:b)
        s.insert(x);
        return s.size();
    }
};
```

### 6. Longest Consecutive Subsequence
Given an array arr[] of non-negative integers. Find the length of the longest sub-sequence such that elements in the subsequence are consecutive integers, the consecutive numbers can be in any order.

```cpp
class Solution {
  public:
    // Function to return length of longest subsequence of consecutive integers.
    int longestConsecutive(vector<int>& arr) {
        set<int>s;
        int res=0;
        for(auto x:arr)
        s.insert(x);
        for(auto ele:s){
            if(s.find(ele)!=s.end() && s.find(ele-1)==s.end()){
                int curr=ele,cnt=0;
                while(s.find(curr)!=s.end()){
                    s.erase(curr);
                    curr++;
                    cnt++;
                }
                res=max(res,cnt);
            }
        }
        return res;
    }
};
```

### 7. Print Anagrams Together
Given an array of strings, return all groups of strings that are anagrams. The strings in each group must be arranged in the order of their appearance in the original array. Refer to the sample case for clarification.

```cpp
class Solution {
  public:
    string hash(string s){
        vector<char>freq(26,0);
        for(auto x:s)
            freq[x-'a']++;
        string h;
        for(auto x:freq)
        h+=to_string(x);
        return h;
    }
    vector<vector<string>> anagrams(vector<string>& arr) {
        vector<vector<string>>res;
        map<string,vector<string>>mp;
        for(auto word:arr){
            string key=hash(word);
            mp[key].push_back(word);
        }
        for(auto item:mp)
            res.push_back(item.second);
        return res;
    }
};
```

### 8. Subarrays with sum K
Given an unsorted array of integers, find the number of subarrays having sum exactly equal to a given number k.

```cpp
class Solution {
  public:
    int countSubarrays(vector<int> &arr, int k) {
        map<int,int>mp;
        int sum=0,cnt=0;
        mp[0]=1;
        for(auto x:arr){
            sum+=x;
            if(mp.find(sum-k)!=mp.end())
            cnt+=mp[sum-k];
            mp[sum]++;
        }
        return cnt;
    }
};
```

### 9. Count Subarrays with given XOR
Given an array of integers arr[] and a number k, count the number of subarrays having XOR of their elements as k.

```cpp
class Solution {
  public:
    long subarrayXor(vector<int> &arr, int k) {
        long cnt=0;
        map<int,int>mp;
        mp[0]=1;
        int total=0;
        for(auto x:arr){
            total^=x;
            if(mp.find(total^k)!=mp.end())
            cnt+=mp[total^k];
            mp[total]++;
        }
        return cnt;
    }
};
```

### 10. Roman Number to Integer
Given a string in Roman number format (s), your task is to convert it to an integer. Various symbols and their values are given below.
Note: I = 1, V = 5, X = 10, L = 50, C = 100, D = 500, M = 1000

```cpp
class Solution {
  public:
    int romanToDecimal(string &s) {
        map<char,int>mp={{'I',1},{'V',5},{'X',10},
        {'L',50},{'C',100},{'D',500},{'M',1000}};
        int res=0;
        map<char,bool>mp2={{'I',0},{'V',0},{'X',0},
        {'L',0},{'C',0},{'D',0},{'M',0}};
        for(auto x:s){
            if(x=='I'){
                mp2[x]=1;
                res++;
            }
            else if(x=='V'){
                mp2['V']=1;
                if(mp2['I'])
                    res+=3;
                else
                    res+=5;
            }
            else if(x=='X'){
                mp2['X']=1;
                if(mp2['I'])
                    res+=8;
                else
                    res+=10;
            }
            else if(x=='L'){
                mp2['L']=1;
                if(mp2['X'])
                    res+=30;
                else
                    res+=50;
            }
            else if(x=='C'){
                mp2['C']=1;
                if(mp2['X'])
                    res+=80;
                else
                    res+=100;
            }
            else if(x=='D'){
                mp2['D']=1;
                if(mp2['C'])
                    res+=300;
                else
                    res+=500;
            }
            else if(x=='M'){
                mp2['M']=1;
                if(mp2['C'])
                    res+=800;
                else
                    res+=1000;
            }
        }
        return res;
    }
};
```
