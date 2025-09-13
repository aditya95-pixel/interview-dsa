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

### 5 Find number of ways to place people II

You are given a 2D array points of size n x 2 representing integer coordinates of some points on a 2D-plane, where points[i] = [xi, yi].

We define the right direction as positive x-axis (increasing x-coordinate) and the left direction as negative x-axis (decreasing x-coordinate). Similarly, we define the up direction as positive y-axis (increasing y-coordinate) and the down direction as negative y-axis (decreasing y-coordinate)

You have to place n people, including Alice and Bob, at these points such that there is exactly one person at every point. Alice wants to be alone with Bob, so Alice will build a rectangular fence with Alice's position as the upper left corner and Bob's position as the lower right corner of the fence (Note that the fence might not enclose any area, i.e. it can be a line). If any person other than Alice and Bob is either inside the fence or on the fence, Alice will be sad.

Return the number of pairs of points where you can place Alice and Bob, such that Alice does not become sad on building the fence.

Note that Alice can only build a fence with Alice's position as the upper left corner, and Bob's position as the lower right corner. For example, Alice cannot build either of the fences in the picture below with four corners (1, 1), (1, 3), (3, 1), and (3, 3), because:

With Alice at (3, 3) and Bob at (1, 1), Alice's position is not the upper left corner and Bob's position is not the lower right corner of the fence.
With Alice at (1, 3) and Bob at (1, 1), Bob's position is not the lower right corner of the fence.

```cpp
class Solution {
public:
    static bool compare(vector<int>&a,vector<int>&b){
        if(a[0]<b[0])
        return true;
        else if(a[0]>b[0])
        return false;
        else
        return a[1]>b[1];
    }
    int numberOfPairs(vector<vector<int>>& points) {
        sort(points.begin(),points.end(),compare);
        int cnt=0;
        for(int i=0;i<points.size();i++){
            int top=points[i][1];
            int bot=INT_MIN;
            for(int j=i+1;j<points.size();j++){
                if(bot<points[j][1] && points[j][1]<=top){
                    cnt++;
                    bot=points[j][1];
                    if(bot==top)
                    break;
                }
            }
        }
        return cnt;
    }
};
```

### 6 Patching Array

Given a sorted integer array nums and an integer n, add/patch elements to the array such that any number in the range [1, n] inclusive can be formed by the sum of some elements in the array.

Return the minimum number of patches required.

```cpp
class Solution {
public:
    int minPatches(vector<int>& nums, int n) {
        long long miss=1,cnt=0,i=0;
        while(miss<=n){
            if(i<nums.size() && nums[i]<=miss)
            miss+=nums[i++];
            else{
                miss+=miss;
                cnt++;
            }
        }
        return (int)cnt;
    }
};
```

### 7 Couples Holding Hands

There are n couples sitting in 2n seats arranged in a row and want to hold hands.

The people and seats are represented by an integer array row where row[i] is the ID of the person sitting in the ith seat. The couples are numbered in order, the first couple being (0, 1), the second couple being (2, 3), and so on with the last couple being (2n - 2, 2n - 1).

Return the minimum number of swaps so that every couple is sitting side by side. A swap consists of choosing any two people, then they stand up and switch seats.

```cpp
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        vector<int>pos(row.size(),0);
        for(int i=0;i<row.size();i++)
        pos[row[i]]=i;
        int cnt=0;
        for(int i=0;i<row.size();i+=2){
            int first=row[i];
            int second=row[i+1];
            int expected_second=first^1;
            if(second!=expected_second){
                int expected_second_pos=pos[expected_second];
                swap(row[i+1],row[expected_second_pos]);
                cnt++;
                pos[expected_second]=i+1;
                pos[second]=expected_second_pos;
            }
        }
        return cnt;
    }
};
```

