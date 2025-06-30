### 1. Minimum Platforms
You are given the arrival times arr[] and departure times dep[] of all trains that arrive at a railway station on the same day. Your task is to determine the minimum number of platforms required at the station to ensure that no train is kept waiting.

At any given time, the same platform cannot be used for both the arrival of one train and the departure of another. Therefore, when two trains arrive at the same time, or when one arrives before another departs, additional platforms are required to accommodate both trains.

```cpp
class Solution {
  public:
    // Function to find the minimum number of platforms required at the
    // railway station such that no train waits.
    int findPlatform(vector<int>& arr, vector<int>& dep) {
       sort(arr.begin(),arr.end());
       sort(dep.begin(),dep.end());
       int res=0,platform=0,i=0,j=0;
       while(i<arr.size() && j<dep.size()){
           if(arr[i]<=dep[j])
           {
               platform++;
               i++;
           }else{
               platform--;
               j++;
           }
           res=max(res,platform);
       }
       return res;
    }
};
```

### 2. Activity Selection
You are given a set of activities, each with a start time and a finish time, represented by the arrays start[] and finish[], respectively. A single person can perform only one activity at a time, meaning no two activities can overlap. Your task is to determine the maximum number of activities that a person can complete in a day.

```cpp
class Solution {
  public:
    static bool compare(const pair<int,int>&a,const pair<int,int>&b){
        return a.second<b.second;
    }
    int activitySelection(vector<int> &start, vector<int> &finish) {
        vector<pair<int,int>>vp;
        for(int i=0;i<start.size();i++)
            vp.push_back({start[i],finish[i]});
        sort(vp.begin(),vp.end(),compare);
        int j=0,cnt=1;
        for(int i=1;i<vp.size();i++){
            if(vp[i].first>vp[j].second){
                cnt++;
                j=i;
            }
        }
        return cnt;
    }
};
```

### 3. Job Sequencing Problem
You are given two arrays: deadline[], and profit[], which represent a set of jobs, where each job is associated with a deadline, and a profit. Each job takes 1 unit of time to complete, and only one job can be scheduled at a time. You will earn the profit associated with a job only if it is completed by its deadline.

Your task is to find:

The maximum number of jobs that can be completed within their deadlines.
The total maximum profit earned by completing those jobs.

```cpp
class Solution {
  public:
    vector<int> jobSequencing(vector<int> &deadline, vector<int> &profit) {
        vector<pair<int,int>>vp;
        for(int i=0;i<deadline.size();i++)
            vp.push_back({deadline[i],profit[i]});
        sort(vp.begin(),vp.end());
        vector<int>res(2,0);
        priority_queue<int,vector<int>,greater<int>>pq;
        for(int i=0;i<vp.size();i++){
            if(vp[i].first>pq.size())
                pq.push(vp[i].second);
            else if(!pq.empty() && vp[i].second>pq.top()){
                pq.pop();
                pq.push(vp[i].second);
            }
        }
        while(!pq.empty()){
            res[0]++;
            res[1]+=pq.top();
            pq.pop();
        }
        return res;
    }
};
```

### 4. Gas Station
There are some gas stations along a circular route. You are given two integer arrays gas[] denoted as the amount of gas present at each station and cost[] denoted as the cost of travelling to the next station. You have a car with an unlimited gas tank. You begin the journey with an empty tank from one of the gas stations. Return the index of the starting gas station if it's possible to travel around the circuit without running out of gas at any station in a clockwise direction. If there is no such starting station exists, return -1.
Note: If a solution exists, it is guaranteed to be unique.

```cpp
class Solution {
  public:
    int startStation(vector<int> &gas, vector<int> &cost) {
        int curr=0,n=gas.size(),startidx=0;
        for(int i=0;i<n;i++){
            curr+=gas[i]-cost[i];
            if(curr<0)
            {
                startidx=i+1;
                curr=0;
            }
        }
        curr=0;
        for(int i=0;i<n;i++){
            int idx=(i+startidx)%n;
            curr+=gas[idx]-cost[idx];
            if(curr<0)
            return -1;
        }
        return startidx;
    }
};
```

### 5. Maximize partitions in a String
Given a string s of lowercase English alphabets, your task is to return the maximum number of substrings formed, after possible partitions (probably zero) of s such that no two substrings have a common character.

```cpp
class Solution {
  public:
    int maxPartitions(string &s) {
        vector<int>last(26,-1);
        for(int i=0;i<s.size();i++)
        last[s[i]-'a']=i;
        int end=0,cnt=0;
        for(int i=0;i<s.size();i++){
            end=max(end,last[s[i]-'a']);
            if(i==end)
            cnt++;
        }
        return cnt;
    }
};
```

### 6. Candy
There are n children standing in a line. Each child is assigned a rating value given in the integer array arr[]. You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating than their neighbors get more candies than their neighbors.
Return the minimum number of candies you need to have to distribute.

Note: The answer will always fit into a 32-bit integer.

```cpp
class Solution {
  public:
    int minCandy(vector<int> &arr) {
        vector<int>res(arr.size(),1);
        for(int i=1;i<arr.size();i++){
            if(arr[i]>arr[i-1])
            res[i]=res[i-1]+1;
        }
        for(int i=arr.size()-2;i>=0;i--){
            if(arr[i]>arr[i+1])
            res[i]=max(res[i],res[i+1]+1);
        }
        return accumulate(res.begin(),res.end(),0);
    }
};
```

### 7. Police and Thieves
Given an array arr[], where each element contains either a 'P' for policeman or a 'T' for thief. Find the maximum number of thieves that can be caught by the police. 
Keep in mind the following conditions :

Each policeman can catch only one thief.
A policeman cannot catch a thief who is more than k units away from him.

```cpp
class Solution {
  public:
    int catchThieves(vector<char> &arr, int k) {
        vector<int>thiefidx;
        for(int i=0;i<arr.size();i++){
            if(arr[i]=='T')
            thiefidx.push_back(i);
        }
        int j=0,cnt=0;
        for(int i=0;i<arr.size();i++){
            if(arr[i]=='P'){
                while(j<thiefidx.size() && thiefidx[j]<i-k)
                j++;
                if(j<thiefidx.size() && thiefidx[j]<=i+k)
                {
                    cnt++;
                    j++;
                }
            }
        }
        return cnt;
    }
};
```
