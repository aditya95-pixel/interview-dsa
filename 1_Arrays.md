## Array

### 1. Second Largest
Given an array of positive integers arr[], return the second largest element from the array. If the second largest element doesn't exist then return -1.

Note: The second largest element should not be equal to the largest element.

```cpp
class Solution {
  public:
    int getSecondLargest(vector<int> &arr) {
        int largest=-1,second_largest=-1;
        for(auto num:arr){
            if(num>largest){
                second_largest=largest;
                largest=num;
            }
            else if(num<largest && num>second_largest)
            second_largest=num;
        }
        return second_largest;
    }
};
```

### 2. Move All Zeroes to End
You are given an array arr[] of non-negative integers. Your task is to move all the zeros in the array to the right end while maintaining the relative order of the non-zero elements. The operation must be performed in place, meaning you should not use extra space for another array.

```cpp
class Solution {
  public:
    void pushZerosToEnd(vector<int>& arr) {
        int k=0;
        for(int i=0;i<arr.size();i++){
            if(arr[i]!=0)
            {
                swap(arr[k],arr[i]);
                k++;
            }
        }
    }
};
```

### 3. Reverse an Array
You are given an array of integers arr[]. Your task is to reverse the given array.

Note: Modify the array in place.

```cpp
class Solution {
  public:
    void reverseArray(vector<int> &arr) {
        for(int i=0;i<arr.size()/2;i++)
        swap(arr[i],arr[arr.size()-i-1]);
    }
};
```

### 4. Rotate Array
Given an array arr[]. Rotate the array to the left (counter-clockwise direction) by d steps, where d is a positive integer. Do the mentioned change in the array in place.

Note: Consider the array as circular.

```cpp
class Solution {
  public:
    void rotateArr(vector<int>& arr, int d) {
        d%=arr.size();
        reverse(arr.begin(),arr.begin()+d);
        reverse(arr.begin()+d,arr.end());
        reverse(arr.begin(),arr.end());
    }
};
```

### 5. Next Permutation
Given an array of integers arr[] representing a permutation, implement the next permutation that rearranges the numbers into the lexicographically next greater permutation. If no such permutation exists, rearrange the numbers into the lowest possible order (i.e., sorted in ascending order). 

Note - A permutation of an array of integers refers to a specific arrangement of its elements in a sequence or linear order.

```cpp
class Solution {
  public:
    void nextPermutation(vector<int>& arr) {
        int pivot=-1;
        for(int i=arr.size()-2;i>=0;i--){
            if(arr[i]<arr[i+1])
            {
                pivot=i;
                break;
            }
        }
        if(pivot==-1)
        {
            reverse(arr.begin(),arr.end());
            return ;
        }
        for(int i=arr.size()-1;i>=0;i--){
            if(arr[i]>arr[pivot])
            {
                swap(arr[i],arr[pivot]);
                break;
            }
        }
        reverse(arr.begin()+pivot+1,arr.end());
    }
};
```

### 6. Majority Element II
You are given an array of integer arr[] where each number represents a vote to a candidate. Return the candidates that have votes greater than one-third of the total votes, If there's not a majority vote, return an empty array. 

Note: The answer should be returned in an increasing format.

```cpp
class Solution {
  public:
    vector<int> findMajority(vector<int>& arr) {
        int val1=-1,val2=-1,cnt1=0,cnt2=0;
        for(auto val:arr){
            if(val==val1)
            cnt1++;
            else if(val==val2)
            cnt2++;
            else if(cnt1==0)
            {
                val1=val;
                cnt1++;
            }
            else if(cnt2==0)
            {
                val2=val;
                cnt2++;
            }
            else
            {
                cnt1--;
                cnt2--;
            }
        }
        vector<int>res;
        cnt1=cnt2=0;
        for(auto val:arr){
            if(val==val1)
            cnt1++;
            else if(val==val2)
            cnt2++;
        }
        if(cnt1>arr.size()/3)
        res.push_back(val1);
        if(cnt2>arr.size()/3)
        res.push_back(val2);
        if(res.size()==2 && res[0]>res[1])
        swap(res[0],res[1]);
        return res;
    }
};
```

### 7. Stock Buy and Sell – Multiple Transaction Allowed
The cost of stock on each day is given in an array price[]. Each day you may decide to either buy or sell the stock i at price[i], you can even buy and sell the stock on the same day. Find the maximum profit that you can get.

Note: A stock can only be sold if it has been bought previously and multiple stocks cannot be held on any given day.

```cpp
class Solution {
  public:
    int maximumProfit(vector<int> &prices) {
        int maxprofit=0;
        for(int i=0;i<prices.size()-1;i++){
            if(prices[i]<prices[i+1])
            maxprofit+=(prices[i+1]-prices[i]);
        }
        return maxprofit;
    }
};
```

### 8. Stock Buy and Sell – Max one Transaction Allowed
Given an array prices[] of length n, representing the prices of the stocks on different days. The task is to find the maximum profit possible by buying and selling the stocks on different days when at most one transaction is allowed. Here one transaction means 1 buy + 1 Sell. If it is not possible to make a profit then return 0.

