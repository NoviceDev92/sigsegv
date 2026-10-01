1. CSES Tree Matching
```cpp
vector<vector<int>> dp;
//for tree, use par, otherwise need vis
void dfs(vector<vector<int>>& adj, int vertex, int par = -1) {
    //entering vertex
    vector<int> pref, suff;
    bool isLeaf = true;
    for(int child : adj[vertex]) {
        if(child == par)
        continue;
       
        //entering child from vertex
        isLeaf = false;
        dfs(adj, child, vertex);

        //leaving child back to vertex
    }

    if(isLeaf)
    return;

    dp[vertex][0] = dp[vertex][1] = 0;

    for(int child : adj[vertex]) {
        if(child == par)
        continue;

        pref.push_back(max(dp[child][0], dp[child][1]));
        suff.push_back(max(dp[child][0], dp[child][1]));
    }
    
    for(int i = 1; i < pref.size(); i++) {
        pref[i] = pref[i-1] + pref[i];
    }

    for(int i = suff.size()-2; i >= 0; i--) {
        suff[i] = suff[i] + suff[i+1];
    }

    dp[vertex][0] = suff[0];
    int temp = 0;
    for(int child : adj[vertex]) {
        if(child == par)
        continue;
        
        int lch = (temp == 0) ? 0 : pref[temp-1];
        int rch = (temp == suff.size()-1) ? 0 : suff[temp+1];
        dp[vertex][1] = max(dp[vertex][1], 1+lch+dp[child][0]+rch);
        temp++;
    }
    //leaving vertex back to parent

}

  

void solve()

{
    //if cannot optimise, maybe brute isnt allat bad
    int n;
    cin >> n;

    dp.resize(n+1, vector<int>(2));
    vector<vector<int>> adj(n+1);

    for(int i = 2; i <= n; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    dfs(adj, 1);
    cout << max(dp[1][0], dp[1][1]) << "\n";
    //YOU CAN IMPLEMENT ANYTHING, YOU CAN DEBUG ANYTHING
}
```

2. Diameter / Longest Path
In general, longest path including any node MUST include atleast one of the endpoints of diameter

```cpp
vector<int> depth;
void dfs(vector<vector<int>>& adj, int vertex, int par = -1) {
    //entering vertex
    for(int child : adj[vertex]) {
        if(child == par)
        continue;

        //entering child from vertex
        depth[child] = depth[vertex] + 1;
        dfs(adj, child, vertex);
        //leaving child back to vertex
    }
    //leaving vertex back to parent
}

void dfs(vector<vector<int>>& adj, vector<int>& depth, int vertex, int par = -1) {
    for(int child : adj[vertex]) {
        if(child == par)
        continue;

        depth[child] = depth[vertex] + 1;
        dfs(adj, depth, child, vertex);
    }
}

void solve()
{
    //if cannot optimise, maybe brute isnt allat bad
    int n;
    cin >> n;

    vector<vector<int>> adj(n+1);
    depth.resize(n+1);
    vector<int> temp(n+1);

    for(int i = 2; i <= n; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    dfs(adj, 1);
    int mx = 0;
    int u = 1;

    for(int i = 1; i <= n; i++) {
        if(depth[i] > mx) {
            mx = depth[i];
            u = i;
        }
    }
    depth = temp;
    dfs(adj, u);
    mx = 0;
    int v = 0;

    for(int i = 1; i <= n; i++) {
        if(depth[i] > mx) {
            mx = depth[i];
            v = i;
        }
    }

    vector<int> depth1(n+1), depth2(n+1);
    dfs(adj, depth1, u);
    dfs(adj, depth2, v);

    for(int i = 1; i <= n; i++) {
        cout << max(depth1[i], depth2[i]) << " ";
    }
    cout << "\n";
    //YOU CAN IMPLEMENT ANYTHING, YOU CAN DEBUG ANYTHING
}
```
