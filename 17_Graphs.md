### 1. DFS of Graph
Given a connected undirected graph containing V vertices represented by a 2-d adjacency list adj[][], where each adj[i] represents the list of vertices connected to vertex i. Perform a Depth First Search (DFS) traversal starting from vertex 0, visiting vertices from left to right as per the given adjacency list, and return a list containing the DFS traversal of the graph.

Note: Do traverse in the same order as they are in the given adjacency list.

```cpp
class Solution {
  public:
    void DFS(vector<vector<int>>& adj,int u,set<int>&s,vector<int>&res){
        for(auto v:adj[u]){
            if(s.find(v)==s.end()){
                s.insert(v);
                res.push_back(v);
                DFS(adj,v,s,res);
            }
        }
    }
    vector<int> dfs(vector<vector<int>>& adj) {
        vector<int>res;
        set<int>s;
        s.insert(0);
        res.push_back(0);
        DFS(adj,0,s,res);
        return res;
    }
};
```

### 2. BFS of graph
Given a connected undirected graph containing V vertices, represented by a 2-d adjacency list adj[][], where each adj[i] represents the list of vertices connected to vertex i. Perform a Breadth First Search (BFS) traversal starting from vertex 0, visiting vertices from left to right according to the given adjacency list, and return a list containing the BFS traversal of the graph.

Note: Do traverse in the same order as they are in the given adjacency list.

```cpp
class Solution {
  public:
    // Function to return Breadth First Traversal of given graph.
    vector<int> bfs(vector<vector<int>> &adj) {
        queue<int>q;
        q.push(0);
        vector<int>res;
        set<int>s;
        s.insert(0);
        while(!q.empty()){
            int u=q.front();
            q.pop();
            res.push_back(u);
            for(auto v:adj[u]){
                if(s.find(v)==s.end()){
                    q.push(v);
                    s.insert(v);
                }
            }
        }
        return res;
    }
};
```

### 3. Rotten Oranges
Given a matrix mat[][] of dimension n * m where each cell in the matrix can have values 0, 1 or 2 which has the following meaning:
0 : Empty cell
1 : Cell have fresh oranges
2 : Cell have rotten oranges

We have to determine what is the earliest time after which all the oranges are rotten. A rotten orange at index (i, j) can rot other fresh orange at indexes (i-1, j), (i+1, j), (i, j-1), (i, j+1) (up, down, left and right) in a unit time.

Note: Your task is to return the minimum time to rot all the fresh oranges. If not possible returns -1.

```cpp
class Solution {
  public:
    int orangesRotting(vector<vector<int>>& mat) {
        queue<pair<pair<int,int>,int>>q;
        set<pair<int,int>>s;
        for(int i=0;i<mat.size();i++){
            for(int j=0;j<mat[0].size();j++){
                if(mat[i][j]==2)
                {
                    q.push({{i,j},0});
                    s.insert({i,j});
                }
            }
        }
        int curr=0;
        while(!q.empty()){
            pair<int,int>p=q.front().first;
            int cnt=q.front().second;
            curr=cnt;
            q.pop();
            if(p.first+1<mat.size() && 
                s.find({p.first+1,p.second})==s.end() 
                && mat[p.first+1][p.second]==1)
            {
                q.push({{p.first+1,p.second},cnt+1});
                mat[p.first+1][p.second]=2;
                s.insert({p.first+1,p.second});
            }
            if(p.second+1<mat[0].size() && 
                s.find({p.first,p.second+1})==s.end() 
                && mat[p.first][p.second+1]==1)
            {
                q.push({{p.first,p.second+1},cnt+1});
                mat[p.first][p.second+1]=2;
                s.insert({p.first,p.second+1});
            }
            if(p.first-1>=0 && 
                s.find({p.first-1,p.second})==s.end() 
                && mat[p.first-1][p.second]==1)
            {
                q.push({{p.first-1,p.second},cnt+1});
                mat[p.first-1][p.second]=2;
                s.insert({p.first-1,p.second});
            }
            if(p.second-1>=0 && 
                s.find({p.first,p.second-1})==s.end() 
                && mat[p.first][p.second-1]==1)
            {
                q.push({{p.first,p.second-1},cnt+1});
                mat[p.first][p.second-1]=2;
                s.insert({p.first,p.second-1});
            }
        }
        for(int i=0;i<mat.size();i++){
            for(int j=0;j<mat[0].size();j++){
                if(mat[i][j]==1)
                return -1;
            }
        }
        return curr;
    }
};
```

