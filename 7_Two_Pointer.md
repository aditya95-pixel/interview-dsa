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
