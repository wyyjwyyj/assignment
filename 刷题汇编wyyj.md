# 刷题汇编

## 动态规划难题

#### 核电站

```python
n, m = map(int, input().split())
a = [0] * (n + 1)
a[0] = 1
for i in range(1, n + 1):
    if i < m:
        a[i] = 2 * a[i - 1]
    elif i == m:
        a[i] = 2 * a[i - 1] - 1
    else:
        a[i] = 2 * a[i - 1] - a[i - 1 - m]
print(a[n])
```

#### 复杂的整数划分问题

```python
def divide_k(n, k):
    # dp[i][j]为将i划分为j个正整数的划分⽅法数ᰁ
    dp = [[0]*(k+1) for _ in range(n+1)]
    for i in range(n+1):
        dp[i][1] = 1
    for i in range(1, n+1):
        for j in range(1, k+1):
            if i >= j:
                # dp[i-1][j-1]为包含1的划分的数ᰁ
                # 若不包含1，我们对每个数-1仍为正整数，划分数ᰁ为dp[i-j][j]
                dp[i][j] = dp[i-j][j]+dp[i-1][j-1]
    return dp[n][k]


def divide_dif(n):
    # dp[i][j]表示将数字 i 划分，其中最⼤的数字不⼤于 j 的⽅法数ᰁ
    dp = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(1, n + 1):
            # ⽐i⼤的数没⽤
            if i < j:
                dp[i][j] = dp[i][i]
            # 多了⼀种：不划分
            elif i == j:
                dp[i][j] = dp[i][j - 1] + 1
            # ⽤/不⽤j
            else:
                dp[i][j] = dp[i][j - 1] + dp[i - j][j - 1]
    return dp[n][n]


def divide_odd(n):
    # dp[i][j]整数i的划分⾥最⼤的数是j
    dp = [[0] * (n + 1) for _ in range(n + 1)]
    dp[0][0] = 1
    for i in range(1, n + 1):
        for j in range(1, n + 1):
            if j % 2 == 0:
                dp[i][j] = dp[i][j-1]
            else:
                if i < j:
                    dp[i][j] = dp[i][i]
                elif i == j:
                    dp[i][j] = dp[i][j - 1] + 1
                # ⽤/不⽤j
                else:
                    dp[i][j] = dp[i][j - 1] + dp[i - j][j]
    return dp[n][n]


while True:
    try:
        n, k = map(int, input().split())
        print(divide_k(n, k))
        print(divide_dif(n))
        print(divide_odd(n))
    except EOFError:
        break
```

#### 宠物小精灵之收服

```python
N, M, K = map(int, input().split())
L = [[-1]*(M+1) for i in range(K+1)]
L[0][M] = N
for i in range(K):
    cost, dmg = map(int, input().split())
    for p in range(M):
        for q in range(i+1, 0, -1):
        	if p+dmg <= M and L[q-1][p+dmg] != -1:
        		L[q][p] = max(L[q][p], L[q-1][p+dmg]-cost)
                
                
def find():
	for i in range(K, -1, -1):
		for j in range(M, -1, -1):
			if L[i][j] != -1:
				return [str(i), str(j)]
            
            
print(' '.join(find()))
```

## 集合覆盖问题（贪心）

#### 世界杯只因

```python
n = int(input())
ls = list(map(int, input().split()))
ends = [0]*n		#通过区间左端点获取右端点
for i in range(n):
    if i - ls[i] > 0:
        ends[i - ls[i]] = i + ls[i]
    else:
        ends[0] = max(ends[0], i + ls[i])		#0处需要特殊判断
count = 1
l = r = ends[0]
for i in range(1, n):
    r = max(r, ends[i])
    if i >= l + 1:
        l = r
        count += 1
print(count)
```

#### Radar Installation