### 4. Undirected Graph Cycle
Given an undirected graph with V vertices and E edges, represented as a 2D vector edges[][], where each entry edges[i] = [u, v] denotes an edge between vertices u and v, determine whether the graph contains a cycle or not.

```cpp
class Solution {
  public:
    bool DFS(int u,set<int>&vis,vector<vector<int>>&adj,int p){
        for(auto v:adj[u]){
            if(v!=p && vis.find(v)!=vis.end())
            return true;
            else if(v!=p){
                vis.insert(v);
                bool var=DFS(v,vis,adj,u);
                if(var)
                return true;
                vis.erase(v);
            }
        }
        return false;
    }
    bool isCycle(int V, vector<vector<int>>& edges) {
        vector<vector<int>>adj(V);
        for(auto arr:edges){
            adj[arr[0]].push_back(arr[1]);
            adj[arr[1]].push_back(arr[0]);
        }
        for(int i=0;i<V;i++){
            set<int>vis;
            vis.insert(i);
            bool var=DFS(i,vis,adj,-1);
            if(var)
            return true;
        }
        return false;
    }
};
```

### 5. Find the number of islands
Given a grid of size n*m (n is the number of rows and m is the number of columns in the grid) consisting of 'W's (Water) and 'L's (Land). Find the number of islands.

Note: An island is either surrounded by water or the boundary of a grid and is formed by connecting adjacent lands horizontally or vertically or diagonally i.e., in all 8 directions.

```cpp
class Solution {
  public:
    int countIslands(vector<vector<char>>& grid) {
        int cnt=0;
        set<pair<int,int>>s;
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(s.find({i,j})==s.end() && grid[i][j]=='L')
                {
                    cnt++;
                    queue<pair<int,int>>q;
                    q.push({i,j});
                    while(!q.empty()){
                        pair<int,int>p=q.front();
                        q.pop();
                        if(p.first-1>=0 && grid[p.first-1][p.second]=='L'
                        && s.find({p.first-1,p.second})==s.end())
                        {
                            s.insert({p.first-1,p.second});
                            q.push({p.first-1,p.second});
                        }
                        if(p.first+1<grid.size() && grid[p.first+1][p.second]=='L'
                        && s.find({p.first+1,p.second})==s.end())
                        {
                            s.insert({p.first+1,p.second});
                            q.push({p.first+1,p.second});
                        }
                        if(p.second-1>=0 && grid[p.first][p.second-1]=='L'
                        && s.find({p.first,p.second-1})==s.end())
                        {
                            s.insert({p.first,p.second-1});
                            q.push({p.first,p.second-1});
                        }
                        if(p.second+1<grid[0].size() && grid[p.first][p.second+1]=='L'
                        && s.find({p.first,p.second+1})==s.end())
                        {
                            s.insert({p.first,p.second+1});
                            q.push({p.first,p.second+1});
                        }
                        if(p.first-1>=0 && p.second-1>=0 && grid[p.first-1][p.second-1]=='L'
                        && s.find({p.first-1,p.second-1})==s.end())
                        {
                            s.insert({p.first-1,p.second-1});
                            q.push({p.first-1,p.second-1});
                        }
                        if(p.first-1>=0 && p.second+1<grid[0].size() && grid[p.first-1][p.second+1]=='L'
                        && s.find({p.first-1,p.second+1})==s.end())
                        {
                            s.insert({p.first-1,p.second+1});
                            q.push({p.first-1,p.second+1});
                        }
                        if(p.first+1<grid.size() && p.second-1>=0 && grid[p.first+1][p.second-1]=='L'
                        && s.find({p.first+1,p.second-1})==s.end())
                        {
                            s.insert({p.first+1,p.second-1});
                            q.push({p.first+1,p.second-1});
                        }
                        if(p.first+1<grid.size() && p.second+1<grid[0].size() && grid[p.first+1][p.second+1]=='L'
                        && s.find({p.first+1,p.second+1})==s.end())
                        {
                            s.insert({p.first+1,p.second+1});
                            q.push({p.first+1,p.second+1});
                        }
                    }
                }
            }
        }
        return cnt;
    }
};
```

