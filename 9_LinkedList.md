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

### 3. Merge two sorted linked lists
Given the head of two sorted linked lists consisting of nodes respectively. The task is to merge both lists and return the head of the sorted merged list.

```cpp
class Solution {
  public:
    Node* sortedMerge(Node* head1, Node* head2) {
        Node*dummy=new Node(0),*ptr1=head1,*ptr2=head2;
        Node*temp=dummy;
        while(ptr1!=NULL && ptr2!=NULL){
            if(ptr1->data<ptr2->data)
            {
                temp->next=ptr1;
                temp=temp->next;
                ptr1=ptr1->next;
            }else{
                temp->next=ptr2;
                temp=temp->next;
                ptr2=ptr2->next;
            }
        }
        while(ptr1!=NULL)
        {
            temp->next=ptr1;
            temp=temp->next;
            ptr1=ptr1->next;
        }
        while(ptr2!=NULL)
        {
            temp->next=ptr2;
            temp=temp->next;
            ptr2=ptr2->next;
        }
        return dummy->next;
    }
};
```

### 4. Linked List Group Reverse
Given the head a linked list, the task is to reverse every k node in the linked list. If the number of nodes is not a multiple of k then the left-out nodes in the end, should be considered as a group and must be reversed.

```cpp
class Solution {
  public:
    Node *reverseKGroup(Node *head, int k) {
        if(!head)
        return head;
        Node*newHead=NULL,*curr=head,*tail=NULL;
        while(curr!=NULL){
            Node* groupHead=curr;
            Node* prev=NULL;
            Node* newNode=NULL;
            int cnt=0;
            while(curr!=NULL && cnt<k){
                newNode=curr->next;
                curr->next=prev;
                prev=curr;
                curr=newNode;
                cnt++;
            }
            if(newHead==NULL)
            newHead=prev;
            if(tail!=NULL)
            tail->next=prev;
            tail=groupHead;
        }
        return newHead;
    }
};
```
