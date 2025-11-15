# Segment Tree

### 303. Range Sum Query - Immutable

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
