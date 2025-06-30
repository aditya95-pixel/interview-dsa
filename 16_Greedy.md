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