### 6. Topological sort
Given a Directed Acyclic Graph (DAG) of V (0 to V-1) vertices and E edges represented as a 2D list of edges[][], where each entry edges[i] = [u, v] denotes a directed edge u -> v. Return the topological sort for the given graph.

Topological sorting for Directed Acyclic Graph (DAG) is a linear ordering of vertices such that for every directed edge u -> v, vertex u comes before v in the ordering.
Note: As there are multiple Topological orders possible, you may return any of them. If your returned Topological sort is correct then the output will be true else false.

```cpp
class Solution {
  public:
    vector<int> topoSort(int V, vector<vector<int>>& edges) {
        vector<vector<int>>adj(V);
        vector<int>indegree(V,0);
        vector<int>res;
        for(auto arr:edges){
            adj[arr[0]].push_back(arr[1]);
            indegree[arr[1]]++;
        }
        queue<int>q;
        for(int i=0;i<V;i++){
            if(indegree[i]==0)
            q.push(i);
        }
        while(!q.empty()){
            int u=q.front();
            q.pop();
            for(auto v:adj[u]){
                indegree[v]--;
                if(indegree[v]==0)
                q.push(v);
            }
            res.push_back(u);
        }
        return res;
    }
};
```

### 7. Directed Graph Cycle
Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, check whether it contains any cycle or not.
The graph is represented as a 2D vector edges[][], where each entry edges[i] = [u, v] denotes an edge from verticex u to v.

```cpp
class Solution {
  public:
    bool DFS(int u,set<int>&vis,vector<vector<int>>&adj){
        for(auto v:adj[u]){
            if(vis.find(v)!=vis.end())
            return true;
            else{
                vis.insert(v);
                bool var=DFS(v,vis,adj);
                if(var)
                return true;
                vis.erase(v);
            }
        }
        return false;
    }
    bool isCyclic(int V, vector<vector<int>> &edges) {
        vector<vector<int>>adj(V);
        for(auto arr:edges)
            adj[arr[0]].push_back(arr[1]);
        for(int i=0;i<V;i++){
            set<int>vis;
            vis.insert(i);
            bool var=DFS(i,vis,adj);
            if(var)
            return true;
        }
        return false;
    }
};
```

### 8. Bridge edge in a graph
Given an undirected graph with V vertices numbered from 0 to V-1 and E edges, represented by 2d array edges[][], where edges[i]=[u,v] represents the edge between the vertices u and v. Determine whether a specific edge between two vertices (c, d) is a bridge.

Note:

An edge is called a bridge if removing it increases the number of connected components of the graph.
if there’s only one path between c and d (which is the edge itself), then that edge is a bridge.

```cpp
class Solution {
  public:
    void DFS(int u,vector<vector<int>>&adj,set<int>&s){
        for(auto v:adj[u]){
            if(s.find(v)==s.end()){
                s.insert(v);
                DFS(v,adj,s);
            }
        }
    }
    bool isBridge(int V, vector<vector<int>> &edges, int c, int d) {
        vector<vector<int>>adj(V);
        for(auto arr:edges){
            if(arr[0]==c && arr[1]==d)
            continue;
            else{
                adj[arr[0]].push_back(arr[1]);
                adj[arr[1]].push_back(arr[0]);
            }
        }
        set<int>s;
        DFS(c,adj,s);
        return (s.find(d)==s.end());
    }
};
```

### 9. Articulation Point - II
You are given an undirected graph with V vertices and E edges. The graph is represented as a 2D array edges[][], where each element edges[i] = [u, v] indicates an undirected edge between vertices u and v.
Your task is to return all the articulation points (or cut vertices) in the graph.
An articulation point is a vertex whose removal, along with all its connected edges, increases the number of connected components in the graph.

Note: The graph may be disconnected, i.e., it may consist of more than one connected component.
If no such point exists, return {-1}.

