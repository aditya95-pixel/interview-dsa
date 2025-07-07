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
