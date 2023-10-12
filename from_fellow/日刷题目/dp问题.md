## 动态规划

### 1、最长递增子序列

[300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

经典动态规划，可以把O（N2）的二维循环，通过贪心+单调栈的方式降低到O(NlogN)。

基于这样一种贪心思想：以当前元素为结尾的最长递增子序列为基础，碰到了比末尾元素大的，可以直接加进来，让结果+1。碰到了比末尾元素小的，我们就尽可能的让当前递增序列中靠前位置的数字变小，这样有利于后面的数字进来时，让结果尽可能大。

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> now_num;
        now_num.push_back(nums[0]);
        for(int i = 1; i < nums.size(); i++) {
            if(nums[i] > now_num.back()) {
                now_num.push_back(nums[i]);
            } else {
                auto it = lower_bound(now_num.begin(), now_num.end(), nums[i]);
                *it = nums[i];
            }
        }
        return now_num.size();
    }
};
```

### 2、完全背包问题

01背包的状态转移公式：对于前i个物体，大小为j的背包，能装的最大价值：

- 如果不选择第i个物体，$dp[i][j] = dp[i - 1][j]$;
- 如果选择第i个物体，$dp[i][j] = dp[i - 1][j - w_i] + v_i$;

选择最大值，结果就是：$dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w_i] + v_i)$；

对于完全背包问题：

- 如果不选择第i个物体，$dp[i][j] = dp[i - 1][j]$;
- 如果选择1个第i个物体，$dp[i][j] = dp[i - 1][j - w_i] + v_i$;
- 如果选择k个第i个物体，$dp[i][j] = dp[i - 1][j -  k * w_i] + k * v_i$;

$dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w_i] + v_i, … , dp[i - 1][j - k * w_i] + k * v_i)$;

如果这样写，这个公式太长了，可以简化一下。

$dp[i][j - w_i] = max(dp[i - 1][j - w_i], dp[i - 1][j -  2w_i] + v_i, … , dp[i - 1][j - k * w_i] + (k - 1) * v_i)$;

会发现原先公式里有很大一部分都可以用dp\[i][j - wi]表示，只剩下：$dp[i - 1][j]$，以及每项都少了一个$v_i$，在公式外面加上即可。公式转换为：

$dp[i][j] = max(dp[i - 1][j], dp[i][j - w_i] + v_i)$;

[322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

```cpp
int coinChange(vector<int>& coins, int amount) {
        int n = coins.size();
        vector<vector<long long>> dp(n + 1, vector<long long>(amount + 1, INT_MAX));
        for(int i = 0; i <= n; i++) {
            dp[i][0] = 0;
        }
        for(int i = 1; i <= n; i++) {
            int now_weight = coins[i - 1];
            for(int j = 1; j <= amount; j++) {
                if(j - now_weight >= 0) {
                    dp[i][j] = min(dp[i - 1][j], dp[i][j - now_weight] + 1);
                }else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[n][amount] == INT_MAX ? -1 : dp[n][amount];
    }
```

[279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

```cpp
int numSquares(int n) {
        vector<vector<int>> dp(110, vector<int>(n + 1, INT_MAX));
        int i;
        dp[0][0] = 0;
        for(i = 1; i * i <= n; i++) {
            int now_weight = i * i;
            dp[i][0] = 0;
            for(int j = 1; j <= n; j++) {
                if(j - now_weight >= 0) {
                    dp[i][j] = min(dp[i - 1][j], dp[i][j - now_weight] + 1);
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[i - 1][n];
    }
```

### 3、单词拆分动态规划

[139. 单词拆分](https://leetcode.cn/problems/word-break/)

使用两个指针，不断截取子段，如果该子段前面的状态是合法的，并且该子段在wordDict存在，那么这个子段的结尾位置就合法。

```cpp
bool wordBreak(string s, vector<string>& wordDict) {
        int n = s.size();
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for(int j = 1; j <= n; j++) {
            for(int i = 0; i < j; i++) {
                if(dp[i]) {
                    string now_s = s.substr(i, j - i);
                    if(count(wordDict.begin(), wordDict.end(), now_s) != 0) {
                        dp[j] = 1;
                    }
                }
            }
        }
        return dp[n];
    }
```

### 4、乘积最大子数组

[152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)

这个题目的前置是：

[53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

但是这个题目更加复杂，因为最佳状态并不简单的来自于上一个状态的累乘。如果数字全是正数，那么做法和53一样，记录从上个位置累乘过来的最大数值即可。

但是由于有负数，两个负数相乘就会是正数，因此还需要记录以当前位置为结尾的最小子数组，因为后面可能碰到负数，变成更大的结果。

```cpp
    int maxProduct(vector<int>& nums) {
        int n = nums.size(), result = nums[0];
        vector<int> max_nums(n, INT_MIN), min_nums(n, INT_MAX);
        max_nums[0] = min_nums[0] =  nums[0];
        for(int i = 1; i < n; i++) {
            max_nums[i] = max(max_nums[i - 1] * nums[i], max(min_nums[i - 1] * nums[i], nums[i]));
            min_nums[i] = min(max_nums[i - 1] * nums[i], min(min_nums[i - 1] * nums[i], nums[i]));
            result = max(result, max_nums[i]);
        }
        return result;
    }
```

### 5、0-1背包问题

[416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

是一个0-1背包问题，用上面完全背包的分析去做，重要的是将分割子集的问题转换成，把一个恰好大小的背包塞满的问题。

```cpp
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        int all_sum = accumulate(nums.begin(), nums.end(), 0);
        if(all_sum % 2 == 1) {return false;}
        int target = all_sum / 2;
        vector<vector<int>> dp(n + 1, vector<int>(target + 1, 0));
        dp[0][0] = 1;
        for(int i = 1; i <= n; i++) {
            int now_weight = nums[i - 1];
            dp[i][0] = 1;
            for(int j = 1; j <= target; j++) {
                if(j - now_weight >= 0) {
                    dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - now_weight]);
                }else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[n][target];
    }
```

### 6、最长公共子序列问题

[1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

使用一个m*n大小的dp数组。dp\[i][j]表示a字符串前i个字符和b字符串前j个字符的最长公共子序列。可以让数组都拓展一位，避免-1导致的溢出问题。

```cpp
int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size(), n = text2.size();
        vector<vector<int>> result(m + 1, vector<int>(n + 1, 0));
        for(int i = 1; i <= m; i++) {
            for(int j = 1; j <= n; j++) {
                if(text2[j - 1] == text1[i - 1]) {
                    result[i][j] = result[i - 1][j - 1] + 1;
                } else {
                    result[i][j] = max(result[i - 1][j], result[i][j - 1]);
                }
            }
        }
        return result[m][n];
    }
```

### 7、最长回文子串问题

[5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

一种最朴实的想法是中心拓展法，从每个位置开始向两边延申，直到不是回文串为止。

但是判断过程需要正确处理开始条件：不能仅仅是某个位置i开始往左往右延申，因为这样就不能处理“abbc”这样的情况。因为对称轴可能在某个字母上，也可能在两个字母中间，因此开始搜索的左右边界，应该把与位置i相同的字母全部跳过，因为位置i中间这一段无论是几个字母，都是轴对称的。

```cpp
    string longestPalindrome(string s) {
        int max_len = 0, n = s.size();
        string result;
        for(int i = 0; i < n; i++) {
            int l = i - 1, r = i + 1;
            while(l >= 0 && s[l] == s[i]) {l--;}
            while(r < n && s[r] == s[i]) {r++;}
            while(l >= 0 && r < n && s[l] == s[r]) {l--; r++;}
            int now_len = r - l - 1;
            if(now_len > max_len) {
                max_len = now_len;
                result = s.substr(l + 1, r - l - 1);
            }
        }
        return result;
    }
```

动态规划法：

动态规划是同样的思想，只是将中心扩散转换成了转移条件：

dp\[i][j] = 1的条件：dp\[i + 1][j - 1] = 1 && s\[i + 1] == s[j - 1]；

对于j - i <= 1的情况：例如子字符串是“a”、“aa“的时候，上面公式不能正确判断，但是我们可以特判，子字符串长度<=2时，只需要s\[i + 1] == s[j - 1]一个条件，子字符串就是回文串。

```cpp
    string longestPalindrome(string s) {
        int max_len = 0, n = s.size();
        string result;
        vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));
        for(int i = 0; i <= n; i++) {
            dp[i][i] = 1;
        }
        for(int j = 0; j < n; j++) {
            for(int i = 0; i <= j; i++) {
                if(s[i] == s[j]) {
                    if(j - i <= 1 || dp[i + 1][j - 1]) {
                        dp[i][j] = 1;
                    }
                } 
                if(dp[i][j]) {
                    if(j - i + 1 > max_len) {
                        max_len = j - i + 1;
                        result = s.substr(i, j - i + 1);
                    }
                }
            }
        }
        return result;
    }
```

[131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

```cpp
vector<vector<string>> result;
    void dfs(vector<string> &now_s, vector<vector<int>> &dp, string s, int start_ind) {
        if(start_ind >= s.size()) {
            result.push_back(now_s);
            return;
        }
        for(int i = start_ind; i < s.size(); i++) {
            if(dp[start_ind][i]) {
                string s1 = s.substr(start_ind, i - start_ind + 1);
                now_s.push_back(s1);
                dfs(now_s, dp, s, i + 1);
                now_s.pop_back();
            }
        }
    }
    vector<vector<string>> partition(string s) {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for(int i = 0; i < n; i++) {
            dp[i][i] = 1;
        }
        for(int j = 0; j < n; j++) {
            for(int i = 0; i < j; i++) {
                if(s[i] == s[j]) {
                    if(j - i <= 1 || dp[i + 1][j - 1]) {
                        dp[i][j] = 1;
                    }
                }
            }
        }
        vector<string> now_s;
        dfs(now_s, dp, s, 0);
        return result;
    }
```

### 8、编辑距离

[72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

```cpp
int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for(int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for(int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }
        for(int i = 1; i <= m; i++) {
            for(int j = 1; j <= n; j++) {
                if(word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = min(min(dp[i - 1][j], dp[i][j - 1]) + 1, dp[i - 1][j - 1]);
                } else {
                    dp[i][j] = min(min(dp[i - 1][j], dp[i][j - 1]) + 1, dp[i - 1][j - 1] + 1);
                }
            }
        }
        return dp[m][n];
    }
```

### 9、单词拆分

[139. 单词拆分](https://leetcode.cn/problems/word-break/)

dp[i]的意义是：第i位结尾的字符串，能否被词典表示，需要注意的是最好后移一位，这样dp[0]就表示空字符串可以被表示。

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        int n = s.size();
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for(int i = 0; i < n; i++) {
            string now_s;
            for(int j = i; j >= 0; j--) {
                now_s = s[j] + now_s;
                if(dp[j]) {
                    if(count(wordDict.begin(), wordDict.end(), now_s) != 0) {
                        dp[i + 1] = 1;
                        break;
                    }
                }
                
            }
        }
        return dp[n];
    }
};
```

