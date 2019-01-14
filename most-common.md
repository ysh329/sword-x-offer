# most-common

## 1. reverse-linked-list

```shell
#include <bits/stdc++.h>
using namespace std;
struct node {
    node* next;
    int val;
};

node* reverseList1(node* head) {
    if(!head -> next || !head) return head;
    node* tmp = reverseList1(head -> next);
    head -> next -> next = head;
    head -> next = NULL;
    return tmp;
}

node* reverseList2(node* head) {
    if(head -> next == NULL || head == NULL) return head;
    node* tmp = NULL;
    while(head) {
        node* nxt = head -> next;
        head -> next = tmp;
        tmp = head;
        head = nxt;
    }
    return tmp;
}
void printOut(node* head) {
    while(head) {
        cout << head -> val << " ";
        head = head -> next;
    }
    cout << endl;
}
int main() {
    //freopen("in.txt", "r", stdin);
    node* head = new node();
    node* a = head;
    int n, x;
    cin >> n;
    for(int i = 0; i < n; i++) {
        cin >> x;
        a -> next = new node();
        a = a -> next;
        a -> val = x;
        a -> next = NULL;
    }
    //node* out1 = reverseList1(head -> next);
    node* out2 = reverseList2(head -> next);
    //printOut(out1);
    printOut(out2);
    return 0;
}
```
