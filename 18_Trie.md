### 1. Implement Trie
Implement Trie class and complete insert(), search() and isPrefix() function for the following queries :

Type 1 : (1, word), calls insert(word) function and insert word in the Trie
Type 2 : (2, word), calls search(word) function and check whether word exists in Trie or not.
Type 3 : (3, word), calls isPrefix(word) function and check whether word exists as a prefix of any string in Trie or not.

```cpp
class Trie {
  public:
    class node{
        public:
            bool end;
            node*next[26];
            node(){
                end=false;
                for(int i=0;i<26;i++)
                next[i]=NULL;
            }
    };
    node*trie;
    Trie() {
        trie=new node();
    }
    void insert(string &word) {
        node* it=trie;
        for(int i=0;i<word.size();i++){
            if(it->next[word[i]-'a']==NULL)
            it->next[word[i]-'a']=new node();
            it=it->next[word[i]-'a'];
        }
        it->end=true;
    }
    bool search(string &word) {
        node* it=trie;
        for(int i=0;i<word.size();i++){
            if(it->next[word[i]-'a']==NULL)
            return false;
            it=it->next[word[i]-'a'];
        }
        return it->end;
    }
    bool isPrefix(string &word) {
        node* it=trie;
        for(int i=0;i<word.size();i++){
            if(it->next[word[i]-'a']==NULL)
            return false;
            it=it->next[word[i]-'a'];
        }
        return true;
    }
};
```

### 2. Maximum XOR of two numbers in an array
Given an array arr[] of non-negative integers of size n. Find the maximum possible XOR between two numbers present in the array.

```cpp
class Trie{
    public:
    class node{
        public:
        node*next[2];
        node(){
            for(int i=0;i<2;i++)
            next[i]=NULL;
        }
    };
    node*trie;
    Trie(vector<int>&a){
        trie=new node();
        for(int i=0;i<a.size();i++){
            int num=a[i];
            node*curr=trie;
            for(int j=31;j>=0;j--){
                int bit=((num>>j)&1);
                if(!curr->next[bit])
                curr->next[bit]=new node();
                curr=curr->next[bit];
            }
        }
    }
    int solve(vector<int>a){
        int res=0;
        for(int i=0;i<a.size();i++){
            int curr_max=0;
            node*it=trie;
            int num=a[i];
            for(int j=31;j>=0;j--){
                int bit=((num>>j)&1)?0:1;
                if(it->next[bit]){
                    curr_max<<=1;
                    curr_max|=1;
                    it=it->next[bit];
                }else{
                    curr_max<<=1;
                    curr_max|=0;
                    it=it->next[bit?0:1];
                }
            }
            res=max(res,curr_max);
        }
        return res;
    }
};
class Solution {
  public:
    int maxXor(vector<int> &arr) {
        Trie* root=new Trie(arr);
        return root->solve(arr);
    }
};
```

### 3. Longest Valid Word with All Prefixes
Given an array of strings words[], find the longest string such that every prefix of it is also present in words[]. If multiple strings have the same maximum length, return the lexicographically smallest one.

If no such string is found, return an empty string.

```cpp
class Trie{
    public:
    class node{
        public:
        bool end;
        node*next[26];
        node(){
            end=false;
            for(int i=0;i<26;i++)
            next[i]=NULL;
        }
    };
    node* trie;
    Trie(){
        trie=new node();
    }
    void insert(string word){
        node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a'])
            it->next[word[i]-'a']=new node();
            it=it->next[word[i]-'a'];
        }
        it->end=true;
    }
    bool check_prefix(string word){
        node*it=trie;
        for(int i=0;i<word.size();i++){
            if(!it->next[word[i]-'a']->end)
            return false;
            it=it->next[word[i]-'a'];
        }
        return true;
    }  
};
class Solution {
    public:
    string longestValidWord(vector<string>& words) {
        Trie* trie=new Trie();
        for(auto word:words)
        trie->insert(word);
        string res;
        for(auto word:words){
            if((trie->check_prefix(word) && word.size()>res.size())
            || (trie->check_prefix(word) && word.size()==res.size() && word<res))
            res=word;
        }
        return res;
    }
};
```

### 4. Maximum XOR With an Element From Array
You are given an array arr[], containing non-negative integers. Additionally, you have q queries represented as a 2D array queries[][], where each query is of the form [xi , mi].
For each query, your task is to find the maximum bitwise XOR value between xi and any element in arr[] that is less than or equal to mi. In other words, for each query [xi , mi], compute: max( arr[j] XOR xi ) for all j such that arr[j]  ≤  mi .

If there is no element in arr[] that satisfies the condition arr[j]  ≤  mi , then the answer for that query should be -1.
Return an array ans[], where ans[i] represents the result of the i-th query.

```cpp
class Trie{
    public:
    class node{
        public:
        node*next[2];
        node(){
            for(int i=0;i<2;i++)
            next[i]=NULL;
        }
    };
    node* trie;
    Trie(){
        trie=new node();
    }
    void insert(int num){
        node*it=trie;
        for(int j=31;j>=0;j--){
            int bit=(num>>j)&1;
            if(!it->next[bit])
            it->next[bit]=new node();
            it=it->next[bit];
        }
    }
    int solve(int num){
        node*it=trie;
        int res=0;
        for(int j=31;j>=0;j--){
            int bit=((num>>j)&1)?0:1;
            if(it->next[bit]){
                res<<=1;
                res|=1;
                it=it->next[bit];
            }else{
                res<<=1;
                res|=0;
                it=it->next[bit?0:1];
            }
        }
        return res;
    }
};
class Solution {
  public:
    vector<int> maxXor(vector<int> &arr, vector<vector<int>> &queries) {
        sort(arr.begin(),arr.end());
        vector<vector<int>>query;
        for(int i=0;i<queries.size();i++)
        query.push_back({queries[i][1],queries[i][0],i});
        sort(query.begin(),query.end());
        Trie*trie=new Trie();
        vector<int>res(query.size());
        int j=0;
        for(int i=0;i<query.size();i++){
            while(j<arr.size() && arr[j]<=query[i][0])
            trie->insert(arr[j++]);
            if(j==0)
            res[query[i][2]]=-1;
            else
            res[query[i][2]]=trie->solve(query[i][1]);
        }
        return res;
    }
};
```
