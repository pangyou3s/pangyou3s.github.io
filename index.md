## [题目链接：](https://atcoder.jp/contests/abc287/tasks/abc287_d)

第一次提交：依据题意直接模拟，喜提 $\sf TLE$。
![image](https://img2024.cnblogs.com/blog/3085107/202404/3085107-20240421193729191-1586612075.png)

```cpp
#include <bits/stdc++.h>

using namespace std;

bool check(string a, string b) {
	for (int i = 0; i < a.size(); i++) {
		if (a[i] == '?' && b[i] != '?') a[i] = b[i];
		else if (a[i] != '?' && b[i] == '?') b[i] = a[i];
	}
	return (a == b);
}

int main()
{
	string S, T;
	cin >> S >> T;
	int n = T.size();
	for (int i = 0; i <= n; i++) {
		string t;
		string t1 = S.substr(0, i);
		string t2 = S.substr(S.size() + i - n, n - i);
		t = t1 + t2;
		if (check(t, T)) puts("Yes");
		else puts("No");
	}
	return 0;
}
```
显然时间复杂度是 $O(n^2)$ 的，数据范围到了 $10^5$ 肯定会超时。



思考优化：暴力解法是把 $S'$ 计算出来，加了两个子串后再去遍历判断。<font color=red>**考虑单个字符的匹配</font>：** 如果两个字符 $a$ 和 $b$ 至少一个为 $?$ 或者 $a=b$，则这两个字符相匹配。

所以我们可以用 $O(n)$ 预处理出 $S$ 和 $T$ 前多少个字符相匹配/后多少个字符相匹配，然后对于每次查询 $x$，只需看前 $x$ 个字符和后 $m-x$ 个字符是否在之前预处理出的字符数范围内即可。$O(1) \times O(n) = O(n)$

总时间复杂度为 $O(n)$


```cpp
#include <bits/stdc++.h>

using namespace std;

bool match(char a, char b) {
	return a == '?' || b == '?' || a == b;
}

int main()
{
	string S, T;
	cin >> S >> T;
	int n = S.size(), m = T.size();
	
	int i = 0, j = 0;
	while (i < m && match(S[i], T[i])) i++;//表示S和T的前i个字符都匹配
	while (j < m && match(S[n - 1 - j], T[m - 1 - j])) j++;//表示S和T的后j个字符都匹配
	
	for (int x = 0; x <= m; x++) {
		cout << (x <= i && m - x <= j ? "Yes" : "No") << "\n";
	}
	return 0;
}
```