```cpp
class Solution {
  public:
    void DFS(int u,vector<vector<int>>&adj,int p,vector<int>&isAP,vector<int>&low
    ,vector<int>&disc,int time,set<int>&vis){
        vis.insert(u);
        low[u]=disc[u]=++time;
        int children=0;
        for(auto v:adj[u]){
            if(vis.find(v)==vis.end()){
                children++;
                vis.insert(v);
                DFS(v,adj,u,isAP,low,disc,time,vis);
                low[u]=min(low[u],low[v]);
                if(p!=-1 && low[v]>=disc[u])
                isAP[u]=1;
            }
            else if(v!=p)
                low[u]=min(low[u],disc[v]);
        }
        if(p==-1 && children>1)
        isAP[u]=1;
    }
    vector<int> articulationPoints(int V, vector<vector<int>>& edges) {
        vector<int>res,isAP(V,0),low(V,0),disc(V,0);
        vector<vector<int>>adj(V);
        set<int>vis;
        int time=0;
        for(auto arr:edges){
            adj[arr[0]].push_back(arr[1]);
            adj[arr[1]].push_back(arr[0]);
        }
        for(int i=0;i<V;i++)
            DFS(i,adj,-1,isAP,low,disc,time,vis);
        for(int i=0;i<V;i++){
            if(isAP[i])
            res.push_back(i);
        }
        if(res.empty())
        return {-1};
        return res;
    }
};
```

### 10. Minimum cost to connect all houses in a city (Kruskal MST problem)
Given a 2D array houses[][], consisting of n 2D coordinates {x, y} where each coordinate represents the location of each house, the task is to find the minimum cost to connect all the houses of the city.

The cost of connecting two houses is the Manhattan Distance between the two points (xi, yi) and (xj, yj) i.e., |xi – xj| + |yi – yj|, where |p| denotes the absolute value of p.

```cpp
class DSU{
    vector<int>parent,rank;
    public:
    DSU(int V){
        parent.resize(V,-1);
        rank.resize(V,0);
    }
    int find(int i){
        if(parent[i]==-1)
        return i;
        else
        return parent[i]=find(parent[i]);
    }
    void merge(int x,int y){
        int i=find(x);
        int j=find(y);
        if(i!=j){
            if(rank[i]>rank[j])
            parent[j]=i;
            else if(rank[j]>rank[i])
            parent[i]=j;
            else{
                parent[i]=j;
                rank[j]++;
            }
        }
    }
};
static bool compare(vector<int>&a,vector<int>&b){
    return a[2]<b[2];
} 
class Solution {
  public:
    int minCost(vector<vector<int>>& houses) {
       vector<vector<int>>edges;
       for(int i=0;i<houses.size();i++){
           for(int j=i+1;j<houses.size();j++)
               edges.push_back({i,j,abs(houses[i][0]-houses[j][0])
               +abs(houses[i][1]-houses[j][1])});
       }
       sort(edges.begin(),edges.end(),compare);
       DSU d(houses.size());
       int ans=0,count=0;
       for(auto arr:edges){
           int x=arr[0],y=arr[1],w=arr[2];
           if(d.find(x)!=d.find(y)){
               d.merge(x,y);
               ans+=w;
               count++;
           }
           if(count==houses.size()-1)
           break;
       }
       return ans;
    }
};
```

### 11. Dijkstra Algorithm
Given an undirected, weighted graph with V vertices numbered from 0 to V-1 and E edges, represented by 2d array edges[][], where edges[i]=[u, v, w] represents the edge between the nodes u and v having w edge weight.
You have to find the shortest distance of all the vertices from the source vertex src, and return an array of integers where the ith element denotes the shortest distance between ith node and source vertex src.

Note: The Graph is connected and doesn't contain any negative weight edge.

