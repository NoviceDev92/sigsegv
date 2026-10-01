1. Knapsack Classic - Subsequence Sum
```cpp
bool canPartition(vector<int>& nums, int k) {
    vector<bool> dp(k + 1, false);
    dp[0] = true;  // empty subset makes sum 0

    for (int num : nums) {
        for (int j = k; j >= num; j--) {   // backward!
            dp[j] = dp[j] || dp[j - num];// to count : just replace || with +
            //to also print, make vec<vec<vec<int>>> dp(target+1)
            //rest same, iterate through auto comb : dp[j-ele]
            //newComb = comb, newComb->add ele, dp[j]->push newComb
        }
    }

    return dp[k];
}
```


2. Target Sum- Choose + or -
```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for(int ele : nums) {
            sum += ele;
        }
        int n = nums.size();

        vector<vector<int>> dp(n, vector<int>(2*sum+1));
        dp[0][sum+nums[0]] = 1;
        dp[0][sum-nums[0]] += 1;
        for(int i = 1; i < n; i++) {
            for(int j = 0; j <= 2*sum; j++) {
                if(j+nums[i] <= 2*sum)
                dp[i][j+nums[i]] += dp[i-1][j];
                if(j-nums[i] >= 0)
                dp[i][j-nums[i]] += dp[i-1][j];
            }
        }
        return (abs(target) > sum) ? 0 : dp[n-1][sum+target];
    }
};
```

Very similar : LC 1049, assign + or -, try to get largest sum <= total_sum/2, ans = sum - 2*(mx)

3. Tracking more states - 2D knapsack
```cpp
int findMaxForm(vector<string>& strs, int m, int n) {
        int sz = strs.size();
        vector<pair<int, int>> v;
        for(auto s : strs) {
            int cnt = 0;

            int len = s.size();
            for(char c : s) {
                if(c == '0')
                cnt++;
            }
            v.push_back({cnt, len-cnt});
        }

        vector<vector<int>> dp(m+1, vector<int>(n+1));
        // dp[0][0] = 1;
        for(int i = 0; i < sz; i++) {
            int zeros = v[i].first;
            int ones = v[i].second;

            for(int j = m; j >= zeros; j--) {
                for(int k = n; k >= ones; k--) {
                    dp[j][k] = max(dp[j][k], dp[j-zeros][k-ones] + 1);
                }
            }
        }
        return dp[m][n];
    }
```

4. Coin Unbounded
```cpp
int coinChange(vector<int>& coins, int amount) {

        if(amount == 0)
        return 0;

        vector<int> dp(amount+1, INF);
        dp[0] = 0;

        for(int ele : coins) {
            for(int j = ele; j <= amount; j++) {
                if(dp[j-ele] != INF)
                dp[j] = min(dp[j], dp[j-ele] + 1);
            }
        }

        return dp[amount] == INF ? -1 : dp[amount];
    }
```
For bounded, just iterate backwards.

5. LIS-ish Upper Bound Based Pull DP(LC 1235)
```cpp
int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        vector<vector<int>> v;
        int n = startTime.size();

        for(int i = 0; i < n; i++) {
            v.push_back({endTime[i], startTime[i], profit[i]});
        }
        sort(v.begin(), v.end());

        for(int i = 0; i < n; i++) {
            endTime[i] = v[i][0];
        }
        
        vector<int> dp(n+1);
        for(int i = 1; i <= n; i++) {
            int start = v[i-1][1];
            int profit = v[i-1][2];
            int idx = upper_bound(endTime.begin(), endTime.end(), start) - endTime.begin();
            dp[i] = max(dp[i-1], dp[idx] + profit);
        }
        return dp[n];
    }
```

6. LIS - Optimised
```cpp
int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> temp;
        temp.push_back(nums[0]);
        for(int i = 1; i < n; i++) {
            if(nums[i] <= temp.back()) {
                int idx = lower_bound(temp.begin(), temp.end(), nums[i]) - temp.begin();
                temp[idx] = nums[i];
            } else {
                temp.push_back(nums[i]);
            }
        }
        return temp.size();
    }
```
If required to discuss brute, use dp array, iterate through j from 0 to i-1, dp[i] = max(dp[i], dp[j] + 1)

To count number of LIS, create a count vector, update when equal, else reset to j if greater found(will update in detail later).

7. Prefix Count with a Choice on Operations
Suppose you need to find a prefix for which you need to perform say either of two operations, brute way to represent :
```cpp
dp[i][j][k] = "can cover first i elements using j health and k money"
```
Naturally, cubic might be too tight to fit. So we can always shrink states to make the k as the value of the dp, and minimise it. 
**It is always optimal to minimise,**
```cpp
dp[3][5][5] = true
dp[3][5][8] = true
```
in no case, will using  8 over 5 be optimal.

So, we represent as :
```cpp
dp[i][j] = "min k(money) to cover i elements using j health"
```

Use a boolean to check, if any i not possible, **immediately break** and report that i as the answer.