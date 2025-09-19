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

### 22 Recontruct Itinerary (Very Important Eulerian Path Concept)

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

### 23 Bus Routes

You are given an array routes representing bus routes where routes[i] is a bus route that the ith bus repeats forever.

For example, if routes[0] = [1, 5, 7], this means that the 0th bus travels in the sequence 1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ... forever.
You will start at the bus stop source (You are not on any bus initially), and you want to go to the bus stop target. You can travel between bus stops by buses only.

Return the least number of buses you must take to travel from source to target. Return -1 if it is not possible.

```cpp
class Solution {
public:
    int numBusesToDestination(vector<vector<int>>& routes, int source, int target) {
        if(source==target)
        return 0;
        map<int,vector<int>>mp;
        for(int i=0;i<routes.size();i++){
            for(int j=0;j<routes[i].size();j++)
            mp[routes[i][j]].push_back(i);
        }
        queue<int>q;
        vector<int>vis_routes(routes.size(),0);
        set<int>vis_stops;
        for(auto route:mp[source]){
            q.push(route);
            vis_routes[route]=1;
        }
        vis_stops.insert(source);
        int buses=1;
        while(!q.empty()){
            int sz=q.size();
            while(sz--){
                int route=q.front();
                q.pop();
                for(auto stop:routes[route]){
                    if(stop==target)
                    return buses;
                    if(vis_stops.count(stop))
                    continue;
                    vis_stops.insert(stop);
                    for(auto next_route:mp[stop]){
                        if(vis_routes[next_route]==0)
                        {
                            q.push(next_route);
                            vis_routes[next_route]=1;
                        }
                    }
                }
            }
            buses++;
        }
        return -1;
    }
};
```

### 24 Shortest Path Visiting all nodes (Hamiltonian Path)

You have an undirected, connected graph of n nodes labeled from 0 to n - 1. You are given an array graph where graph[i] is a list of all the nodes connected with node i by an edge.

Return the length of the shortest path that visits every node. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.

```cpp
class Solution {
public:
    int shortestPathLength(vector<vector<int>>& graph) {
        int allVis=(1<<graph.size())-1;
        vector<vector<bool>>vis(graph.size(),vector<bool>(1<<graph.size(),0));
        queue<pair<int,int>>q;
        for(int i=0;i<graph.size();i++){
            q.push({i,(1<<i)});
            vis[i][(1<<i)]=1;
        }
        int steps=0;
        while(!q.empty()){
            int sz=q.size();
            while(sz--){
                int u=q.front().first;
                int mask=q.front().second;
                q.pop();
                if(mask==allVis)
                return steps;
                for(auto v:graph[u]){
                    int newmask=mask|(1<<v);
                    if(!vis[v][newmask])
                    {
                        vis[v][newmask]=1;
                        q.push({v,newmask});
                    }
                }
            }
            steps++;
        }
        return -1;
    }
};
```

### 25 Critical Connections in a Network

There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some servers unable to reach some other server.

Return all critical connections in the network in any order.

```cpp
class Solution {
public:
    vector<int>disc,low;
    vector<vector<int>>res;
    vector<vector<int>>adj;
    int timer;
    void dfs(int u,int parent){
        disc[u]=low[u]=++timer;
        for(auto v:adj[u]){
            if(v==parent)
            continue;
            if(!disc[v]){
                dfs(v,u);
                low[u]=min(low[u],low[v]);
                if(low[v]>disc[u])
                res.push_back({u,v});
            }else
            low[u]=min(low[u],disc[v]);
        }
    }
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        adj.assign(n,{});
        for(auto edge:connections){
            adj[edge[0]].push_back(edge[1]);
            adj[edge[1]].push_back(edge[0]);
        }
        disc.assign(n,0);
        low.assign(n,0);
        timer=0;
        for(int i=0;i<n;i++){
            if(!disc[i])
            dfs(i,-1);
        }
        return res;
    }
};
```