```cpp
class Solution {
  public:
    vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
        vector<vector<pair<int,int>>>adj(V);
        for(auto arr:edges){
            adj[arr[0]].push_back({arr[1],arr[2]});
            adj[arr[1]].push_back({arr[0],arr[2]});
        }
        priority_queue<pair<int,int>,vector<pair<int,int>>
        ,greater<pair<int,int>>>pq;
        vector<int>dist(V,INT_MAX);
        pq.push({0,src});
        dist[src]=0;
        while(!pq.empty()){
            int u=pq.top().second;
            pq.pop();
            for(auto pi:adj[u]){
                int v=pi.first;
                int w=pi.second;
                if(dist[v]>dist[u]+w){
                    dist[v]=dist[u]+w;
                    pq.push({dist[v],v});
                }
            }
        }
        return dist;
    }
};
```

### 12. Flood fill Algorithm
You are given a 2D grid image[][] of size n*m, where each image[i][j] represents the color of a pixel in the image. Also provided a coordinate(sr, sc) representing the starting pixel (row and column) and a new color value newColor.

Your task is to perform a flood fill starting from the pixel (sr, sc), changing its color to newColor and the color of all the connected pixels that have the same original color. Two pixels are considered connected if they are adjacent horizontally or vertically (not diagonally) and have the same original color.

```cpp
class Solution {
  public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc,
                                  int newColor) {
        queue<pair<int,int>>q;
        q.push({sr,sc});
        set<pair<int,int>>s;
        s.insert({sr,sc});
        int color=image[sr][sc];
        image[sr][sc]=newColor;
        while(!q.empty()){
            int x=q.front().first;
            int y=q.front().second;
            q.pop();
            if(x+1<image.size() && s.find({x+1,y})==s.end() && image[x+1][y]==color)
            {
                s.insert({x+1,y});
                q.push({x+1,y});
                image[x+1][y]=newColor;
            }
            if(y+1<image[0].size() && s.find({x,y+1})==s.end() && image[x][y+1]==color)
            {
                s.insert({x,y+1});
                q.push({x,y+1});
                image[x][y+1]=newColor;
            }
            if(x-1>=0 && s.find({x-1,y})==s.end() && image[x-1][y]==color)
            {
                s.insert({x-1,y});
                q.push({x-1,y});
                image[x-1][y]=newColor;
            }
            if(y-1>=0 && s.find({x,y-1})==s.end() && image[x][y-1]==color)
            {
                s.insert({x,y-1});
                q.push({x,y-1});
                image[x][y-1]=newColor;
            }
        }
        return image;
    }
};
```

### 13. Clone an Undirected Graph
Given a connected undirected graph represented by adjacency list, adjList[][] with n nodes, having a distinct label from 0 to n-1, where each adj[i] represents the list of vertices connected to vertex i.

Create a clone of the graph, where each node in the graph contains an integer val and an array (neighbors) of nodes, containing nodes that are adjacent to the current node.

class Node {
    val: integer
    neighbors: List[Node]
}

Your task is to complete the function cloneGraph( ) which takes a starting node of the graph as input and returns the copy of the given node as a reference to the cloned graph.

Note: If you return a correct copy of the given graph, then the driver code will print true; and if an incorrect copy is generated or when you return the original node, the driver code will print false.

```cpp
// struct Node {
//     int val;
//     vector<Node*> neighbors;
//     Node() {
//         val = 0;
//         neighbors = vector<Node*>();
//     }
//     Node(int _val) {
//         val = _val;
//         neighbors = vector<Node*>();
//     }
//     Node(int _val, vector<Node*> _neighbors) {
//         val = _val;
//         neighbors = _neighbors;
//     }
// };

class Solution {
  public:
    Node* cloneGraph(Node* node) {
        Node *newNode=new Node(node->val);
        queue<Node*>q1,q2;
        q1.push(node);
        q2.push(newNode);
        map<int,Node*>mp;
        mp[node->val]=newNode;
        while(!q1.empty()){
            Node*ptr=q1.front();
            q1.pop();
            Node* newptr=q2.front();
            q2.pop();
            for(auto n:ptr->neighbors){
                if(mp.find(n->val)==mp.end()){
                    mp[n->val]=new Node(n->val);
                    newptr->neighbors.push_back(mp[n->val]);
                    q1.push(n);
                    q2.push(mp[n->val]);
                }else
                    newptr->neighbors.push_back(mp[n->val]);
            }
        }
        return newNode;
    }
};
```
