1. Merge 2 sorted lists
```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* curr1 = list1;
        ListNode* curr2 = list2;
        ListNode* res = new ListNode(-1);
        ListNode* curr = res;//keep 2 extra, one to traverse, one for head
        while(curr1 != NULL && curr2 != NULL) {
            if(curr1->val < curr2->val) {
                curr->next = curr1;
                curr1 = curr1->next;
            }
            else {
                curr->next = curr2;
                curr2 = curr2->next;
            }
            curr = curr->next;
        }
        if(curr1 != NULL) {
            curr->next = curr1;
        } else if(curr2 != NULL) {
            curr->next = curr2;
        }
        return res->next;
    }
```

2. Merge k sorted lists
```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* res = new ListNode(-1);
        ListNode* curr = res;

        priority_queue<pair<int, ListNode*>, vector<pair<int, ListNode*>>, greater<pair<int, ListNode*>>> pq;
        int n = lists.size();

        for(int i = 0; i < n; i++) {
            if(lists[i])
            pq.push({lists[i]->val, lists[i]});
        }
        
        while(!pq.empty()) {
            auto p = pq.top();
            pq.pop();

            curr->next = p.second;
            curr = curr->next;

            if(p.second->next) {
                pq.push({p.second->next->val, p.second->next});
            }
        }

        return res->next;
    }
```