### 26 Course Schedule IV

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course ai first if you want to take course bi.

For example, the pair [0, 1] indicates that you have to take course 0 before you can take course 1.
Prerequisites can also be indirect. If course a is a prerequisite of course b, and course b is a prerequisite of course c, then course a is a prerequisite of course c.

You are also given an array queries where queries[j] = [uj, vj]. For the jth query, you should answer whether course uj is a prerequisite of course vj or not.

Return a boolean array answer, where answer[j] is the answer to the jth query.

```cpp
class Solution {
public:
    vector<bool> checkIfPrerequisite(int numCourses, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) {
        vector<vector<bool>>graph(numCourses,vector<bool>(numCourses,0));
        for(auto edge:prerequisites)
            graph[edge[0]][edge[1]]=1;
        for(int k=0;k<numCourses;k++){
            for(int i=0;i<numCourses;i++){
                for(int j=0;j<numCourses;j++){
                    if(graph[i][k] && graph[k][j])
                    graph[i][j]=1;
                }
            }
        }
        vector<bool>res;
        for(auto edge:queries){
            if(graph[edge[0]][edge[1]])
            res.push_back(1);
            else
            res.push_back(0);
        }
        return res;
    }
};
```

### 27 Longest Path with different adjacent characters
You are given a tree (i.e. a connected, undirected graph that has no cycles) rooted at node 0 consisting of n nodes numbered from 0 to n - 1. The tree is represented by a 0-indexed array parent of size n, where parent[i] is the parent of node i. Since node 0 is the root, parent[0] == -1.

You are also given a string s of length n, where s[i] is the character assigned to node i.

Return the length of the longest path in the tree such that no pair of adjacent nodes on the path have the same character assigned to them.

```cpp
class Solution {
public:
    vector<vector<int>>adj;
    int maxlen=0;
    int dfs(int u,string &s){
        int len1=0,len2=0;
        for(auto v:adj[u]){
            int len=dfs(v,s);
            if(s[u]==s[v])
            continue;
            else{
                if(len>len1)
                {
                    len2=len1;
                    len1=len;
                }
                else if(len>len2)
                len2=len;
            }
        }
        maxlen=max(maxlen,1+len1+len2);
        return 1+len1;
    }
    int longestPath(vector<int>& parent, string s) {
        adj.resize(parent.size(),{});
        for(int i=1;i<parent.size();i++)
            adj[parent[i]].push_back(i);
        dfs(0,s);
        return maxlen;
    }
};
```

### 28 Replace Non-Coprime Numbers in Array

You are given an array of integers nums. Perform the following steps:

Find any two adjacent numbers in nums that are non-coprime.
If no such numbers are found, stop the process.
Otherwise, delete the two numbers and replace them with their LCM (Least Common Multiple).
Repeat this process as long as you keep finding two adjacent non-coprime numbers.
Return the final modified array. It can be shown that replacing adjacent non-coprime numbers in any arbitrary order will lead to the same result.

The test cases are generated such that the values in the final array are less than or equal to 108.

Two values x and y are non-coprime if GCD(x, y) > 1 where GCD(x, y) is the Greatest Common Divisor of x and y.

```cpp
class Solution {
public:
    int gcd(int a,int b){
        if(b==0)
        return a;
        else
        return gcd(b,a%b);
    }
    vector<int> replaceNonCoprimes(vector<int>& nums) {
        vector<int>res;
        stack<int>stk;  
        for(int i=0;i<nums.size();i++){
            long long temp=nums[i];
            while(!stk.empty()){
                int top=stk.top();
                int g=gcd(top,temp);
                if(g==1)
                break;
                stk.pop();
                temp=(long long)top/g*temp;
            }
            stk.push((int)temp);
        }
        while(!stk.empty()){
            res.push_back(stk.top());
            stk.pop();
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```

### 29 Design a Food Rating System

Design a food rating system that can do the following:

Modify the rating of a food item listed in the system.
Return the highest-rated food item for a type of cuisine in the system.
Implement the FoodRatings class:

FoodRatings(String[] foods, String[] cuisines, int[] ratings) Initializes the system. The food items are described by foods, cuisines and ratings, all of which have a length of n.
foods[i] is the name of the ith food,
cuisines[i] is the type of cuisine of the ith food, and
ratings[i] is the initial rating of the ith food.
void changeRating(String food, int newRating) Changes the rating of the food item with the name food.
String highestRated(String cuisine) Returns the name of the food item that has the highest rating for the given type of cuisine. If there is a tie, return the item with the lexicographically smaller name.
Note that a string x is lexicographically smaller than string y if x comes before y in dictionary order, that is, either x is a prefix of y, or if i is the first position such that x[i] != y[i], then x[i] comes before y[i] in alphabetic order.

```cpp
class FoodRatings {
    map<string,map<string,int>>mp1;
    map<string,string>mp2;
    map<string,set<pair<int,string>>>mp3;
public:
    FoodRatings(vector<string>& foods, vector<string>& cuisines, vector<int>& ratings) {
        for(int i=0;i<foods.size();i++){
            mp1[cuisines[i]].insert({foods[i],ratings[i]});
            mp2[foods[i]]=cuisines[i];
            mp3[cuisines[i]].insert({-ratings[i],foods[i]});    
        }
    }
    
    void changeRating(string food, int newRating) {
        string cuisine=mp2[food];
        int oldRating=mp1[cuisine][food];
        mp1[cuisine][food]=newRating;
        mp3[cuisine].erase({-oldRating,food});
        mp3[cuisine].insert({-newRating,food});  
    }
    
    string highestRated(string cuisine) {
        return (*(mp3[cuisine].begin())).second;
    }
};

/**
 * Your FoodRatings object will be instantiated and called as such:
 * FoodRatings* obj = new FoodRatings(foods, cuisines, ratings);
 * obj->changeRating(food,newRating);
 * string param_2 = obj->highestRated(cuisine);
 */
```

### 30 Swim in Rising Water

You are given an n x n integer matrix grid where each value grid[i][j] represents the elevation at that point (i, j).

It starts raining, and water gradually rises over time. At time t, the water level is t, meaning any cell with elevation less than equal to t is submerged or reachable.

You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most t. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return the minimum time until you can reach the bottom right square (n - 1, n - 1) if you start at the top left square (0, 0).

```cpp
class Solution {
public:
    int swimInWater(vector<vector<int>>& grid) {
        priority_queue<vector<int>,vector<vector<int>>,greater<>>pq;
        pq.push({grid[0][0],0,0});
        set<pair<int,int>>vis;
        vis.insert({0,0});
        while(!pq.empty()){
            vector<int>v=pq.top();
            pq.pop();
            if(v[1]==grid.size()-1 && v[2]==grid[0].size()-1)
            return v[0];
            if(v[1]-1>=0 && !vis.count({v[1]-1,v[2]}))
            {
                vis.insert({v[1]-1,v[2]});
                pq.push({max(v[0],grid[v[1]-1][v[2]]),v[1]-1,v[2]});
            }
            if(v[1]+1<grid.size() && !vis.count({v[1]+1,v[2]}))
            {
                vis.insert({v[1]+1,v[2]});
                pq.push({max(v[0],grid[v[1]+1][v[2]]),v[1]+1,v[2]});
            }
            if(v[2]-1>=0 && !vis.count({v[1],v[2]-1}))
            {
                vis.insert({v[1],v[2]-1});
                pq.push({max(v[0],grid[v[1]][v[2]-1]),v[1],v[2]-1});
            }
            if(v[2]+1<grid[0].size() && !vis.count({v[1],v[2]+1}))
            {
                vis.insert({v[1],v[2]+1});
                pq.push({max(v[0],grid[v[1]][v[2]+1]),v[1],v[2]+1});
            }
        }
        return -1;
    }
};
```

