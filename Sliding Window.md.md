1. at most K - longest
```cpp
int kDistinctChar(string& s, int k) {
        int n = s.size();

        int i = 0, j = 0;
        int mx = 0;
        map<char, int> mpp;

        while(j < n) {
            mpp[s[j]]++;
            while(mpp.size() > k) {
                mpp[s[i]]--;
                if(mpp[s[i]] == 0)
                mpp.erase(s[i]);
                i++;
            }

            mx = max(mx, j-i+1);
            j++;
        }

        return mx;
    }
```

2. find atmost - count
```cpp
int findAtmost(vector<int>& nums, int k) {
        int n = nums.size();
        int i = 0, j = 0;
        map<int, int> mpp;
        int cnt = 0;

        while(j < n) {
            mpp[nums[j]]++;
            while(mpp.size() > k) {
                mpp[nums[i]]--;
                if(mpp[nums[i]] == 0)
                mpp.erase(nums[i]);
                i++;
            }

            cnt += j-i+1;
            j++;
        }

        return cnt;
    }
```

to find count with exactly k -> atmost(k) - atmost(k-1)