Note: Stock must be bought before being sold.

```cpp
class Solution {
  public:
    int maximumProfit(vector<int> &prices) {
        int maxprofit=0,min_val=prices[0];
        for(int i=1;i<prices.size();i++){
            min_val=min(min_val,prices[i]);
            maxprofit=max(maxprofit,prices[i]-min_val);
        }
        return maxprofit;
    }
};
```

### 9. Minimize the Heights II
Given an array arr[] denoting heights of N towers and a positive integer K.

For each tower, you must perform exactly one of the following operations exactly once.

Increase the height of the tower by K
Decrease the height of the tower by K
Find out the minimum possible difference between the height of the shortest and tallest towers after you have modified each tower.

Note: It is compulsory to increase or decrease the height by K for each tower. After the operation, the resultant array should not contain any negative integers.

```cpp
class Solution {
  public:
    int getMinDiff(vector<int> &arr, int k) {
        sort(arr.begin(),arr.end());
        int mindiff=arr[arr.size()-1]-arr[0];
        for(int i=1;i<arr.size();i++){
            if(arr[i]-k<0)
            continue;
            int maxh=max(arr[i-1]+k,arr[arr.size()-1]-k);
            int minh=min(arr[0]+k,arr[i]-k);
            mindiff=min(mindiff,maxh-minh);
        }
        return mindiff;
    }
};
```

### 10. Kadane's Algorithm
Given an integer array arr[]. You need to find the maximum sum of a subarray.

```cpp
class Solution {
  public:
    int maxSubarraySum(vector<int> &arr) {
        int sum=arr[0],maxsum=arr[0];
        for(int i=1;i<arr.size();i++){
            sum=max(arr[i],sum+arr[i]);
            maxsum=max(maxsum,sum);
        }
        return maxsum;
    }
};
```

### 11. Maximum Product Subarray
Given an array arr[] that contains positive and negative integers (may contain 0 as well). Find the maximum product that we can get in a subarray of arr[].

Note: It is guaranteed that the output fits in a 32-bit integer.

```cpp
class Solution {
  public:
    int maxProduct(vector<int> &arr) {
        int leftRight=1,rightLeft=1,maxprod=INT_MIN;
        for(int i=0;i<arr.size();i++){
            if(leftRight==0)
            leftRight=1;
            if(rightLeft==0)
            rightLeft=1;
            leftRight*=arr[i];
            rightLeft*=arr[arr.size()-i-1];
            maxprod=max({leftRight,rightLeft,maxprod});
        }
        return maxprod;
    }
};
```

### 12. Max Circular Subarray Sum
Given an array of integers arr[] in a circular fashion. Find the maximum subarray sum that we can get if we assume the array to be circular.

```cpp
class Solution {
  public:
    int circularSubarraySum(vector<int> &arr) {
        int sum1=arr[0],sum2=arr[0],maxsum=arr[0],minsum=arr[0],totalsum=arr[0];
        for(int i=1;i<arr.size();i++){
            sum1=max(sum1+arr[i],arr[i]);
            maxsum=max(sum1,maxsum);
            sum2=min(sum2+arr[i],arr[i]);
            minsum=min(sum2,minsum);
            totalsum+=arr[i];
        }
        int circsum=totalsum-minsum;
        if(minsum==totalsum)
        return maxsum;
        return max(maxsum,circsum);
    }
};
```

### 13. Smallest Positive Missing
You are given an integer array arr[]. Your task is to find the smallest positive number missing from the array.

Note: Positive number starts from 1. The array can have negative integers too.

```cpp
class Solution {
  public:
    int missingNumber(vector<int> &arr) {
        for(int i=0;i<arr.size();i++){
            while(arr[i]>=1 && arr[i]<=arr.size() && arr[i]!=arr[arr[i]-1])
            swap(arr[i],arr[arr[i]-1]);
        }
        for(int i=1;i<=arr.size();i++){
            if(arr[i-1]!=i)
            return i;
        }
        return arr.size()+1;
    }
};
```
### 14. Split array in three equal sum subarrays
Given an array, arr[], determine if arr can be split into three consecutive parts such that the sum of each part is equal. If possible, return any index pair(i, j) in an array such that sum(arr[0..i]) = sum(arr[i+1..j]) = sum(arr[j+1..n-1]), otherwise return an array {-1,-1}.

Note: Since multiple answers are possible, return any of them. The driver code will print true if it is correct otherwise, it will print false.

```cpp
class Solution {
  public:
    vector<int> findSplit(vector<int>& arr) {
        int sum=accumulate(arr.begin(),arr.end(),0);
        vector<int>res;
        res.push_back(-1);
        res.push_back(-1);
        int tempsum=0;
        for(int i=0;i<arr.size();i++){
            if(tempsum==sum/3 && res[0]==-1)
            {
                tempsum=0;
                res[0]=i-1;
            }
            else if(tempsum==sum/3 && res[1]==-1)
            {
                res[1]=i-1;
            }
            tempsum+=arr[i];
        }
        if(res[1]==-1)
        res[0]=-1;
        return res;
    }
};
```