### 31 Min Cost to Connect All Points
You are given an array points representing integer coordinates of some points on a 2D-plane, where points[i] = [xi, yi].

The cost of connecting two points [xi, yi] and [xj, yj] is the manhattan distance between them: |xi - xj| + |yi - yj|, where |val| denotes the absolute value of val.

Return the minimum cost to make all points connected. All points are connected if there is exactly one simple path between any two points.

```cpp
class DSU{
    vector<int>parent,rank;
    public:
    DSU(int v){
        parent.resize(v,-1);
        rank.resize(v,0);
    }
    int find(int v){
        if(parent[v]==-1)
        return v;
        else
        return parent[v]=find(parent[v]);
    }
    void merge(int u,int v){
        int x=find(u),y=find(v);
        if(x!=y){
            if(rank[x]>rank[y])
            parent[y]=x;
            else if(rank[y]>rank[x])
            parent[x]=y;
            else
            {
                parent[x]=y;
                rank[y]++;
            }
        }
    }
};
static bool compare(vector<int>&a,vector<int>&b){
    return a[2]<b[2];
}
class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        vector<vector<int>>edges;
        for(int i=0;i<points.size();i++){
            for(int j=i+1;j<points.size();j++)
            edges.push_back({i,j,abs(points[i][0]-points[j][0])
            +abs(points[i][1]-points[j][1])});
        }
        sort(edges.begin(),edges.end(),compare);
        DSU *d=new DSU(points.size());
        int res=0,cnt=0;
        for(auto edge:edges){
            int u=edge[0],v=edge[1],w=edge[2];
            if(d->find(u)!=d->find(v)){
                d->merge(u,v);
                res+=w;
                cnt++;
            }
            if(cnt==points.size()-1)
            break;
        }
        return res;
    }
};
```

### 32 Remove Max number of edges to keep graph fully traversable

Alice and Bob have an undirected graph of n nodes and three types of edges:

Type 1: Can be traversed by Alice only.
Type 2: Can be traversed by Bob only.
Type 3: Can be traversed by both Alice and Bob.
Given an array edges where edges[i] = [typei, ui, vi] represents a bidirectional edge of type typei between nodes ui and vi, find the maximum number of edges you can remove so that after removing the edges, the graph can still be fully traversed by both Alice and Bob. The graph is fully traversed by Alice and Bob if starting from any node, they can reach all other nodes.

Return the maximum number of edges you can remove, or return -1 if Alice and Bob cannot fully traverse the graph.

 ```cpp
class DSU{
    vector<int>parent,rank;
    public:
    DSU(int v){
        parent.resize(v,-1);
        rank.resize(v,0);
    }
    int find(int v){
        if(parent[v]==-1)
        return v;
        else
        return parent[v]=find(parent[v]);
    }
    bool merge(int u,int v){
        int x=find(u),y=find(v);
        if(x==y)
        return false;
        if(x!=y){
            if(rank[x]>rank[y])
            parent[y]=x;
            else if(rank[y]>rank[x])
            parent[x]=y;
            else
            {
                parent[x]=y;
                rank[y]++;
            }
        }
        return true;
    }
};
class Solution {
public:
    int maxNumEdgesToRemove(int n, vector<vector<int>>& edges) {
        DSU *d1,*d2;
        d1=new DSU(n+1);
        d2=new DSU(n+1);
        int cnt=0;
        for(auto edge:edges){
            if(edge[0]==3){
                if(d1->merge(edge[1],edge[2]))
                {
                    d2->merge(edge[1],edge[2]);
                    cnt++;
                }
            }
        }
        for(auto edge:edges){
            if(edge[0]==1){
                if(d1->merge(edge[1],edge[2]))
                    cnt++;
            }
        }
        for(auto edge:edges){
            if(edge[0]==2){
                if(d2->merge(edge[1],edge[2]))
                    cnt++;
            }
        }
        int root1=d1->find(1),root2=d2->find(1);
        for(int i=2;i<=n;i++){
            if(d1->find(i)!=root1 || d2->find(i)!=root2)
            return -1;
        }
        return edges.size()-cnt;
    }
};
```

