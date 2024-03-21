# BOI2009 Radio Transmission 无线传输
## 题目描述
给你一个字符串 $s_1$，它是由某个字符串 $s_2$ 不断自我连接形成的（保证至少重复 $2$ 次）。但是字符串 $s_2$ 是不确定的，现在只想知道它的最短长度是多少。

## 输入格式
第一行一个整数 $L$，表示给出字符串的长度。 
第二行给出字符串 $s_1$ 的一个子串，全由小写字母组成。

## 输出格式
仅一行，表示 $s_2$ 的最短长度。
## 样例 #1
### 样例输入 #1
```
8
cabcabca
```
### 样例输出 #1
```
3
```

## 提示
#### 样例输入输出 1 解释
对于样例，我们可以利用 $\texttt{abc}$ 不断自我连接得到 $\texttt{abcabcabcabc}$，读入的 $\texttt{cabcabca}$，是它的子串。
#### 规模与约定
对于全部的测试点，保证 $1 < L \le 10^6$。

### 思路
对于给定的字符串 运用KMP思想：
设p\[x]为前x个字符前缀和后缀相同的最长长度，则对于题目中的长度len有：**len-p\[len]为第一个重复子串的最后一个字符位置**
因此**len-p\[len]即重复子串长度**

### 证明：
(1) $P$由*完整*的$k$个$S_2$连接而成，则$Next[n]$等于$k-1$个$S_2$的长度，那么剩下的$n-Next[n]$等于$S_2$的长度
(2) $P$由$k$个完整的$S_2$和 $1$ 个 *不完整* 的 $S_2$ 连接而成。设 $S_2$ 长度为$L$，不完整的部分长度为$Z$，则$Next[n]=(k-1)L+Z$，$n-Next[n]=kL+Z-(K-1)L-Z=L$，即为答案

这部分主要是运用到了KMP中求Next数组的部分，而且深入考察了最长公共前后缀这一个概念
```c++
#include <iostream>  
  
using namespace std;  
  
const int N = 1000010;  // 注意数据范围
int m;  
int ne[N];  
char s[N];  
  
int main(void) {  
	cin >> m >> s + 1;  
	ne[1] = 0;  
	for (int i  =  2, j = 0; i <= m; i++) {  // 注意这里是等于号！！  
		while (j && s[i] != s[j + 1]) j = ne[j];  
		if (s[j + 1] == s[i]) j++;  
		ne[i] = j;  
	}  
	printf("%d", m - ne[m]);  
}
```

# USACO15FEB Censoring S

## 题面翻译

Farmer John为他的奶牛们订阅了Good Hooveskeeping杂志，因此他们在谷仓等待挤奶期间，可以有足够的文章可供阅读。不幸的是，最新一期的文章包含一篇关于如何烹制完美牛排的不恰当的文章，FJ不愿让他的奶牛们看到这些内容。

FJ已经根据杂志的所有文字，创建了一个字符串  $S$  ( $S$ 的长度保证不超过 $10^6$ )，他想删除其中的子串 $T$ ,他将删去 $S$ 中第一次出现的子串 $T$ ，然后不断重复这一过程，直到 $S$  中不存在子串 $T$。

注意：每次删除一个子串后，可能会出现一个新的子串 $T$（说白了就是删除之后，两端的字符串有可能会拼接出来一个新的子串 $T$）。

**输入格式**：第一行是字符串  $S$  ，第二行输入字符串  $T$ ，保证  $S$  的长度大于等于  $T$  的长度， $S$  和  $T$  都只由小写字母组成。

**输出格式**：输出经过处理后的字符串，保证处理后的字符串不会为空串。

## 样例 #1

### 样例输入 #1
```
whatthemomooofun
moo
```

### 样例输出 #1
```
whatthefun
```

```c++
#include <iostream>
#include <cstring>
#include <stack>
using namespace std;

const int N = 1000010, M = 1000010;
int ne[N], location[N];
char s[N], p[M];
int m, n;

stack<int>stkch;

int main(void) {
	cin >> s + 1 >> p + 1;  // 注意一下这里的读入操作！
	m = strlen(s + 1);
	n = strlen(p + 1);

	// 构造ne数组
	for (int i = 2, j = 0; i <= m; i++ ) {
		while (j && p[i] != p[j + 1]) j = ne[j];
		if (p[i] == p[j + 1]) j++;
		ne[i] = j;
	}

	// 进行kmp匹配
	for (int i = 1, j = 0; i <= m; i++ ) {
		stkch.push(i);

		while (j && s[i] != p[j + 1]) j = ne[j];
		if (s[i] == p[j + 1]) j++;
		location[i] = j;

		if (j == n) {
			for (int k = 0;  k < n; k++ ) {
				if (!stkch.empty())
					stkch.pop();
			}
			if (!stkch.empty()) j = location[stkch.top()]; 
		// j = location[i - n]不可以，因为只适用于进行了一次删除，要是多次的话位置会卡住;
		}
	}

	int len = 0;
	char ans[1000010];
	while (stkch.empty() == false)
		ans[++len] = s[stkch.top()], stkch.pop();
	for (int i = len; i >= 1; i --) printf("%c", ans[i]);
}
```


