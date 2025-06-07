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

### 5. Add Number Linked Lists
Given the head of two singly linked lists num1 and num2 representing two non-negative integers. The task is to return the head of the linked list representing the sum of these two numbers.

For example, num1 represented by the linked list : 1 -> 9 -> 0, similarly num2 represented by the linked list: 2 -> 5. Sum of these two numbers is represented by 2 -> 1 -> 5.

Note: There can be leading zeros in the input lists, but there should not be any leading zeros in the output list.

```cpp
class Solution {
  public:
    Node* reverse(Node*head){
        Node*p=NULL,*q=NULL,*r=head;
        while(r!=NULL){
            p=q;
            q=r;
            r=r->next;
            q->next=p;
        }
        return q;
    }
    Node* addTwoLists(Node* num1, Node* num2) {
        Node*head1=reverse(num1),*head2=reverse(num2);
        int carry=0;
        Node*ptr1=head1,*ptr2=head2,*dummy=new Node(0);
        Node* ptr3=dummy;
        while(ptr1!=NULL && ptr2!=NULL){
            int sum=ptr1->data+ptr2->data+carry;
            if(sum>=10)
            carry=1;
            else
            carry=0;
            Node* newNode=new Node(sum%10);
            ptr3->next=newNode;
            ptr3=ptr3->next;
            ptr1=ptr1->next;
            ptr2=ptr2->next;
        }
        while(ptr1!=NULL){
            int sum=ptr1->data+carry;
            if(sum>=10)
            carry=1;
            else
            carry=0;
            Node* newNode=new Node(sum%10);
            ptr3->next=newNode;
            ptr3=ptr3->next;
            ptr1=ptr1->next;
        }
        while(ptr2!=NULL){
            int sum=ptr2->data+carry;
            if(sum>=10)
            carry=1;
            else
            carry=0;
            Node* newNode=new Node(sum%10);
            ptr3->next=newNode;
            ptr3=ptr3->next;
            ptr2=ptr2->next;
        }
        if(carry==1)
        {
            Node* newNode=new Node(1);
            ptr3->next=newNode;
        }
        Node *res=reverse(dummy->next);
        while(res!=NULL && res->data==0)
        res=res->next;
        return res;
    }
};
```

### 6. Clone List with Next and Random
You are given a special linked list with n nodes where each node has two pointers a next pointer that points to the next node of the singly linked list, and a random pointer that points to the random node of the linked list.

Construct a copy of this linked list. The copy should consist of the same number of new nodes, where each new node has the value corresponding to its original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list, such that it also represent the same list state. None of the pointers in the new list should point to nodes in the original list.

Return the head of the copied linked list.

NOTE : Original linked list should remain unchanged.

```cpp
class Solution {
  public:
    Node *cloneLinkedList(Node *head) {
        Node*ptr=head;
        while(ptr){
            Node*newNode=new Node(ptr->data);
            newNode->next=ptr->next;
            ptr->next=newNode;
            ptr=ptr->next->next;
        }
        ptr=head;
        while(ptr){
            if(ptr->random)
            ptr->next->random=ptr->random->next;
            ptr=ptr->next->next;
        }
        ptr=head;
        Node*newhead=head->next;
        Node*ptr1=newhead;
        while(ptr1->next){
            ptr->next=ptr->next->next;
            ptr1->next=ptr1->next->next;
            ptr=ptr->next;
            ptr1=ptr1->next;
        }
        ptr->next=NULL;
        ptr1->next=NULL;
        return newhead;
    }
};
```

### 7. Detect Loop in linked list
You are given the head of a singly linked list. Your task is to determine if the linked list contains a loop. A loop exists in a linked list if the next pointer of the last node points to any other node in the list (including itself), rather than being null.

Custom Input format:
A head of a singly linked list and a pos (1-based index) which denotes the position of the node to which the last node points to. If pos = 0, it means the last node points to null, indicating there is no loop.

```cpp
class Solution {
  public:
    // Function to check if the linked list has a loop.
    bool detectLoop(Node* head) {
        Node*fast=head,*slow=head;
        do{
            fast=fast->next;
            slow=slow->next;
            if(fast && fast->next)
            fast=fast->next;
            else
            return false;
        }while(fast->next && fast!=slow);
        return (fast==slow);
    }
};
```

### 8. Find the first node of loop in linked list
Given a head of the singly linked list. If a loop is present in the list then return the first node of the loop else return NULL.

Custom Input format:
A head of a singly linked list and a pos (1-based index) which denotes the position of the node to which the last node points to. If pos = 0, it means the last node points to null, indicating there is no loop.

```cpp
class Solution {
  public:
    Node* findFirstNode(Node* head) {
        Node*slow=head,*fast=head;
        do{
            fast=fast->next;
            slow=slow->next;
            if(fast && fast->next)
            fast=fast->next;
            else
            return NULL;
        }while(fast->next && fast!=slow);
        if(fast==slow)
        {
            slow=head;
            while(fast!=slow)
            {
                fast=fast->next;
                slow=slow->next;
            }
            return slow;
        }else
        return NULL;
    }
};
```

### 9. Remove loop in Linked List
Given the head of a linked list that may contain a loop.  A loop means that the last node of the linked list is connected back to a node in the same list. The task is to remove the loop from the linked list (if it exists).

Custom Input format:

A head of a singly linked list and a pos (1-based index) which denotes the position of the node to which the last node points to. If pos = 0, it means the last node points to null, indicating there is no loop.

The generated output will be true if there is no loop in list and other nodes in the list remain unchanged, otherwise, false.

```cpp
class Solution {
  public:
    // Function to remove a loop in the linked list.
    void removeLoop(Node* head) {
        Node*fast=head,*slow=head;
        do{
            fast=fast->next;
            slow=slow->next;
            if(fast && fast->next)
            fast=fast->next;
            else
            return ;
        }while(fast->next && fast!=slow);
        if(fast==slow)
        {
            slow=head;
            if(fast==slow)
            {
                while(fast->next!=slow)
                    fast=fast->next;
                fast->next=NULL;
                return ;
            }
            while(fast->next!=slow->next)
            {
                fast=fast->next;
                slow=slow->next;
            }
            fast->next=NULL;
            return ;
        }
        else
        return ;
    }
};
```
