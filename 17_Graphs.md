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
    bool isCycle(int V, vector<vector<int>>& edges) {
        vector<vector<int>>adj(V);
        for(auto item:edges)
        {
            int u=item[0],v=item[1];
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        for(int i=0;i<V;i++){
            queue<pair<int,int>>q;
            set<int>s;
            q.push({i,-1});
            s.insert(i);
            while(!q.empty()){
                int u=q.front().first,p=q.front().second;
                q.pop();
                for(auto v:adj[u]){
                    if(s.find(v)!=s.end() && v!=p)
                    return true;
                    else if(s.find(v)!=s.end())
                    continue;
                    else{
                        q.push({v,u});
                        s.insert(v);
                    }
                }
            }
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