```python
from math import sqrt

num = 1
while True:
    n, d = map(int, input().split())
    if n + d == 0:
        break
    x_y = {}
    for _ in range(n):
        x, y = map(int, input().split())
        if x in x_y:
            x_y[x] = max(x_y[x], y)
        else:
            x_y[x] = y
    n = len(x_y)
    count = 1
    x = list(sorted(x_y.keys()))
    try:
        ls = [x[i] + sqrt(d**2 - x_y[x[i]]**2) for i in range(n)]
        for i in range(2, n + 1):
            ls[-i] = min(ls[-i], ls[-i + 1])
        l = ls[0]
        for i in range(1, n):
            if (x[i] - l)**2 + x_y[x[i]]**2 - d**2 > 0.001:
                l = ls[i]
                count += 1
    except ValueError:
        count = -1
    if d < 0:
        count = -1
    print(f'Case {num}: {count}')
    num += 1
    input()
```

## 田忌赛马（贪心）

```python
while True:
    n = int(input())
    if not n:
        break
    t = list(map(int, input().split()))
    k = list(map(int, input().split()))
    t.sort()
    k.sort()
    k_j = t_j = n - 1
    ans = k_k = t_k = 0
    for i in range(n):
        if k[k_j] < t[t_j]:
            k_j -= 1
            t_j -= 1
            ans += 1
        elif k[k_j] > t[t_j]:
            ans -= 1
            t_k += 1
            k_j -= 1
        else:
            if k[k_k] < t[t_k]:
                k_k += 1
                t_k += 1
                ans += 1
            else:
                ans -= (k[k_j] > t[t_k])
                k_j -= 1
                t_k += 1
    print(ans*200)
```

## Jumping Cows（贪心）

```python
n = int(input())
a = [int(input()) for _ in range(n)]
ans = 0
b = True
for i in range(n - 1):
    if b:
        if a[i] > a[i + 1]:
            ans += a[i]
            b = False
    else:
        if a[i] < a[i + 1]:
            ans -= a[i]
            b = True
if b:
    ans += a[-1]
print(ans)
```

## 最大连续子序列和（DP）

```python
dp = [0]*n
dp[0] = a[0]
for i in range(1, n):
	dp[i] = max(dp[i-1]+ls[i], ls[i])
print(max(dp))
```

## 最大上升子序列和（DP）

```python
n = int(input())
ls = [-1] + list(map(int, input().split()))		#已知输入数据非负，因此ls[0]可设为-1
dp = [0, ls[1]] + [0]*(n - 1)
for i in range(2, n + 1):
    for j in range(i):
        if ls[i] > ls[j]:
            dp[i] = max(dp[i], dp[j] + ls[i])
print(max(dp))
```

## 最长公共子序列

```python
while True:
    try:
        x, y = input().split()
    except EOFError:
        break
    lx, ly = len(x), len(y)
    dp = [[0]*(ly + 1) for _ in range(lx + 1)]
    for i in range(1, lx + 1):
        for j in range(1, ly + 1):
            if x[i - 1] == y[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            dp[i][j] = max([dp[i][j], dp[i - 1][j], dp[i][j - 1]])
    print(dp[-1][-1])
```

​	**最长回文子序列问题：**A的最长回文子序列长度等于A与reversed(A)的最长公共子序列长度

## KMP算法

```python
for _ in range(int(input())):
    txt, pat = map(str, input().split())
    n = len(txt)
    m = len(pat)
    ne = [0]*m
    for i in range(1, m):
        if pat[ne[i - 1] + 1] == pat[i] and ne[i - 1] + 1 < i:
            ne[i] = ne[i - 1] + 1
    ne = [0] + ne
    i = j = 0
    ans = []
    while i < n:
        if txt[i] == pat[j]:
            i += 1
            j += 1
            if j == m:
                ans.append(str(i - j))
                j = min(m - 1, ne[j] + 1)
        else:
            i += (not j)
            j = ne[j]
    if ans:
        print(' '.join(ans))		#输出结果为子串首部在txt中的索引
    else:
        print('no')
```

## 河中跳房子（二分查找）

```python
L,n,m = map(int,input().split())
rock = [0]
for i in range(n):
	rock.append(int(input()))
rock.append(L)


def check(x):
	num = 0
	now = 0
	for i in range(1, n+2):
		if rock[i] - now < x:
			num += 1
		else:
			now = rock[i]
	if num > m:
		return True
	else:
		return False

    
lo, hi = 0, L
while lo < hi:
	mid = (lo + hi) // 2
	if check(mid):
		hi = mid
	else:
		lo = mid + 1
print(lo-1)
```

## 