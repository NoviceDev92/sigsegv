1. Height Calculation (Heavily reused)
```cpp
int height(TreeNode* root) {
        if(root == NULL)
        return 0;

        return 1+max(height(root->left), heigh(root->right));
    }
```

2. Check balanced
```cpp

int height(TreeNode* root) {
        if(root == NULL)
        return 0;

        int lh = height(root->left);
        if(lh == -1)
        return -1;
        int rh = height(root->right);
        if(rh == -1)
        return -1;

        if(abs(lh-rh)>1)
        return -1;

        return 1+max(height(root->left), height(root->right));
    }
```

3. Diameter
```cpp
int mx = 0;

    int height(TreeNode* root) {
        if(root == NULL)
        return 0;

        int lh = height(root->left);
        int rh = height(root->right);
        
        mx = max(mx, lh+rh);
        return 1 + max(lh, rh);
    }

    int diameterOfBinaryTree(TreeNode* root) {
        int ht = height(root);
        return mx;
    }
```

4. Max Path Sum
```cpp
int mx = 0;
    int pathSum(TreeNode* root) {
        if(root == NULL)
        return 0;

        int lsum = max(0, pathSum(root->left));
        int rsum = max(0, pathSum(root->right));

        mx = max(mx, lsum + root->val + rsum);
        return root->val + max(lsum, rsum);
    }

    int maxPathSum(TreeNode* root) {
        if(root == NULL)
        return 0;

        mx = root->val;
        int sum = pathSum(root);
        return mx;
    }
```

5. Level Order / Zig Zag Traversal
```cpp
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;

        if(root == NULL)
        return res;

        queue<TreeNode*> q;
        q.push(root);

        int k = 0;
        while(!q.empty()) {
            int sz = q.size();
            vector<int> levels;
            
            for(int i = 0; i < sz; i++) {
                auto curr = q.front();
                q.pop();
                
                if(curr->left)
                q.push(curr->left);
                if(curr->right)
                q.push(curr->right);
                
                levels.push_back(curr->val);
            }
            
            if(k++ % 2) {
                reverse(levels.begin(), levels.end());
            }
            res.push_back(levels);
        }

        return res;
    }
```

For only level order, no need to reverse.

6. Boundary Traversal
```cpp
bool isLeaf(Node* root) {
        return root != NULL && root->left == NULL && root->right == NULL;
    }
    
    void printLeft(Node* root, vector<int>& res) {
        auto curr = root->left;
        while(curr) {
            if(!isLeaf(curr))
            res.push_back(curr->data);
            
            if(curr->left)
            curr = curr->left;
            else
            curr = curr->right;
        }
    }
    
    void printLeaf(Node* root, vector<int>& res) {
        if(isLeaf(root)) {
            res.push_back(root->data);
            return;
        }
        
        if(root->left)
        printLeaf(root->left, res);
        if(root->right)
        printLeaf(root->right, res);
    }
    
    void printRight(Node* root, vector<int>& res) {
        vector<int> temp;
        auto curr = root->right;
        while(curr) {
            if(!isLeaf(curr))
            temp.push_back(curr->data);
            
            if(curr->right)
            curr = curr->right;
            else
            curr = curr->left;
        }
        
        for(int i = temp.size()-1; i >= 0; i--) {
            res.push_back(temp[i]);
        }
    }
    
    vector<int> boundaryTraversal(Node *root) {
        // code here
        vector<int> res;
        if(root == NULL)
        return res;
        
        if(!isLeaf(root))
        res.push_back(root->data);
        
        printLeft(root, res);
        printLeaf(root, res);
        printRight(root, res);
        
        return res;
    }
```


