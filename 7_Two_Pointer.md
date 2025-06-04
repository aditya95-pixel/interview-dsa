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
