# Segment Tree

### Range Sum Query - Immutable

Given an integer array nums, handle multiple queries of the following type:

Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
Implement the NumArray class:

NumArray(int[] nums) Initializes the object with the integer array nums.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).

```cpp
class SegmentTree{
    vector<int>seg;
    vector<int>arr;
    int n;
    public:
    SegmentTree(vector<int>nums){
        n=nums.size();
        seg.resize(4*n);
        arr=nums;
        build(0,0,n-1);
    }
    void build(int idx,int l,int r){
        if(l==r)
        {
            seg[idx]=arr[l];
            return ;
        }
        int mid=l+(r-l)/2;
        build(2*idx+1,l,mid);
        build(2*idx+2,mid+1,r);
        seg[idx]=seg[2*idx+1]+seg[2*idx+2];
    }
    int helper(int idx,int low,int high,int l,int r){
        if(low>=l && high<=r)
        return seg[idx];
        else if(low>r || high<l)
        return 0;
        int mid=low+(high-low)/2;
        int lsum=helper(2*idx+1,low,mid,l,r);
        int rsum=helper(2*idx+2,mid+1,high,l,r);
        return lsum+rsum;
    }
    int query(int l,int r){
        return helper(0,0,n-1,l,r);
    }
};
class NumArray {
    SegmentTree *t;
public:
    NumArray(vector<int>& nums) {
        t=new SegmentTree(nums);
    }
    
    int sumRange(int left, int right) {
        return t->query(left,right);
    }
};
```

### 2 Range Min Max Queries
You are given an array of integers of size N and Q queries. 

Your task is to complete the following functions:

getMinMax(L,R): return the minimum and maximum in the given range [L,R]
updateValue(index,value): update arr[index] to value.

Note: 0-based indexing is used.

```cpp
class SegmentTree{
    vector<pair<int,int>>seg;
    vector<int>arr;
    int n;
    public:
    SegmentTree(vector<int>nums){
        n=nums.size();
        seg.resize(4*n);
        arr=nums;
        build(0,0,n-1);
    }
    void build(int idx,int l,int r){
        if(l==r)
        {
            seg[idx].first=seg[idx].second=arr[l];
            return ;
        }
        int mid=l+(r-l)/2;
        build(2*idx+1,l,mid);
        build(2*idx+2,mid+1,r);
        seg[idx].first=max(seg[2*idx+1].first,seg[2*idx+2].first);
        seg[idx].second=min(seg[2*idx+1].second,seg[2*idx+2].second);
    }
    int minhelper(int idx,int low,int high,int l,int r){
        if(low>=l && high<=r)
        return seg[idx].second;
        else if(low>r || high<l)
        return INT_MAX;
        int mid=low+(high-low)/2;
        int lmin=minhelper(2*idx+1,low,mid,l,r);
        int rmin=minhelper(2*idx+2,mid+1,high,l,r);
        return min(lmin,rmin);
    }
    int minquery(int l,int r){
        return minhelper(0,0,n-1,l,r);
    }
    int maxhelper(int idx,int low,int high,int l,int r){
        if(low>=l && high<=r)
        return seg[idx].first;
        else if(low>r || high<l)
        return INT_MIN;
        int mid=low+(high-low)/2;
        int lmax=maxhelper(2*idx+1,low,mid,l,r);
        int rmax=maxhelper(2*idx+2,mid+1,high,l,r);
        return max(lmax,rmax);
    }
    int maxquery(int l,int r){
        return maxhelper(0,0,n-1,l,r);
    }
    void update_helper(int idx,int i,int val,int l,int r){
        if(l==r && l==i)
        {
            seg[idx].first=seg[idx].second=arr[l]=val;
            return ;
        }
        int mid=l+(r-l)/2;
        if(i>=l && i<=mid)
        {
            update_helper(2*idx+1,i,val,l,mid);
            seg[idx].first=max(seg[2*idx+1].first,seg[2*idx+2].first);
            seg[idx].second=min(seg[2*idx+1].second,seg[2*idx+2].second);
        }
        else if(i<=r && i>=mid+1)
        {
            update_helper(2*idx+2,i,val,mid+1,r);
            seg[idx].first=max(seg[2*idx+1].first,seg[2*idx+2].first);
            seg[idx].second=min(seg[2*idx+1].second,seg[2*idx+2].second);
        }
    }
    void update(int i,int val){
        update_helper(0,i,val,0,n-1);
    }
};
class Solution {
    SegmentTree *t=NULL;
  public:
    vector<int> getMinMax(vector<int>& arr, int L, int R,
                          vector<vector<int>>& segTree) {
        if(t==NULL)
        t=new SegmentTree(arr);
        int maxo=t->maxquery(L,R);
        int mino=t->minquery(L,R);
        vector<int>res;
        res.push_back(mino);
        res.push_back(maxo);
        return res;
    }
    void updateValue(vector<int>& arr, int index, int value,
                     vector<vector<int>>& segTree) {
        if(t==NULL)
        t=new SegmentTree(arr);
        t->update(index,value);
    }
};
```