### 8 Minimum Window Substring

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        if(t.size()>s.size())
        return "";
        map<char,int>mp1,mp2;
        for(auto c:t)
        mp1[c]++;
        int start=0,end=0;
        int minlen=INT_MAX;
        int begin=-1,netcnt=0;
        while(end<s.size()){
           mp2[s[end]]++;
           if(mp1.find(s[end])!=mp1.end() && mp1[s[end]]==mp2[s[end]])
                netcnt++;
            while(netcnt==mp1.size()){
                if(minlen>end-start+1)
                {minlen=end-start+1;begin=start;}
                if(mp1.find(s[start])!=mp1.end() && mp1[s[start]]==mp2[s[start]])
                    netcnt--;
                mp2[s[start]]--;
                start++;
            }
            end++;
        }
        if(begin!=-1)
        return s.substr(begin,minlen);
        else
        return "";
    }
};
```

### 9 Subarrays with k different integers

Given an integer array nums and an integer k, return the number of good subarrays of nums.

A good array is an array where the number of different integers in that array is exactly k.

For example, [1,2,3,1,2] has 3 different integers: 1, 2, and 3.
A subarray is a contiguous part of an array.

```cpp
class Solution {
public:
    int solve(vector<int>& nums,int k){
        map<int,int>mp;
        int end=0,start=0,cnt=0;
        while(end<nums.size()){
            mp[nums[end]]++;
            while(mp.size()>k){
                if(mp[nums[start]]==1)
                    mp.erase(nums[start]);
                else
                    mp[nums[start]]--;
                start++;
            }
            cnt+=end-start+1;
            end++;
        }
        return cnt;
    }
    int subarraysWithKDistinct(vector<int>& nums, int k) {
        return solve(nums,k)-solve(nums,k-1);
    }
};
```

### 10 Maximum Number of robots within budget

You have n robots. You are given two 0-indexed integer arrays, chargeTimes and runningCosts, both of length n. The ith robot costs chargeTimes[i] units to charge and costs runningCosts[i] units to run. You are also given an integer budget.

The total cost of running k chosen robots is equal to max(chargeTimes) + k * sum(runningCosts), where max(chargeTimes) is the largest charge cost among the k robots and sum(runningCosts) is the sum of running costs among the k robots.

Return the maximum number of consecutive robots you can run such that the total cost does not exceed budget.

```cpp
class Solution {
public:
    int maximumRobots(vector<int>& chargeTimes, vector<int>& runningCosts, long long budget) {
        int i=0;
        long long sum=0;
        multiset<int,greater<int>>s;
        for(int j=0;j<chargeTimes.size();j++){
            sum+=runningCosts[j];
            s.insert(chargeTimes[j]);
            if(*s.begin()+sum*(j-i+1)>budget){
                sum-=runningCosts[i];
                s.erase(s.lower_bound(chargeTimes[i++]));
            }
        }
        return chargeTimes.size()-i;
    }
};
```

### 11 Longest Valid Parentheses

Given a string containing just the characters '(' and ')', return the length of the longest valid (well-formed) parentheses substring.

```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int>stk;
        stk.push(-1);
        int maxlen=0;
        for(int i=0;i<s.size();i++){
            if(s[i]=='(')
            stk.push(i);
            else{
                stk.pop();
                if(stk.empty())
                stk.push(i);
                else
                maxlen=max(maxlen,i-stk.top());
            }
        }
        return maxlen;
    }
};
```

### 12 Minimum operations to make array elements 0

You are given a 2D array queries, where queries[i] is of the form [l, r]. Each queries[i] defines an array of integers nums consisting of elements ranging from l to r, both inclusive.

In one operation, you can:

Select two integers a and b from the array.
Replace them with floor(a / 4) and floor(b / 4).
Your task is to determine the minimum number of operations required to reduce all elements of the array to zero for each query. Return the sum of the results for all queries.

```cpp
class Solution {
public:
    long long minOperations(vector<vector<int>>& queries) {
        long long ans=0;
        for(auto query:queries){
            long long a=query[0],b=query[1],k1=query[0],k2=query[1],temp=0,l=0,r=0;
            while(k1!=0){
                k1/=4;
                l++;
            }
            while(k2!=0){
                k2/=4;
                r++;
            }
            long long c=b-a+1,d=(long long)pow(4,l)-a;
            temp+=min(c,d)*l;
            if(r!=l){
                temp+=(b-(long long)pow(4,r-1)+1)*r;
            }
            if(r-l>1){
                for(long long i=l+1;i<r;i++)
                temp+=((long long)pow(4,i)-(long long)pow(4,i-1))*i;
            }
            if(temp%2!=0)
            ans++;
            ans+=temp/2;
        }
        return ans;
    }
};
```

### 13 Valid Number

Given a string s, return whether s is a valid number.

For example, all the following are valid numbers: "2", "0089", "-0.1", "+3.14", "4.", "-.9", "2e10", "-90E3", "3e+7", "+6e-1", "53.5e93", "-123.456e789", while the following are not valid numbers: "abc", "1a", "1e", "e3", "99e2.5", "--6", "-+3", "95a54e53".

Formally, a valid number is defined using one of the following definitions:

An integer number followed by an optional exponent.
A decimal number followed by an optional exponent.
An integer number is defined with an optional sign '-' or '+' followed by digits.

A decimal number is defined with an optional sign '-' or '+' followed by one of the following definitions:

Digits followed by a dot '.'.
Digits followed by a dot '.' followed by digits.
A dot '.' followed by digits.
An exponent is defined with an exponent notation 'e' or 'E' followed by an integer number.

The digits are defined as one or more digits.

```cpp
class Solution {
public:
    bool isNumber(string s) {
        int cntE=0,cntAE=0,cntD=0,cntN=0;
        for(int i=0;i<s.size();i++){
            if(s[i]=='.' && cntD>0)
            return false;
            else if((s[i]=='E' || s[i]=='e') && (i==0 || i==s.size()-1))
            return false;
            else if(isalpha(s[i]) && (s[i]!='e' && s[i]!='E'))
            return false;
            else if((s[i]=='e' || s[i]=='E') && cntE>0)
            return false;
            else if(s[i]=='.' && cntE>0)
            return false;
            else if((s[i]=='-' || s[i]=='+') && i==s.size()-1)
            return false;
            else if((s[i]=='+' || s[i]=='-') && i!=0 && (s[i-1]!='e' && s[i-1]!='E'))
            return false;
            else if(isdigit(s[i]) && cntE>0)
            cntAE++;
            else if(isdigit(s[i]) && cntE==0)
            cntN++;
            else if(s[i]=='.')
            cntD++;
            else if(s[i]=='e' || s[i]=='E')
            cntE++;
        }
        if(cntN>0)
        return true;
        else
        return false;
    }
};
```

### 14 Number of people aware of a secret

On day 1, one person discovers a secret.

You are given an integer delay, which means that each person will share the secret with a new person every day, starting from delay days after discovering the secret. You are also given an integer forget, which means that each person will forget the secret forget days after discovering it. A person cannot share the secret on the same day they forgot it, or on any day afterwards.

Given an integer n, return the number of people who know the secret at the end of day n. Since the answer may be very large, return it modulo 109 + 7.

```cpp
#define MOD 1000000007
class Solution {
public:
    int peopleAwareOfSecret(int n, int delay, int forget) {
        if(n==1)
        return 1;
        vector<long long>dp(n+1);
        dp[1]=1;
        int window=0;
        for(int i=2;i<=n;i++){
            int entry=i-delay;
            int exit=i-forget;
            if(entry>=1)
            window=(window+dp[entry])%MOD;
            if(exit>=1)
            window=(window-dp[exit]+MOD)%MOD;
            dp[i]=window;
        }
        int start=max(1,n-forget+1);
        int res=0;
        for(int i=start;i<=n;i++)
        res=(res+dp[i])%MOD;
        return (int)res;
    }
};
```

### 15 Minimum Number of people to teach

On a social network consisting of m users and some friendships between users, two users can communicate with each other if they know a common language.

You are given an integer n, an array languages, and an array friendships where:

There are n languages numbered 1 through n,
languages[i] is the set of languages the i​​​​​​th​​​​ user knows, and
friendships[i] = [u​​​​​​i​​​, v​​​​​​i] denotes a friendship between the users u​​​​​​​​​​​i​​​​​ and vi.
You can choose one language and teach it to some users so that all friends can communicate with each other. Return the minimum number of users you need to teach.

Note that friendships are not transitive, meaning if x is a friend of y and y is a friend of z, this doesn't guarantee that x is a friend of z.

```cpp
class Solution {
public:
    int minimumTeachings(int n, vector<vector<int>>& languages, vector<vector<int>>& friendships) {
        set<int>incompat;
        for(auto arr:friendships){
            int u=arr[0]-1,v=arr[1]-1;
            bool chk=false;
            for(int i=0;i<languages[u].size();i++){
                for(int j=0;j<languages[v].size();j++){
                    if(languages[u][i]==languages[v][j])
                    {   
                        chk=true;
                        break;
                    }
                }
            }
            if(!chk)
            {
                incompat.insert(u);
                incompat.insert(v);
            }
        }
        int mino=INT_MAX;
        for(int i=1;i<=n;i++){
            int cnt=0;
            for(auto frnd:incompat){
                bool chk=false;
                for(int j=0;j<languages[frnd].size();j++){
                    if(languages[frnd][j]==i){
                        chk=true;
                        break;
                    }
                }
                if(!chk)
                cnt++;
            }
            mino=min(mino,cnt);
        }
        return mino;
    }
};
```

### 16 Palindrome Pairs

You are given a 0-indexed array of unique strings words.

A palindrome pair is a pair of integers (i, j) such that:

0 <= i, j < words.length,
i != j, and
words[i] + words[j] (the concatenation of the two strings) is a palindrome.
Return an array of all the palindrome pairs of words.

You must write an algorithm with O(sum of words[i].length) runtime complexity.

```cpp
class Trie{
    public:
    class node{
        public:
            bool end;
            int index;
            vector<int>palin_suff_idxs;
            node*next[26];
            node(){
                end=false;
                index=-1;
                for(int i=0;i<26;i++)
                next[i]=NULL;
            }
    };
    node* trie;
    Trie(){
        trie=new node();
    }
    bool ispalin(const string &s,int l,int r){
        while(l<r){
            if(s[l++]!=s[r--])return false;
        }
        return true;
    }
    void insert(string &word,int idx){
        node*it=trie;
        for(int i=0;i<word.size();i++){
            if(ispalin(word,i,word.size()-1))
            it->palin_suff_idxs.push_back(idx);
            int c=word[i]-'a';
            if(!it->next[c])it->next[c]=new node();
            it=it->next[c];
        }
        it->end=true;
        it->index=idx;
        it->palin_suff_idxs.push_back(idx);
    }
    void search(string &word,int idx,vector<vector<int>>&res){
        node*it=trie;
        for(int i=0;i<word.size();i++){
            if(it->end && it->index!=idx && ispalin(word,i,word.size()-1))
            res.push_back({idx,it->index});
            int c=word[i]-'a';
            if(!it->next[c])return;
            it=it->next[c];
        }
        for(auto j:it->palin_suff_idxs){
            if(j!=idx)
            res.push_back({idx,j});
        }
    }
};
class Solution {
public:
    vector<vector<int>> palindromePairs(vector<string>& words) {
        Trie t;
        for(int i=0;i<words.size();i++){
            string rev=words[i];
            reverse(rev.begin(),rev.end());
            t.insert(rev,i);
        }
        vector<vector<int>>res;
        for(int i=0;i<words.size();i++)
            t.search(words[i],i,res);
        return res;
    }
};
```

### 17 Text Justification

Given an array of strings words and a width maxWidth, format the text such that each line has exactly maxWidth characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly maxWidth characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

Note:

A word is defined as a character sequence consisting of non-space characters only.
Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
The input array words contains at least one word.

```cpp
class Solution {
public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        int idx=0,n=words.size();
        vector<string>res;
        while(idx<n){
            int total=words[idx].size(),last=idx+1;
            while(last<n && total+words[last].size()+(last-idx)<=maxWidth)
                total+=words[last++].size();
            int count=last-idx;
            int count_space=maxWidth-total;
            string temp;
            if(last==n || count==1){
                for(int i=idx;i<last;i++){
                    temp+=words[i];
                    if(i+1<last)
                    temp+=" ";
                }
                temp+=string(maxWidth-temp.size(),' ');
            }else{
                int space_per_word=count_space/(count-1);
                int extra=count_space%(count-1);
                for(int i=idx;i<last;i++){
                    temp+=words[i];
                    if(i+1<last)
                    temp+=string(space_per_word+(i-idx<extra?1:0),' ');
                }
            }
            res.push_back(temp);
            idx=last;
        }
        return res;
    }
};
```

### 18 Minimum One Bit Operations to Make Integers 0

Given an integer n, you must transform it into 0 using the following operations any number of times:

Change the rightmost (0th) bit in the binary representation of n.
Change the ith bit in the binary representation of n if the (i-1)th bit is set to 1 and the (i-2)th through 0th bits are set to 0.
Return the minimum number of operations to transform n into 0.

```cpp
class Solution {
public:
    int minimumOneBitOperations(int n) {
        if(n<=1)
        return n;
        int bit=0;
        while((1<<bit)<=n)
            bit++;
        return (((1<<bit)-1)-minimumOneBitOperations(n-(1<<(bit-1))));
    }
};
```

### 19 Shortest Palindrome

You are given a string s. You can convert s to a palindrome by adding characters in front of it.

Return the shortest palindrome you can find by performing this transformation.

```cpp
class Solution {
public:
    vector<int>prefix_function(string s){
        vector<int>pi(s.size(),0);
        for(int i=1;i<s.size();i++){
            int j=pi[i-1];
            while(j>0 && s[i]!=s[j])
            j=pi[j-1];
            if(s[i]==s[j])
            j++;
            pi[i]=j;
        }
        return pi;
    }
    string shortestPalindrome(string s) {
        string s_new,rev=s;
        reverse(rev.begin(),rev.end());
        s_new=s+'$'+rev;
        vector<int>pi=prefix_function(s_new);
        int num_chars=s.size()-pi.back();
        string res=rev.substr(0,num_chars)+s;
        return res;
    }
};
```

### 20 Word Ladder II

A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

Every adjacent pair of words differs by a single letter.
Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
sk == endWord
Given two words, beginWord and endWord, and a dictionary wordList, return all the shortest transformation sequences from beginWord to endWord, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words [beginWord, s1, s2, ..., sk].

```cpp
class Solution {
    string b;
    map<string,int>mp;
public:
    void DFS(vector<vector<string>>&res,vector<string>&temp,string str){
        if(str==b){
            reverse(temp.begin(),temp.end());
            res.push_back(temp);
            reverse(temp.begin(),temp.end());
            return;
        }
        int steps=mp[str];
        for(int i=0;i<str.size();i++){
            char org=str[i];
            for(char c='a';c<='z';c++){
                str[i]=c;
                if(mp.find(str)!=mp.end() && mp[str]+1==steps){
                    temp.push_back(str);
                    DFS(res,temp,str);
                    temp.pop_back();
                }
            }
            str[i]=org;
        }
    }
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        vector<vector<string>>res;
        set<string>s(wordList.begin(),wordList.end());
        queue<string>q;
        q.push(beginWord);
        mp[beginWord]=1;
        b=beginWord;
        while(!q.empty()){
            string str=q.front();
            q.pop();
            int steps=mp[str];
            if(str==endWord)
            break;
            for(int i=0;i<str.size();i++){
                char org=str[i];
                for(char c='a';c<='z';c++){
                    str[i]=c;
                    if(s.count(str) && mp.find(str)==mp.end()){
                        mp[str]=steps+1;
                        q.push(str);
                    }
                }
                str[i]=org;
            }
        }
        if(mp.find(endWord)!=mp.end()){
            vector<string>temp;
            temp.push_back(endWord);
            DFS(res,temp,endWord);
        }
        return res;
    }
};
```

### 21 Minimum Cost to cut a board into squares
Given a board of dimensions n × m that needs to be cut into n*m squares. The cost of making a cut along a horizontal or vertical edge is provided in two arrays:

x[]: Cutting costs along the vertical edges (length-wise).
y[]: Cutting costs along the horizontal edges (width-wise).
Find the minimum total cost required to cut the board into squares optimally.

```cpp
class Solution {
  public:
    int minCost(int n, int m, vector<int>& x, vector<int>& y) {
        sort(x.rbegin(),x.rend());
        sort(y.rbegin(),y.rend());
        int cost=0;
        int hor_cuts=1,ver_cuts=1,i=0,j=0;
        while(i<x.size() && j<y.size()){
            if(x[i]>y[j]){
                cost+=x[i]*hor_cuts;
                ver_cuts++;
                i++;
            }else{
                cost+=y[j]*ver_cuts;
                hor_cuts++;
                j++;
            }
        }
        while(i<x.size()){
            cost+=x[i]*hor_cuts;
            ver_cuts++;
            i++;
        }
        while(j<y.size()){
            cost+=y[j]*ver_cuts;
            hor_cuts++;
            j++;
        }
        return cost;
    }
};
```

### 22 Recontruct Itinerary

You are given a list of airline tickets where tickets[i] = [fromi, toi] represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from "JFK", thus, the itinerary must begin with "JFK". If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

 ```cpp
class Solution {
public:
    void DFS(string& src,map<string,priority_queue<string,vector<string>,greater<string>>>&graph,vector<string>&route){
        priority_queue<string,vector<string>,greater<string>>&pq=graph[src];
        while(!pq.empty()){
            string dest=pq.top();
            pq.pop();
            DFS(dest,graph,route);
        }
        route.push_back(src);
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        map<string,priority_queue<string,vector<string>,greater<string>>>graph;
        for(auto ticket:tickets)
            graph[ticket[0]].push(ticket[1]);
        vector<string>route;
        string src="JFK";
        DFS(src,graph,route);
        reverse(route.begin(),route.end());
        return route;
    }
};
```
