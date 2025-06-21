### 1. K Sized Subarray Maximum
Given an array arr[] of integers and an integer k, your task is to find the maximum value for each contiguous subarray of size k. The output should be an array of maximum values corresponding to each contiguous subarray.

```cpp
class Solution {
  public:
    vector<int> maxOfSubarrays(vector<int>& arr, int k) {
        deque<int>dq;
        for(int i=0;i<k;i++)
        {
            while(!dq.empty() && arr[i]>=arr[dq.back()])
            dq.pop_back();
            dq.push_back(i);
        }
        vector<int>res;
        for(int i=k;i<arr.size();i++){
            res.push_back(arr[dq.front()]);
            while(!dq.empty() && dq.front()<=i-k)
            dq.pop_front();
            while(!dq.empty() && arr[i]>=arr[dq.back()])
            dq.pop_back();
            dq.push_back(i);
        }
        res.push_back(arr[dq.front()]);
        return res;
    }
};
```

### 2. Longest Bounded-Difference Subarray
Given an array of positive integers arr[] and a non-negative integer x, the task is to find the longest sub-array where the absolute difference between any two elements is not greater than x.
If multiple such subarrays exist, return the one that starts at the smallest index.

```cpp
class Solution {
  public:
    vector<int> longestSubarray(vector<int>& arr, int x) {
        deque<int>maxq,minq;
        int end=0,start=0,resStart=0,resEnd=0;
        while(end<arr.size()){
            while(!maxq.empty() && arr[maxq.back()]<arr[end])
            maxq.pop_back();
            while(!minq.empty() && arr[minq.back()]>arr[end])
            minq.pop_back();
            maxq.push_back(end);
            minq.push_back(end);
            while(arr[maxq.front()]-arr[minq.front()]>x){
                if(start==maxq.front())
                maxq.pop_front();
                if(start==minq.front())
                minq.pop_front();
                start++;
            }
            if(end-start>resEnd-resStart)
            {
                resEnd=end;
                resStart=start;
            }
            end++;
        }
        vector<int>res;
        for(int i=resStart;i<=resEnd;i++)
        res.push_back(arr[i]);
        return res;
    }
};
```

### 3. Minimum K Consecutive Bit Flips
Given a binary array arr[] and an integer k.

A k-bit flip involves selecting a contiguous subarray of length k from arr[] and flipping all its bits - changing every 0 to 1 and every 1 to 0 simultaneously.

Your task is to return the minimum number of k-bit flips needed to eliminate all 0s from the array. If it is impossible to achieve, return -1.

```cpp
class Solution {
  public:
    int kBitFlips(vector<int>& arr, int k) {
        queue<int>q;
        bool flag=false;
        int cnt=0;
        for(int i=0;i<arr.size();i++){
            if(i+1>k){
                flag^=q.front();
                q.pop();
            }
            if(flag)
            arr[i]^=1;
            if(arr[i]==0){
                if(i+k>arr.size())
                return -1;
                arr[i]^=1;
                cnt++;
                flag^=1;
                q.push(1);
            }else
            q.push(0);
        }
        return cnt;
    }
};
```

### 4. Maximum score from at most K jumps
Given an array arr[] and an integer k, your task is to collect the maximum score possible by performing the following operations:
1. Start from the 0th index of the array.
2. Reach the last index of the array by jumping at most k indices in each move.
3. Add the value stored at each index where you reach after the jump (consider the score of the starting index too).

Note: 0-based indexing is followed.

```cpp
class Solution {
  public:
    int getScore(vector<int>& arr, int k) {
        vector<int>dp(arr.size(),INT_MIN);
        deque<int>dq;
        dp[arr.size()-1]=arr[arr.size()-1];
        dq.push_back(arr.size()-1);
        for(int i=arr.size()-2;i>=0;i--){
            while(!dq.empty() && dq.front()>i+k)
            dq.pop_front();
            dp[i]=arr[i]+dp[dq.front()];
            while(!dq.empty() && dp[i]>=dp[dq.back()])
            dq.pop_back();
            dq.push_back(i);
        }
        return dp[0];
    }
};
```
