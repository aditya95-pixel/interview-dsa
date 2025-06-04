### 1. Reverse a linked list
Given the head of a linked list, the task is to reverse this list and return the reversed head.

```cpp
class Solution {
  public:
    Node* reverseList(struct Node* head) {
        Node*p=NULL,*q=NULL,*r=head;
        while(r!=NULL){
            p=q;
            q=r;
            r=r->next;
            q->next=p;
        }
        return q;
    }
};
```

### 2. Rotate a Linked List
Given the head of a singly linked list, your task is to left rotate the linked list k times.

```cpp
class Solution {
  public:
  int length(Node*head){
      Node*ptr=head;
      int len=0;
      while(ptr!=NULL)
      {
          len++;
          ptr=ptr->next;
      }
      return len;
  }
    Node* rotate(Node* head, int k) {
        Node*dummy=new Node(0),*ptr=head,*prevptr=NULL;
        int len=length(head);
        if(len==1)
        return head;
        k%=len;
        if(k==0)
        return head;
        int cnt=0;
        ptr=head;
        while(ptr!=NULL && cnt<k){
            cnt++;
            prevptr=ptr;
            ptr=ptr->next;
        }
        dummy->next=ptr;
        prevptr->next=NULL;
        while(ptr->next!=NULL)
        ptr=ptr->next;
        ptr->next=head;
        return dummy->next;
    }
};
```