### 3 Range Sum Query - Mutable

Given an integer array nums, handle multiple queries of the following types:

Update the value of an element in nums.
Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
Implement the NumArray class:

NumArray(int[] nums) Initializes the object with the integer array nums.
void update(int index, int val) Updates the value of nums[index] to be val.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).

```cpp
class SegmentTree{
    vector<int>seg;
    vector<int>arr;
    int n;
    public:
    SegmentTree(vector<int>&nums){
        arr=nums;
        n=arr.size();
        seg.resize(4*n);
        build(0,0,n-1);
    }
    void build(int idx,int l,int r){
        if(l==r)
        {
            seg[idx]=arr[l];
            return ;
        }
        int mid=l+(r-l)/2;
        build(2*idx+1,l,mid);
        build(2*idx+2,mid+1,r);
        seg[idx]=seg[2*idx+1]+seg[2*idx+2];
    }
    int query_helper(int idx,int low,int high,int l,int r){
        if(low>=l && high<=r)
        return seg[idx];
        else if(high<l || low>r)
        return 0;
        int mid=low+(high-low)/2;
        int lsum=query_helper(2*idx+1,low,mid,l,r);
        int rsum=query_helper(2*idx+2,mid+1,high,l,r);
        return lsum+rsum;
    }
    int query(int l,int r){
        return query_helper(0,0,n-1,l,r);
    }
    void update_helper(int idx,int i,int val,int l,int r){
        if(l==r && l==i){
            seg[idx]=arr[i]=val;
            return ;
        }
        int mid=l+(r-l)/2;
        if(i>=l && i<=mid)
            update_helper(2*idx+1,i,val,l,mid);    
        else if(i<=r && i>=mid+1)
            update_helper(2*idx+2,i,val,mid+1,r);
        seg[idx]=seg[2*idx+1]+seg[2*idx+2];
    }
    void update(int index,int val){
        return update_helper(0,index,val,0,n-1);
    }
};
class NumArray {
    SegmentTree *t;
public:
    NumArray(vector<int>& nums) {
        t=new SegmentTree(nums);
    }
    
    void update(int index, int val) {
        t->update(index,val);
    }
    
    int sumRange(int left, int right) {
        return t->query(left,right);
    }
};
```

### 4. Count Stable Subarrays

You are given an integer array nums.

A subarray of nums is called stable if it contains no inversions, i.e., there is no pair of indices i < j such that nums[i] > nums[j].

You are also given a 2D integer array queries of length q, where each queries[i] = [li, ri] represents a query. For each query [li, ri], compute the number of stable subarrays that lie entirely within the segment nums[li..ri].

Return an integer array ans of length q, where ans[i] is the answer to the ith query.​​​​​​​​​​​​​​

Note:

A single element subarray is considered stable.

```cpp
class Node{
    public:
        long long pref,suff,rightVal,leftVal,len,ans;
    Node(){
        pref=suff=leftVal=rightVal=len=ans=0;
    }  
};
Node merge(Node a,Node b){
        Node c;
        c.len=a.len+b.len;
        c.leftVal=a.leftVal;
        c.rightVal=b.rightVal;
        c.pref=a.pref;
        if(a.pref==a.len && a.rightVal<=b.leftVal)
        c.pref=a.len+b.pref;
        c.suff=b.suff;
        if(b.suff==b.len && a.rightVal<=b.leftVal)
        c.suff=b.len+a.suff;
        c.ans=a.ans+b.ans;
        if(a.rightVal<=b.leftVal)
        c.ans+=1LL*a.suff*b.pref;
        return c;
}
class SegmentTree{
    vector<Node>seg;
    vector<int>nums;
    int n;
    public:
    SegmentTree(vector<int>&nums){
        this->nums=nums;
        n=nums.size();
        seg.resize(4*n);
        build(0,0,n-1);
    }
    Node build(int idx,int l,int r){
        if(l==r){
            seg[idx].rightVal=seg[idx].leftVal=nums[l];
            seg[idx].pref=seg[idx].suff=seg[idx].len=seg[idx].ans=1;
            return seg[idx];
        }
        int mid=l+(r-l)/2;
        return seg[idx]=merge(build(2*idx+1,l,mid),build(2*idx+2,mid+1,r));
    }
    Node query_helper(int idx,int low,int high,int l,int r){
        if(low>=l && high<=r)
        return seg[idx];
        else if(low>r || high<l)
        return Node();
        else
        {
            int mid=low+(high-low)/2;
            return merge(query_helper(2*idx+1,low,mid,l,r),query_helper(2*idx+2,mid+1,high,l,r));
        }
    }
    long long query(int l,int r){
        return query_helper(0,0,n-1,l,r).ans;
    }
};
class Solution {
public:
    vector<long long> countStableSubarrays(vector<int>& nums, vector<vector<int>>& queries) {
        vector<long long>res;
        SegmentTree *t=new SegmentTree(nums);
        for(auto &q:queries)
            res.push_back(t->query(q[0],q[1]));
        return res;
    }
};
```