### 33 Parallel Courses III

You are given an integer n, which indicates that there are n courses labeled from 1 to n. You are also given a 2D integer array relations where relations[j] = [prevCoursej, nextCoursej] denotes that course prevCoursej has to be completed before course nextCoursej (prerequisite relationship). Furthermore, you are given a 0-indexed integer array time where time[i] denotes how many months it takes to complete the (i+1)th course.

You must find the minimum number of months needed to complete all the courses following these rules:

You may start taking a course at any time if the prerequisites are met.
Any number of courses can be taken at the same time.
Return the minimum number of months needed to complete all the courses.

Note: The test cases are generated such that it is possible to complete every course (i.e., the graph is a directed acyclic graph).

```cpp
class Solution {
public:
    int minimumTime(int n, vector<vector<int>>& relations, vector<int>& time) {
        vector<vector<int>>adj(n+1);
        vector<int>indeg(n+1,0),dp(n+1,0);
        for(int i=0;i<relations.size();i++){
            adj[relations[i][0]].push_back(relations[i][1]);
            indeg[relations[i][1]]++;
        }
        queue<int>q;
        int ans=0;
        for(int i=1;i<=n;i++){
            if(indeg[i]==0){
                q.push(i);
                dp[i]=time[i-1];
                ans=max(ans,dp[i]);
            }
        }
        while(!q.empty()){
            int u=q.front();
            q.pop();
            for(auto v:adj[u]){
                indeg[v]--;
                if(indeg[v]==0)
                q.push(v);
                if(dp[u]+time[v-1]>dp[v])
                {
                    dp[v]=dp[u]+time[v-1];
                    ans=max(ans,dp[v]);
                }
            }
        }
        return ans;
    }
};
```

### 34 Longest Cycle in a Graph

You are given a directed graph of n nodes numbered from 0 to n - 1, where each node has at most one outgoing edge.

The graph is represented with a given 0-indexed array edges of size n, indicating that there is a directed edge from node i to node edges[i]. If there is no outgoing edge from node i, then edges[i] == -1.

Return the length of the longest cycle in the graph. If no cycle exists, return -1.

A cycle is a path that starts and ends at the same node.

```cpp
class Solution {
    int maxlen=-1;
public:
    void dfs(int u,vector<int>&vis,vector<int>&dep,int d,vector<int>& edges){
        dep[u]=d;
        vis[u]=1;
        int v=edges[u];
        if(v!=-1){
            if(vis[v]==0)
            dfs(v,vis,dep,d+1,edges);
            else if(vis[v]==1)
            maxlen=max(maxlen,d-dep[v]+1);
        }
        vis[u]=2;
    }
    int longestCycle(vector<int>& edges) {
        vector<int>vis(edges.size(),0),dep(edges.size(),-1);
        for(int i=0;i<edges.size();i++){
            if(vis[i]==0)
                dfs(i,vis,dep,0,edges);
        }
        return maxlen;
    }
};
```

### 35 Design Task Manager
There is a task management system that allows users to manage their tasks, each associated with a priority. The system should efficiently handle adding, modifying, executing, and removing tasks.

Implement the TaskManager class:

TaskManager(vector<vector<int>>& tasks) initializes the task manager with a list of user-task-priority triples. Each element in the input list is of the form [userId, taskId, priority], which adds a task to the specified user with the given priority.

void add(int userId, int taskId, int priority) adds a task with the specified taskId and priority to the user with userId. It is guaranteed that taskId does not exist in the system.

void edit(int taskId, int newPriority) updates the priority of the existing taskId to newPriority. It is guaranteed that taskId exists in the system.

void rmv(int taskId) removes the task identified by taskId from the system. It is guaranteed that taskId exists in the system.

