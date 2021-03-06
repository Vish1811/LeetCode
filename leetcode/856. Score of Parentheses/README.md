# [856. Score of Parentheses (Medium)](https://leetcode.com/problems/score-of-parentheses/)

<p>Given a balanced parentheses string <code>S</code>, compute the score of the string based on the following rule:</p>

<ul>
	<li><code>()</code> has score 1</li>
	<li><code>AB</code> has score <code>A + B</code>, where A and B are balanced parentheses strings.</li>
	<li><code>(A)</code> has score <code>2 * A</code>, where A is a balanced parentheses string.</li>
</ul>

<p>&nbsp;</p>

<div>
<p><strong>Example 1:</strong></p>

<pre><strong>Input: </strong><span id="example-input-1-1">"()"</span>
<strong>Output: </strong><span id="example-output-1">1</span>
</pre>

<div>
<p><strong>Example 2:</strong></p>

<pre><strong>Input: </strong><span id="example-input-2-1">"(())"</span>
<strong>Output: </strong><span id="example-output-2">2</span>
</pre>

<div>
<p><strong>Example 3:</strong></p>

<pre><strong>Input: </strong><span id="example-input-3-1">"()()"</span>
<strong>Output: </strong><span id="example-output-3">2</span>
</pre>

<div>
<p><strong>Example 4:</strong></p>

<pre><strong>Input: </strong><span id="example-input-4-1">"(()(()))"</span>
<strong>Output: </strong><span id="example-output-4">6</span>
</pre>

<p>&nbsp;</p>

<p><strong>Note:</strong></p>

<ol>
	<li><code>S</code> is a balanced parentheses string, containing only <code>(</code> and <code>)</code>.</li>
	<li><code>2 &lt;= S.length &lt;= 50</code></li>
</ol>
</div>
</div>
</div>
</div>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Stack](https://leetcode.com/tag/stack/)

## Solution 1. Divide and Conquer

```cpp
// OJ: https://leetcode.com/problems/score-of-parentheses/
// Author: github.com/lzl124631x
// Time: O(N^2)
// Space: O(N^2)
class Solution {
public:
    int scoreOfParentheses(string S) {
        if (S == "()") return 1;
        int i = 0, left = 0;
        do {
            left += S[i++] == '(' ? 1 : -1;
        } while (left);
        if (i == S.size()) return 2 * scoreOfParentheses(S.substr(1, S.size() - 2));
        return scoreOfParentheses(S.substr(0, i)) + scoreOfParentheses(S.substr(i));
    }
};
```

## Solution 2. Divide and Conquer

Same idea as Solution 1, but more space efficient.

```cpp
// OJ: https://leetcode.com/problems/score-of-parentheses/
// Author: github.com/lzl124631x
// Time: O(N^2)
// Space: O(N)
class Solution {
private:
    int score(string &S, int begin, int end) {
        if (begin + 2 == end) return 1;
        int i = begin, left = 0;
        do {
            left += S[i++] == '(' ? 1 : -1;
        } while (left);
        if (i == end) return 2 * score(S, begin + 1, end - 1);
        return score(S, begin, i) + score(S, i, end);
    }
public:
    int scoreOfParentheses(string S) {
        return score(S, 0, S.size());
    }
};
```

## Solution 3. DFS

```cpp
// OJ: https://leetcode.com/problems/score-of-parentheses/solution/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(N)
class Solution {
    int dfs(string &s, int &i) {
        int ans = 0;
        while (i < s.size() && s[i] == '(') { 
            if (i + 1 < s.size() && s[i + 1] == ')') {
                i += 2;
                ++ans;
            } else {
                ++i;
                ans += 2 * dfs(s, i);
                ++i;
            }
        }
        return ans;
    }
public:
    int scoreOfParentheses(string s) {
        int i = 0;
        return dfs(s, i);
    }
};
```

## Soltuion 4. Stack

```cpp
// OJ: https://leetcode.com/problems/score-of-parentheses/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(N)
class Solution {
public:
    int scoreOfParentheses(string s) {
        stack<int> st;
        st.push(0);
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '(') {
                if (i + 1 < s.size() && s[i + 1] == ')') {
                    ++i;
                    st.top()++;
                } else st.push(0);
            } else {
                int val = 2 * st.top();
                st.pop();
                st.top() += val;
            }
        }
        return st.top();
    }
};
```

Or

```cpp
// OJ: https://leetcode.com/problems/score-of-parentheses/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(N)
class Solution {
public:
    int scoreOfParentheses(string s) {
        stack<int> st;
        st.push(0);
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '(') st.push(0);
            else {
                int val = max(2 * st.top(), 1);
                st.pop();
                st.top() += val;
            }
        }
        return st.top();
    }
};
```

## Solution 5.

```cpp
// OJ: https://leetcode.com/problems/score-of-parentheses/solution/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(1)
// Ref: https://leetcode.com/problems/score-of-parentheses/solution/
class Solution {
public:
    int scoreOfParentheses(string S) {
        int ans = 0, depth = 0;
        for (int i = 0; i < S.size(); ++i) {
            if (S[i] == '(') ++depth;
            else {
                --depth;
                if (i - 1 >= 0 && S[i - 1] == '(') ans += 1 << depth;
            }
        }
        return ans;
    }
};
```