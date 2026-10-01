1. Point Update (sum / max / min)
```cpp
struct item {
    int sum;
};

	item neutral = {0};// switch to INF for min

struct segtree {
    int sz;
    vector<item> vals;

    void init(int n) {
        sz = 1;
        while(sz < n)
        sz *= 2;

        vals.assign(2*sz, neutral);
    }

    item single(int v) {
        if(v > 0)
        return {v};
        else
        return {v};
    }
    
    item merge(item a, item b) {
        return {
            a.sum + b.sum //switch to max(a.val, b.val) OR min(a.val, b.val)
        };
    }

    void build(vector<int>& a, int x, int lx, int rx) {
        if(rx - lx == 1) {
            if(lx < (int)a.size())
            vals[x] = single(a[lx]);
            
            return;
        } else {
            int m = lx + (rx - lx) / 2;
            build(a, 2*x+1, lx, m);
            build(a, 2*x+2, m, rx);
        }

        vals[x] = merge(vals[2*x+1], vals[2*x+2]);
    }

    void build(vector<int>& a) {
        init(a.size());
        build(a, 0, 0, sz);
    }

    void set(int i, int v, int x, int lx, int rx) {
        if(rx-lx == 1) {
            vals[x] = single(v);
            return;
        }

        int m = lx + (rx-lx)/2;

        if(i < m)
            set(i, v, 2*x+1, lx, m);
        else
            set(i, v, 2*x+2, m, rx);

        vals[x] = merge(vals[2*x+1], vals[2*x+2]);
    }

    void set(int i, int v) {
        set(i, v, 0, 0, sz);

    }

    item query(int l, int r, int x, int lx, int rx) {
        if(rx <= l || lx >= r)
        return neutral;

        if(l <= lx && r >= rx)
        return vals[x];

        int m = lx + (rx-lx)/2;
        item i1 = query(l, r, 2*x+1, lx, m);
        item i2 = query(l, r, 2*x+2, m, rx);
        return merge(i1, i2);
    }

    item query(int l, int r) {
        return query(l, r, 0, 0, sz);
    }
};
```

2. Special type : Find max subarray sum in [l, r) with updates
```cpp
struct item {
    int seg, pref, suff, sum;
};

item neutral = {0, 0, 0, 0};

struct segtree {
    int sz;
    vector<item> vals;

    void init(int n) {
        sz = 1;
        while(sz < n)
        sz *= 2;

        vals.assign(2*sz, neutral);
    }

    item single(int v) {
        if(v > 0)
        return {v, v, v, v};
        else
        return {0, 0, 0, v};
    }
    
    item merge(item a, item b) {
        return {
            max({a.seg, b.seg, a.suff+b.pref}),
            max(a.pref, a.sum+b.pref),
            max(b.suff, b.sum+a.suff),
            a.sum + b.sum
        };
    }

    void build(vector<int>& a, int x, int lx, int rx) {
        if(rx - lx == 1) {
            if(lx < (int)a.size())
            vals[x] = single(a[lx]);
            
            return;
        } else {
            int m = lx + (rx - lx) / 2;
            build(a, 2*x+1, lx, m);
            build(a, 2*x+2, m, rx);
        }

        vals[x] = merge(vals[2*x+1], vals[2*x+2]);
    }

    void build(vector<int>& a) {
        init(a.size());
        build(a, 0, 0, sz);
    }

    void set(int i, int v, int x, int lx, int rx) {
        if(rx-lx == 1) {
            vals[x] = single(v);
            return;
        }

        int m = lx + (rx-lx)/2;

        if(i < m)
            set(i, v, 2*x+1, lx, m);
        else
            set(i, v, 2*x+2, m, rx);

        vals[x] = merge(vals[2*x+1], vals[2*x+2]);
    }

    void set(int i, int v) {
        set(i, v, 0, 0, sz);

    }

    item query(int l, int r, int x, int lx, int rx) {
        if(rx <= l || lx >= r)
        return neutral;

        if(l <= lx && r >= rx)
        return vals[x];

        int m = lx + (rx-lx)/2;
        item i1 = query(l, r, 2*x+1, lx, m);
        item i2 = query(l, r, 2*x+2, m, rx);
        return merge(i1, i2);
    }

    item query(int l, int r) {
        return query(l, r, 0, 0, sz);
    }
};
```

3. Special Type : Kth one(Highly reused)
