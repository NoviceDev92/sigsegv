1. Detect and print cycle in undirected
```cpp
int shuru = -1, sesh = -1;
vector<int> vis, parent;

bool dfs(vector<vector<int>>& adj, int vertex, int par = -1) {
    // entering vertex

    vis[vertex] = 1;
    parent[vertex] = par;

    for (auto& child : adj[vertex]) {

        if (child == par)
            continue;

        // entering child from vertex

        if (vis[child]) {
            shuru = child;
            sesh = vertex;
            return true;
        } else {
            if (dfs(adj, child, vertex))
                return true;
        }

        // leaving child back to vertex
    }

    return false;

    // leaving vertex back to parent
}

void solve() {
    // if cannot optimise, maybe brute isnt allat bad
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adj(n + 1);

    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;

        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    vis.assign(n + 1, 0);
    parent.assign(n + 1, -1);

    for (int i = 1; i <= n; i++) {
        if (!vis[i] && dfs(adj, i, -1))
            break;
    }

    if (shuru == -1) {
        cout << "IMPOSSIBLE\n";
    } else {
        vector<int> cycle;

        cycle.push_back(shuru);

        int curr = sesh;

        while (curr != shuru) {
            cycle.push_back(curr);
            curr = parent[curr];
        }

        cycle.push_back(shuru);

        reverse(all(cycle));

        cout << cycle.size() << "\n";

        for (int ele : cycle) {
            cout << ele << " ";
        }

        cout << "\n";
    }

    // YOU CAN IMPLEMENT ANYTHING, YOU CAN DEBUG ANYTHING
}
```

2. BFS + Time stuff + Print
```cpp
bool isValid(int i, int j, int n, int m) {
    return (i >= 0 && i < n && j >= 0 && j < m);
}

vector<int> hor{-1, 1, 0, 0};
vector<int> vert{0, 0, -1, 1};

void solve() {
    // if cannot optimise, maybe brute isnt allat bad
    int n, m;
    cin >> n >> m;

    vector<vector<char>> grid(n, vector<char>(m));
    queue<pair<int, int>> q;
    vector<vector<int>> dist(n, vector<int>(m, INFF));

    int sr = -1, sc = -1;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];

            if (grid[i][j] == 'M') {
                q.push({i, j});
                dist[i][j] = 0;
            }

            if (grid[i][j] == 'A') {
                sr = i;
                sc = j;
            }
        }
    }

    map<int, char> mpp;
    mpp[0] = 'U';
    mpp[1] = 'D';
    mpp[2] = 'L';
    mpp[3] = 'R';

    while (!q.empty()) {
        auto p = q.front();
        q.pop();

        int r = p.first;
        int c = p.second;

        for (int k = 0; k < 4; k++) {
            int nr = r + hor[k];
            int nc = c + vert[k];

            if (!isValid(nr, nc, n, m))
                continue;

            if (grid[nr][nc] == '#')
                continue;

            if (dist[r][c] + 1 < dist[nr][nc]) {
                dist[nr][nc] = dist[r][c] + 1;
                q.push({nr, nc});
            }
        }
    }

    vector<char> moves;
    int er = -1, ec = -1;

    queue<pair<int, int>> q2;
    q2.push({sr, sc});

    vector<vector<int>> ops(n, vector<int>(m, -1));
    vector<vector<int>> dist2(n, vector<int>(m, INFF));
    dist2[sr][sc] = 0;

    while (!q2.empty()) {
        auto p = q2.front();
        q2.pop();

        int r = p.first;
        int c = p.second;

        if (r == 0 || r == n - 1 || c == 0 || c == m - 1) {
            er = r;
            ec = c;
            break;
        }

        bool b = false;

        for (int k = 0; k < 4; k++) {
            int nr = r + hor[k];
            int nc = c + vert[k];

            if (!isValid(nr, nc, n, m) || grid[nr][nc] == '#')
                continue;

            if (dist2[r][c] + 1 < dist[nr][nc] &&
                dist2[r][c] + 1 < dist2[nr][nc]) {

                dist2[nr][nc] = dist2[r][c] + 1;
                q2.push({nr, nc});
                ops[nr][nc] = k;
            }
        }
    }

    if (er == -1 || ec == -1) {
        cout << "NO\n";
    } else {
        cout << "YES\n";

        string s = "";

        int r = er, c = ec;

        while (r != sr || c != sc) {
            int k = ops[r][c];

            s += mpp[k];

            r -= hor[k];
            c -= vert[k];
        }

        reverse(all(s));

        cout << s.size() << "\n";
        cout << s << "\n";
    }

    // YOU CAN IMPLEMENT ANYTHING, YOU CAN DEBUG ANYTHING
}
```