int execTop() executes the task with the highest priority across all users. If there are multiple tasks with the same highest priority, execute the one with the highest taskId. After executing, the taskId is removed from the system. Return the userId associated with the executed task. If no tasks are available, return -1.

Note that a user may be assigned multiple tasks.

```cpp
class TaskManager {
    set<pair<int,int>>s;
    map<int,int>mp;
    map<int,int>task_user;
public:
    TaskManager(vector<vector<int>>& tasks) {
        for(auto task:tasks)
        {
            s.insert({-task[2],-task[1]});
            mp[task[1]]=task[2];
            task_user[task[1]]=task[0];
        }
    }
    
    void add(int userId, int taskId, int priority) {
        s.insert({-priority,-taskId});
        mp[taskId]=priority;
        task_user[taskId]=userId;
    }
    
    void edit(int taskId, int newPriority) {
        int priority=mp[taskId];
        s.erase({-priority,-taskId});
        s.insert({-newPriority,-taskId});
        mp[taskId]=newPriority;
    }
    
    void rmv(int taskId) {
        int priority=mp[taskId];
        s.erase({-priority,-taskId});
        mp.erase(taskId);
        task_user.erase(taskId);
    }
    
    int execTop() {
        if(s.size()==0)
        return -1;
        int maxtask=-(*s.begin()).second;
        s.erase(*s.begin());
        int userId=task_user[maxtask];
        task_user.erase(maxtask);
        mp.erase(maxtask);
        return userId;
    }
};

/**
 * Your TaskManager object will be instantiated and called as such:
 * TaskManager* obj = new TaskManager(tasks);
 * obj->add(userId,taskId,priority);
 * obj->edit(taskId,newPriority);
 * obj->rmv(taskId);
 * int param_4 = obj->execTop();
 */
```

### 36 Longest Subarray Length

You are given an array of integers arr[]. Your task is to find the length of the longest subarray such that all the elements of the subarray are smaller than or equal to the length of the subarray.

```cpp
class Solution {
  public:
    vector<int>nextGreater(vector<int>&arr){
        vector<int>ng(arr.size(),arr.size());
        stack<int>stk;
        for(int i=arr.size()-1;i>=0;i--){
            while(!stk.empty() && arr[stk.top()]<=arr[i])
            stk.pop();
            if(!stk.empty())
            ng[i]=stk.top();
            stk.push(i);
        }
        return ng;
    }
    vector<int>prevGreater(vector<int>&arr){
        vector<int>pg(arr.size(),-1);
        stack<int>stk;
        for(int i=0;i<arr.size();i++){
            while(!stk.empty() && arr[stk.top()]<=arr[i])
            stk.pop();
            if(!stk.empty())
            pg[i]=stk.top();
            stk.push(i);
        }
        return pg;
    }
    int longestSubarray(vector<int>& arr) {
        // code here
        vector<int>ng=nextGreater(arr);
        vector<int>pg=prevGreater(arr);
        int maxlen=0;
        for(int i=0;i<arr.size();i++){
            int len=ng[i]-pg[i]-1;
            if(arr[i]<=len)
            maxlen=max(maxlen,len);
        }
        return maxlen;
    }
};
```

### 37 Wildcard Matching

Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        vector<vector<int>>dp(s.size()+1,vector<int>(p.size()+1,0));
        dp[0][0]=1;
        for(int j=1;j<=p.size();j++)
        {
            if(p[j-1]=='*')
            dp[0][j]=dp[0][j-1];
        }
        for(int i=1;i<=s.size();i++){
            for(int j=1;j<=p.size();j++){
                if(s[i-1]==p[j-1] || p[j-1]=='?')
                dp[i][j]=dp[i-1][j-1];
                else if(p[j-1]=='*')
                dp[i][j]=dp[i-1][j] || dp[i][j-1];
            }
        }
        return dp[s.size()][p.size()];
    }
};
```
