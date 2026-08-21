1. Remove K Digits(LC 402)

```cpp
class Solution {

public:

    string removeKdigits(string num, int k) {

        vector<char> st;

        for(char c : num) {

            while(!st.empty() && c < st[st.size()-1] && k > 0) {

                st.pop_back();

                k--;

            }

            st.push_back(c);

        }

  

        string res;

        while(!st.empty() && k > 0) {

            st.pop_back();

            k--;

        }

  

        bool b = false;

        for(char c : st) {

            if(!b && c != '0')

            b = true;

  

            if(b || c != '0')

            res += c;

        }

  

        return res == "" ? "0" : res;

    }

};
```

Same idea for LMS(Lex Min Subseq), maybe of size k, or k removals.

2. Asteroid Collision
```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        vector<int> st;
        for (int ast : asteroids) {
            if (ast > 0) {
                st.push_back(ast);
            } else {
                while (!st.empty() && st.back() > 0 && st.back() < abs(ast)) {
                    st.pop_back();
                }
                if (!st.empty() && st.back() > 0 && st.back() == abs(ast)) {
                    st.pop_back();
                } else if (st.empty() || st.back() < 0) {
                    st.push_back(ast);
                }
            }
        }
        return st;
    }
};
```

3. Maximum Rectangle in Histogram
```cpp
int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        stack<int> st;

        int mx = 0;
        for(int i = 0; i < n; i++) {
            while(!st.empty() && heights[st.top()] >= heights[i]) {// careful of = condition, will give TLE otherwise
                int temp = st.top();
                st.pop();
                int nse = i;
                int pse = st.empty() ? -1 : st.top();
                mx = max(mx, (nse-pse-1) * heights[temp]);
            }
            st.push(i);
        }

          while(!st.empty()) {
            int temp = st.top();
            st.pop();
            int nse = n;
            int pse = st.empty() ? -1 : st.top();
            mx = max(mx, (nse-pse-1) * heights[temp]);
        }
        return mx;
    }
```

4. Maximal Rectangle in Grid
	Extension of same idea, convert grid to histograms
```cpp
int maximalRectangle(vector<vector<char>>& matrix) {
        int n = matrix.size();
        int m = matrix[0].size();
        vector<vector<int>> pref(n, vector<int>(m, 0));

        for(int j = 0; j < m; j++) {
            for(int i = 0; i < n; i++) {
                if(matrix[i][j] == '0')
                pref[i][j] = 0;
                else
                pref[i][j] = (i == 0) ? 1 : pref[i-1][j] + 1;
            }
        }
        int mx = 0;
        for(int i = 0; i < n; i++) {
            stack<int> st;
            for(int j = 0; j < m; j++) {
                while(!st.empty() && pref[i][st.top()] > pref[i][j]) {
                    int temp = st.top();
                    st.pop();
                    int nse = j;
                    int pse = st.empty() ? -1 : st.top();
                    mx = max(mx, (nse-pse-1) * pref[i][temp]);
                }
                st.push(j);
            }
            while(!st.empty()) {
                int temp = st.top();
                st.pop();
                int nse = m;
                int pse = st.empty() ? -1 : st.top();
                mx = max(mx, (nse-pse-1) * pref[i][temp]);
            }
        }
        return mx;
    }
```
