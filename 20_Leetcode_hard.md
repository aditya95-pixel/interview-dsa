### 1 The skyline problem

A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return the skyline formed by these buildings collectively.

The geometric information of each building is given in the array buildings where buildings[i] = [lefti, righti, heighti]:

lefti is the x coordinate of the left edge of the ith building.
righti is the x coordinate of the right edge of the ith building.
heighti is the height of the ith building.
You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

The skyline should be represented as a list of "key points" sorted by their x-coordinate in the form [[x1,y1],[x2,y2],...]. Each key point is the left endpoint of some horizontal segment in the skyline except the last point in the list, which always has a y-coordinate 0 and is used to mark the skyline's termination where the rightmost building ends. Any ground between the leftmost and rightmost buildings should be part of the skyline's contour.

Note: There must be no consecutive horizontal lines of equal height in the output skyline. For instance, [...,[2 3],[4 5],[7 5],[11 5],[12 7],...] is not acceptable; the three lines of height 5 should be merged into one in the final output as such: [...,[2 3],[4 5],[12 7],...]

```cpp
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        vector<vector<int>>edges,skyline;
        priority_queue<vector<int>>pq;
        for(int i=0;i<buildings.size();i++)
        {
            edges.push_back({buildings[i][0],i});
            edges.push_back({buildings[i][1],i});
        }
        sort(edges.begin(),edges.end());
        int edge_idx=0;
        while(edge_idx<edges.size()){
            int curr_height;
            int curr_x=edges[edge_idx][0];
            while(edge_idx<edges.size() && curr_x==edges[edge_idx][0]){
                int building_idx=edges[edge_idx][1];
                vector<int>building=buildings[building_idx];
                if(building[0]==curr_x){
                    pq.push({building[2],building[1]});
                }
                edge_idx++;
            }
            while(!pq.empty() && pq.top()[1]<=curr_x)
            pq.pop();
            if(pq.empty())
            curr_height=0;
            else
            curr_height=pq.top()[0];
            if(skyline.empty () || skyline.back()[1]!=curr_height)
            skyline.push_back({curr_x,curr_height});
        }
        return skyline;
    }
};
```

### 2 Sliding Window Maximum

You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int>res;
        multiset<int,greater<int>>s;
        for(int i=0;i<k;i++)
        s.insert(nums[i]);
        res.push_back(*s.begin());
        for(int i=k;i<nums.size();i++){
            s.erase(s.lower_bound(nums[i-k]));
            s.insert(nums[i]);
            res.push_back(*s.begin());
        }
        return res;
    }
};
```

### 3 Find Median from data stream

The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

For example, for arr = [2,3,4], the median is 3.
For example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.
Implement the MedianFinder class:

MedianFinder() initializes the MedianFinder object.
void addNum(int num) adds the integer num from the data stream to the data structure.
double findMedian() returns the median of all elements so far. Answers within 10-5 of the actual answer will be accepted.

```cpp
class MedianFinder {
    priority_queue<int>maxq;
    priority_queue<int,vector<int>,greater<int>>minq;
public:
    MedianFinder() {
        
    }
    
    void addNum(int num) {
        maxq.push(num);
        int maxo=maxq.top();
        maxq.pop();
        minq.push(maxo);
        if(minq.size()>maxq.size()){
            int mino=minq.top();
            minq.pop();
            maxq.push(mino);
        }
    }
    
    double findMedian() {
        if(maxq.size()==minq.size()){
            return (maxq.top()+minq.top())/2.0;
        }
        else{
            return maxq.top();
        }
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

### 4 Count of Range Sum

Given an integer array nums and two integers lower and upper, return the number of range sums that lie in [lower, upper] inclusive.

Range sum S(i, j) is defined as the sum of the elements in nums between indices i and j inclusive, where i <= j.

```cpp
class Solution {
public:
    int merge(vector<long long>&prefix_sum,int l,int mid,int r,int lower,int upper){
        int cnt=0;
        int j1=mid+1,j2=mid+1;
        for(int i=l;i<=mid;i++){
            while(j1<=r && prefix_sum[j1]-prefix_sum[i]<lower)j1++;
            while(j2<=r && prefix_sum[j2]-prefix_sum[i]<=upper)j2++;
            cnt+=(j2-j1);
        }
        int n1=mid-l+1,n2=r-mid;
        vector<long long>nums1(n1),nums2(n2);
        for(int i=0;i<n1;i++)
        nums1[i]=prefix_sum[l+i];
        for(int i=0;i<n2;i++)
        nums2[i]=prefix_sum[mid+i+1];
        int k=l,i=0,j=0;
        while(i<nums1.size() && j<nums2.size()){
            if(nums1[i]<nums2[j])
            prefix_sum[k++]=nums1[i++];
            else
            prefix_sum[k++]=nums2[j++];
        }
        while(i<nums1.size())
        prefix_sum[k++]=nums1[i++];
        while(j<nums2.size())
        prefix_sum[k++]=nums2[j++];
        return cnt;
    }
    int mergesort(vector<long long>&prefix_sum,int l,int r,int lower,int upper){
        int res=0;
        if(l<r){
            int mid=l+(r-l)/2;
            res+=mergesort(prefix_sum,l,mid,lower,upper);
            res+=mergesort(prefix_sum,mid+1,r,lower,upper);
            res+=merge(prefix_sum,l,mid,r,lower,upper);
        }
        return res;
    }
    int countRangeSum(vector<int>& nums, int lower, int upper) {
        vector<long long>prefix_sum(nums.size()+1,0);
        for(int i=0;i<nums.size();i++)
        prefix_sum[i+1]=prefix_sum[i]+nums[i];
        return mergesort(prefix_sum,0,nums.size(),lower,upper);
    }
};
```
