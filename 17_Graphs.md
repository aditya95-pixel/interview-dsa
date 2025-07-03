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
