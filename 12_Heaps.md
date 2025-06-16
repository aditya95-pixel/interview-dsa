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
