### 1. k largest elements
Given an array arr[] of positive integers and an integer k, Your task is to return k largest elements in decreasing order. 

```cpp
class Solution {
  public:
    vector<int> kLargest(vector<int>& arr, int k) {
        priority_queue<int,vector<int>>pq;
        vector<int>res;
        for(auto ele:arr)
        pq.push(ele);
        while(k--){
            int ele=pq.top();
            pq.pop();
            res.push_back(ele);
        }
        return res;
    }
};
```

### 2. K Closest Points to Origin
Given an array of points where each point is represented as points[i] = [xi, yi] on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).

The distance between two points on the X-Y plane is the Euclidean distance, defined as: 

sqrt( (x2 - x1)2 + (y2 - y1)2 )

Note: You can return the k closest points in any order, driver code will sort them before printing.

```cpp
class Solution {
  public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        vector<vector<int>>res;
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>>pq;
        for(int i=0;i<points.size();i++)
            pq.push({points[i][0]*points[i][0]+points[i][1]*points[i][1],i});
        while(k--){
            pair<int,int>pi=pq.top();
            pq.pop();
            res.push_back(points[pi.second]);
        }
        return res;
    }
};
```

### 3. Merge K sorted linked lists
Given an array arr[] of n sorted linked lists of different sizes. The task is to merge them in such a way that after merging they will be a single sorted linked list, then return the head of the merged linked list.

```cpp
class Solution {
  public:
    Node* mergeKLists(vector<Node*>& arr) {
        priority_queue<int,vector<int>,greater<int>>pq;
        for(auto head:arr){
            while(head){
                pq.push(head->data);
                head=head->next;
            }
        }
        Node*dummy=new Node(0);
        Node* ptr=dummy;
        while(!pq.empty()){
            int val=pq.top();
            pq.pop();
            Node* newNode=new Node(val);
            ptr->next=newNode;
            ptr=ptr->next;
        }
        return dummy->next;
    }
};
```

### 4. Find median in a stream
Given a data stream arr[] where integers are read sequentially, the task is to determine the median of the elements encountered so far after each new integer is read.

There are two cases for median on the basis of data set size.

1. If the data set has an odd number then the middle one will be consider as median.
2. If the data set has an even number then there is no distinct middle value and the median will be the arithmetic mean of the two middle values.

```cpp
class Solution {
  public:
    vector<double> getMedian(vector<int> &arr) {
        priority_queue<int,vector<int>>left;
        priority_queue<int,vector<int>,greater<int>>right;
        vector<double>res;
        for(int i=0;i<arr.size();i++){
            left.push(arr[i]);
            int maxo=left.top();
            right.push(maxo);
            left.pop();
            if(right.size()>left.size()){
                int mino=right.top();
                left.push(mino);
                right.pop();
            }
            if(left.size()!=right.size())
            res.push_back(left.top());
            else
            res.push_back((left.top()+right.top())/2.0);
        }
        return res;
    }
};
```

### 5. Top K Frequent in Array
Given a non-empty integer array arr[] of size n, find the top k elements which have the highest frequency in the array.

Note: If two numbers have the same frequencies, then the larger number should be given more preference.

```cpp
struct Compare{
    bool operator()(pair<int,int> a,pair<int,int> b){
        if(a.second<b.second)
        return true;
        else if(a.second>b.second)
        return false;
        else if(a.first<b.first)
        return true;
        else
        return false;
    }  
};
class Solution {
  public:
    vector<int> topKFrequent(vector<int> &arr, int k) {
        priority_queue<pair<int,int>,vector<pair<int,int>>,Compare>pq;
        map<int,int>mp;
        vector<int>res;
        for(auto ele:arr)
        mp[ele]++;
        for(auto item:mp)
            pq.push(item);
        while(k-- && !pq.empty()){
            pair<int,int>pi=pq.top();
            pq.pop();
            res.push_back(pi.first);
        }
        return res;
    }
};
```

### 6. Meeting Rooms II
Given two arrays start[] and end[] such that start[i] is the starting time of ith meeting and end[i] is the ending time of ith meeting. Return the minimum number of rooms required to attend all meetings.

```cpp
class Solution {
  public:
    int minMeetingRooms(vector<int> &start, vector<int> &end) {
        int res=0,room=0;
        int i=0,j=0;
        sort(start.begin(),start.end());
        sort(end.begin(),end.end());
        while(i<start.size() && j<end.size()){
            if(start[i]<end[j]){
                room++;
                i++;
            }
            else{
                room--;
                j++;
            }
            res=max(res,room);
        }
        return res;
    }
};
```

### 7. Meeting Rooms III
You are given an integer n representing the number of rooms numbered from 0 to n - 1. Additionally, you are given a 2D integer array meetings[][] where meetings[i] = [starti, endi] indicates that a meeting is scheduled during the half-closed time interval [starti, endi). All starti values are unique.

Meeting Allocation Rules:

When a meeting starts, assign it to the available room with the smallest number.
If no rooms are free, delay the meeting until the earliest room becomes available. The delayed meeting retains its original duration.
When a room becomes free, assign it to the delayed meeting with the earliest original start time.
Determine the room number that hosts the most meetings. If multiple rooms have the same highest number of meetings, return the smallest room number among them.

```cpp
class Solution {
  public:
    int mostBooked(int n, vector<vector<int>> &meetings) {
        vector<int>freq(n,0);
        priority_queue<int,vector<int>,greater<int>>avail;
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>>occ;
        for(int i=0;i<n;i++)
        avail.push(i);
        sort(meetings.begin(),meetings.end());
        int room;
        for(auto item:meetings){
            int s=item[0],e=item[1];
            while(!occ.empty() && occ.top().first<=s)
            {
                int r=occ.top().second;
                avail.push(r);
                occ.pop();
            }
            if(!avail.empty()){
                room=avail.top();
                avail.pop();
                occ.push({e,room});
            }
            else{
                int firstendtime=occ.top().first;
                room=occ.top().second;
                occ.pop();
                occ.push({firstendtime+(e-s),room});
            }
            freq[room]++;
        }
        int maxcnt=0;
        int maxroom;
        for(int i=0;i<n;i++){
            if(maxcnt<freq[i]){
                maxcnt=freq[i];
                maxroom=i;
            }
        }
        return maxroom;
    }
};
```

### 8. Rearrange characters
Given a string s with repeated characters, the task is to rearrange characters in a string such that no two adjacent characters are the same.
Note: The string has only lowercase English alphabets and it can have multiple solutions. Return any one of them. If there is no possible solution, then print empty string ("").

```cpp
class Solution {
  public:
    string rearrangeString(string s) {
        int maxcnt=0;
        char maxchar;
        map<char,int>mp;
        for(auto c:s){
            mp[c]++;
            if(mp[c]>maxcnt)
            {
                maxcnt=mp[c];
                maxchar=c;
            }
        }
        if(maxcnt>(s.size()+1)/2)
        return "";
        int idx=0;
        while(maxcnt--){
            s[idx]=maxchar;
            idx+=2;
            mp[maxchar]--;
        }
        for(char c='a';c<='z';c++){
            while(mp[c]){
                idx=(idx>=s.size())?1:idx;
                s[idx]=c;
                idx+=2;
                mp[c]--;
            }
        }
        return s;
    }
};
```
