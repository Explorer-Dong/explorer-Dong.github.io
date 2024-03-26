---
title: z_logic
categories: Algorithm
category_bar: true
---

## 思维题

### 1. 最长严格递增子序列

https://www.acwing.com/problem/content/5273/

> 题意：给定长度为n的序列，问将这个序列拼接n次后，最长严格递增子序列的长度为多少？
>
> 思路：其实最终的思路很简单，讲题目转化为在每个序列中选一个数，一共可以选出多少个不同的数。但是在产生这样的想法之前，先讲一下我的思考过程。我将序列脑补出一幅散点折线图，然后将这些点投影到y轴上，最终投影点的个数就是答案的数量，但是投影会有重合，因此答案最多就是n个数，最少1个数，从而想到就是在n个序列中选数，选的数依次增大即可，相信的就是一个求序列不重复数的个数的过程。去重即可。
>
> 注意点：由于C++的STL的unique函数的前提是一个有序的序列，因此在unique之前需要将序列进行排序。
>
> 时间复杂度：$O(n)$

```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int T;
    cin >> T;

    while (T--)
    {
        int n;
        cin >> n;

        vector<int> a;
        for (int i = 0; i < n; i++)
        {
            int x;
            cin >> x;
            a.emplace_back(x);
        }

        // 去重老套路，string也可以用
        sort(a.begin(), a.end());
        a.erase(unique(a.begin(), a.end()), a.end());

        cout << a.size() << endl;
    }

    return 0;
}
```

### 2. 三元组

https://www.acwing.com/problem/content/5280/

> 题意：在一个序列中如何选择一个三元组，使得三个数之积最小，给出情况数
>
> 思路：首先很容易得知，这三个数一定是序列排序后的前三个数，那么就是对这三个数可能的情况进行讨论
>
> - 情况1：**前三个数都相等 **`（2 2 2 2 2 4 5）`，则就是在所有的 `a[0]` 中选3个，情况数就是 $C_{cnt\_a[0]}^{3}$
> - 情况2：**前三个数中有两个数相等**，由于数组经过了排序，因此情况2分为两种情况
>     - 前两个数相等 `（1 1 3 3 3 5）`，则就是在所有的 `a[2]` 中选1个，情况数就是 $C_{cnt\_a[2]}^{1}$
>     - 后两个数相等 `（1 2 2 2 4 6）`，则就是在所有的 `a[1]` 中选2个，情况数就是 $C_{cnt\_a[1]}^{2}$
> - 情况3：**前三个数中全都不相等** `（1 2 4 4 4 5 7）`，这就是在所有的 `a[2]` 中选1个，情况数就是 $C_{cnt\_a[2]}^{1}$
>
> 可以发现上述情况2的第一种和情况3的答案相等，故分为三种情况即可。

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

// 计算组合数 C_{a}^{b}
ll calc(ll a, ll b) {
    ll fz = 1, fm = 1;
    for (int i = 0, num = a; i < b; i++, num--) {
        fz *= num;
    }
    for (int i = 1; i <= b; i++) {
        fm *= i;
    }
    return fz / fm;
}

int main() {
    int n; cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    
    sort(a.begin(), a.begin() + n);
    
    ll res = 0;
    
    if (a[0] == a[1] && a[1] == a[2]) {
        // 情况1：前三个数都相等（2 2 2 2 2 4 5），则就是在所有的a[0]中选3个
        ll cnt = count(a.begin(), a.begin() + n, a[0]);
        res = calc(cnt, 3);
    } else if (a[0] == a[1] || (a[0] != a[1] && a[1] != a[2])) {
        // 情况2：前两个数相等（1 1 3 3 3 4 5） or 三个数都不相等（1 2 4 4 4 5 7），则就是在所有的a[2]中选2个
        res = count(a.begin(), a.begin() + n, a[2]);
    } else {
        // 情况3：后两个数相等（1 2 2 2 4 6），则就是在所有的a[1]中选2个
        ll cnt = count(a.begin(), a.begin() + n, a[1]);
        res = calc(cnt, 2);
    }
    
    cout << res << "\n";
    
    return 0;
}
```

### 3. 删除元素

https://www.acwing.com/problem/content/5281/

> 题意：给定一个排列为1~n，定义一个数“有价值”为当前数的前面没有数比当前的数大。现在需要删除一个数，使得序列中增加尽可能多的“有价值”的数，如果这个数有多个，则删除最小的那个数
>
> ==最开始想到的思路==：枚举每一个数，如果删除，则序列中会增加多少个“有价值”的数，算法设计如下：
>
> 1. 首先判断每一个数是否是有价值的数
>     - 创建一个变量来记录当前数的前面序列的最大值
>     - 比较判断当前数和前方最大值的关系，如果小于，则无价值，反之有价值
> 2. 接着枚举每一个数，如果删除该数，则序列会损失多少个有价值的数
>     - 首先判断自己是不是有价值的数，如果是，则当前损失值 -1
>     - 接着判断删除当前数对后续的影响 :star: ：我们在枚举后续数的时候，起始的newd（前驱除掉a[i]的最大值）应该就是当前的d，只不过需要使用新变量newd而非d是因为这一步与整体无关，不可以改变整体的d。否则会出现错误，比如对于`4 3 5 1 2`，如果我们在枚举后续数的时候直接对d进行迭代，那么在第一轮d就会被更新为5，就再也无法更新了
> 3. 最终根据维护的损失值数组cnt即可求解
> 4. 时间复杂度 $O(n^2)$
>
> ---
>
> ==优化的思路==：
>
> 1. 同样是枚举每一个数，如果当前数字是有价值的数，那么cnt[a[i]]就 -1
>
>     - 取决于当前数字前面是否有比它大的数，如果没有，那么就是有价值的
>
> 2. 接下来我们不需要遍历j=i+1到j=n-1，而是讨论当前数a[i]不是有价值数时的所有情况。现在我们想要维护cnt数组。不难发现，对于某个位置上的数a[k]，能否成为有价值的数只取决于前排序列是否有比他大的数以及大的数的个数。那么理论上我们在枚举a[i]的时候维护cnt[1~i-1]就可以确保cnt数组的正确性了。
>
>     - 假设a[k]的前排有一个比它大的数d：那么把d去掉之后，a[k]就会从无价值变为有价值
>     - 假设a[k]的前排有至少两个比它大的数d1,d2,...dj...：那么不管去掉哪一个 $d_j$，我们都没法让a[k]变得有价值
>
>     如此一来，cnt数组就可以正确维护了
>
> 3. 最终根据维护的损失值数组cnt即可求解
>
> 4. 时间复杂度 $O(n)$

暴力：

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int n;
	cin >> n;
	
	vector<int> a(n);
	for (int i = 0; i < n; i++) {
		cin >> a[i];
	}
	
	// 判断每个数是否是有价值的数
	int d = 0; // 前方序列中的最大值
	vector<bool> is_val(n + 1);
	for (int i = 0; i < n; i++) {
		if (a[i] > d) {
			is_val[a[i]] = true;
			d = a[i];
		}
	}
	
	// 枚举每一个数，计算删除该数之后会增加的有价值的数的个数 
	d = 0; // 同上
	vector<int> cnt(n + 1); // cnt[x]表示删除数x之后可以增加的有价值的数的个数
	for (int i = 0; i < n; i++) {
		if (is_val[a[i]]) {
			cnt[a[i]]--;
		}
		
		int newd = d; // 去掉a[i]后的前方序列的最大值 newd
		for (int j = i + 1; j < n; j++) {
			if (is_val[a[j]]) {
				if (a[j] < newd) {
					cnt[a[i]]--;
				}
			} else {
				if (a[j] > newd) {
					cnt[a[i]]++;
				}
			}
			if (a[j] > newd) {
			    newd = a[j];
			}
		}
		
		// 更新前方序列的最大值 d
		if (a[i] > d) {
			d = a[i];
		}
	}

	// 找出可以增加的有价值的数的最大个数 ma
	int ma = -n - 1;
	for (int i = 0; i < n; i++) {
		if (cnt[a[i]] > ma) {
			ma = max(ma, cnt[a[i]]);
		}
	}

	// 答案是满足个数的最小的数字
	int res = 0;
	for (int i = 1; i <= n; i++) {
		if (cnt[i] == ma) {
			res = i;
			break;
		}
	}
	
	cout << res << "\n";
	
	return 0;
}
```

优化：

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n; 
    cin >> n;
    
    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
     
    vector<int> cnt(n + 1);     // cnt[x]表示删除数x之后可以增加的有价值的数的个数
    int d1 = 0, d2 = 0;         // d1: 最大值, d2: 次大值
    for (int i = 0; i < n; i++) {
        if (a[i] > d1) {
            // 前方序列 没有数比当前的数大
            cnt[a[i]]--;
        } else if (a[i] > d2) {
            // 前方序列 只有一个数比当前大
            cnt[d1]++;
        }
        
        // 更新最大值d1和次大值d2
        if (a[i] > d1) d2 = d1, d1 = a[i];
        else if (a[i] > d2) d2 = a[i];
    }
    
    // 找到可以增加的最大值
    int ma = -1;
    for (int i = 1; i <= n; i++) {
        if (cnt[i] > ma) {
            ma = cnt[i];
        }
    }
    
    // 取元素的最小值
    int res = n + 1;
    for (int i = 1; i <= n; i++) {
        if (cnt[i] == ma) {
            res = i;
            break;
        }
    }
    
    cout << res << "\n";
    
    return 0;
}
```

### 4. Sorting with Twos

https://codeforces.com/contest/1891/problem/A

> 题意：给定一个序列，现在需要通过以下方法对序列进行升序排序
>
> - 可以选择前 $2^m$ 个数执行 $-1$ 的操作（保证不会越界的情况下）
>
> 现在需要确定给定的序列经过k次上述后能否变为升序序列
>
> 思路：我们可以将每 $2^k$ 个数看成一个数，可以不断的-1。那么可以发现执行一定的次数后，一定可以实现升序序列。但是现在是 $2^k$ 个数，那么只需要这相邻区段的数是升序即可，即 $2^{k-1} \to 2^k$ 之间的数是升序即可。按区间进行判断即可
>
> 时间复杂度：$O(n)$

```cpp
void judge(vector<int>& a, int l, int r, bool& ok, int n) {
	for (int i = l + 1; i <= r && i <= n; i++) {
		if (a[i] < a[i - 1]) {
			ok = false;
			return;
		}
	}
}
 
void solve() {
	int n;
	cin >> n;
 
	vector<int> a(n + 1);
	for (int i = 1; i <= n; i++) {
		cin >> a[i];
	}
 
	bool ok = true;
	judge(a, 3, 4, ok, n);
	judge(a, 5, 8, ok, n);
	judge(a, 9, 16, ok, n);
	judge(a, 17, 20, ok, n);
 
	if (ok) {
		cout << "YES\n";
	} else {
		cout << "NO\n";
	}
}
```

### 5. 指针运动​ :fire:

https://www.acwing.com/problem/content/description/5468/

> - 题意：给定一个序列，和一个从第一个数开始的指针，若当前指的数为 0 则输出当前数的下标结束程序，若不为 0 则全体正数减一并将指针后移，若指针越界则重新指向第一个数。问最终指向第几个数
> - 思路：
>     - 第一层思路：纯暴力。即按照题意进行模拟，时间复杂度 $O(n \times \max (a_i))$
>     - 第二层思路：枚举优化。假设当前枚举到了第 k 次，若第一次遇到当前的数小于 k，则该数就是终止指向的数，时间复杂度 $O(n \times \max(a_i))$
>     - 第三层思路：数列优化。
> - 时间复杂度：

```cpp

```

### 6. 套餐设计

https://www.acwing.com/problem/content/5480/

> 题意：给定 n 个食物，其中可能有相同种类的，现在需要设计一个食物套餐包含 k 个食物，问如何设计套餐可以使得产生的套餐数最多，给出最多数量的套餐数
>
> 思路：如果不限定套餐中食物的种类数，那么我们可以从最多可生产的套餐数 $\left \lfloor \frac{n}{k} \right \rfloor$ 开始降序检查，直到一套都无法生产为止（即一套需要的食物数超过了总的食物数）。检查的逻辑很简单，我们知道对于当前的套餐数 $i$，每一种食物的数量如果超过了 $i$，就至少可以对套餐贡献 $1$ 个食物的数量。我们统计当前需要套餐数的局面下，所有食物可以贡献的食物数量 $cnt$，如果超过了需要的套餐数 $i$，显然是可以满足需求的套餐数的。为了求得最大的套餐数，从降序开始检查直到可以满足即可。
>
> 时间复杂度：$O(n^2)$

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;
typedef long long ll;

const int N = 110;

int n, k, a[N];
unordered_map<int, int> v;

void solve() {
	cin >> k >> n;
	for (int i = 1; i <= n; i++) {
		cin >> a[i];
		v[a[i]]++;
	}
	
	int res = 0;
	
	int i; // 当前需要生产的套餐数
	for (i = n / k; i >= 1; i--) {
        // 所有食物对于当前套餐需要的食物数量可以贡献的最大食物数量
		int cnt = 0; 
		for (auto& x: v) {
            // 每种食物对于当前套餐需要的食物数量可以贡献的最大食物数量
			cnt += x.second / i;
		}
		if (cnt >= k) {
			res = i;
			break;
		}
	}
	
	cout << res << "\n";
}

int main() {
	ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
	int T = 1;
//	cin >> T;
	while(T--) solve();
	return 0;
}
```

### 7. 分班

https://www.acwing.com/problem/content/5481/

> 题意：给定 $n \times k$ 个学生，需要将这些学生分成 $n$ 组，每组 $k$ 个人，第 $i$ 个学生有一个属性值 $a_i$ 。问如何分组可以使得在满足任意两组学生中最小值相差不超过 $lim$ 的情况下，所有组最小值之和最大，给出这个最大值
>
> 思路：一开始觉得是二分答案，但是不知道在已知答案的情况下如何检查，因为我不知道应该如何分组最优，未遂。重读题目发现题中的 lim 约束不是相邻两组，而是任意两组，思路打开，那我直接根据这个条件将满足约束的所有学生枚举出来（很显然是相对于数值最小的学生而言），然后设计如何将这些学生安排到 n 组且保证他们是组里最小的即可。先看下面的学生编号 - 学生属性示意图：
>
> ![学生编号 - 学生属性示意图](https://dwj-oss.oss-cn-nanjing.aliyuncs.com/images/202403030040949.png)
>
> 很显然我们需要将这 $n \times k$ 个学生按数值升序排序，为了满足任意两组的最小值不超过 $lim$，我们找到前 $r$ 个学生满足比最小的学生超过的数不超过 $lim$ 即可。接下来就是从这 $r$ 个学生中选择出 $n$ 个安排到 $n$ 个组中，即可满足题中 $lim$ 的限制，现在就是考虑如何分组可以使得答案最大了。不难发现一共有三种情况
>
> - $r<n$ ：即可选择人数都没有组数多。那么分完组以后肯定无法满足任意两组的最小值不超过 $lim$，直接输出 $0$ 即可
> - $r=n$ ：即可选择人数和组数相等。这种情况最好理解，就是将这 $r(n)$ 个人安排到 $n$ 组，答案就是这 $r(n)$ 个人数值之和
> - $r>n$ ：即可选择人数多于组数。如果纯粹的贪心，将第一个人放在第一组，然后从第 $r$ 个人开始将 $n-1$ 个人安排在剩余的 $n-1$ 组。这样的确可以满足答案最大，但是忽略了一个点就是从第 $2$ 个人开始的 $r-n$ 个人何去何从？如果全部安排到了第一组那万事大吉，如果第一组塞不进去就不得不安排到别的组，这样答案就不是上面计算的结果了，应该更小才对！那么应该如何解决这个问题呢？我们可以从往第一组塞剩余的人得到启发：如果第一组可以容纳剩余的人活着还有空位就已经满足了第二种情况，就可以直接计算结果，如果第一组无法容纳从第 $2$ 个人开始的 $r-n$ 个人，就将第 $1+k$ 个人作为一组的最小值，继续贪心的容纳紧跟着的人，从而确保剩下的人数值尽可能的大。如此循环知道出现 **剩余的人的数量=还未安排最小值的组的数量** 为止，计算答案总和即可！
>
> 时间复杂度：$O(n \log n)$

```cpp
#include <iostream>
using namespace std;
typedef long long ll;

const int N = 1e5 + 10;

ll n, k, lim, a[N];

void solve() {
	cin >> n >> k >> lim;

	int num = n * k;

	for (int i = 1; i <= num; i++) cin >> a[i];

	sort(a + 1, a + num + 1);

    // 找到最大的满足 lim 限制的人的下标 r - 线性或二分均可
	int l = 1, r = num;
	while (l < r) {
		int mid = (l + r + 1) >> 1;
  		if (a[mid] <= a[1] + lim) l = mid;
  		else r = mid - 1;
	}

	ll res = 0;

	if (r < n) res = 0;
	else {
		res += a[1];
		int idx = 2;   // 指针
		int in = 1;    // 已安排作为组内最小值的人员数
		int team = 1;  // 当前组内的人员数
		while (r - idx + 1 > n - in) { // 当可选择人数多于组数时
			if (team < k) team++, idx++; // 塞得下就继续塞
			else team = 1, res += a[idx], idx++, in++; // 塞不下就重开一组
		}
		for (int i = idx; i <= r; i++) {
		    res += a[i];
		}
	}

	cout << res << "\n";
}

int main() {
	ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
	int T = 1;
//	cin >> T;
	while(T--) solve();
	return 0;
}
```

### 8. 双端队列

https://www.acwing.com/problem/content/5484/

> 题意：给定一个序列，每次可以从序列头或尾取出一个数，在满足：按取出数的顺序是升序序列的条件下，给定的序列最多可以取出多少数？
>
> 思路：首先很容易想到的就是哪头小取哪头，因为取了较小者后较大者可以后续继续被选择，反之则不能。当然，还需要考虑的一个点就是如果较小者不符合题意，即较小者比上一次选出来的数更小从而无法组成严格升序序列时，还是得考虑较大值的那一头数。接下来考虑相等的情况，若两头数值相等，那一定只能选择一端且后续都只能从那端继续选择，我们直接两种方法都计算统计一遍可选择的数的数量，取较大者即可
>
> 时间复杂度：$O(n)$

```cpp
#include <iostream>
#include <queue>
#include <cstring>
#include <vector>
#include <unordered_map>
#include <algorithm>
#include <stack>
using namespace std;

const int N = 200010;

int n, a[N];

void solve() {
	cin >> n;
	for (int i = 1; i <= n; i++) cin >> a[i];
	
	int res = 0, top = -1, l = 1, r = n;
	while (l <= r) {
        // 左端是较小值
		if (a[l] < a[r]) {
			if (a[l] > top) {
				res++;
				top = a[l++];
			} else if (a[r] > top) {
				res++;
				top = a[r--];
			} else {
				break;
			}
        // 右端是较小值
		} else if (a[l] > a[r]) {
			if (a[r] > top) {
				res++;
				top = a[r--];
			} else if (a[l] > top) {
				res++;
				top = a[l++];
			} else {
				break;
			}
        // 两端数值相等
		} else { // a[l] == a[r]
			if (a[l] > top) {
				int lcnt = 1, rcnt = 1;
				for (int i = l + 1; i <= r; i++) {
					if (a[i] > a[i - 1]) lcnt++;
					else break;
				}
				for (int i = r - 1; i >= l; i--) {
					if (a[i] > a[i + 1]) rcnt++;
					else break;
				}
				res += max(lcnt, rcnt);
				break;
			} else {
				break;
			}
		}
	}

	cout << res;
}

int main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

### 9. 你说的对，但是这是签到 I

https://hydro.ac/d/nnu_contest/p/P1201

> 题意：给定两个单调不减且只含有小写字母的字符串，求解其中最长公共子串长度
>
> 思路：
>
> - 一眼 dp？错误的，那时间复杂度就是 $O(n^2)$，显然不可能。可以发现与常规的最长公共子串的题目不同，本题中两个字符串是单调的，这个性质一定很有用。事实的确如此。
> - 对于单调的两个字符串而言，如果**连续的**字符数量之和相等，即对于 $c_i,c_j$ 之间的所有的字符对应的数量都相等 $a[c_k]=b[c_k],(k=i+1,i+1, \cdots,j-1)$，则该段字符串就一定是公共子串。答案就是 $\displaystyle  \sum_{k=i+1}^{j-1}a[c_k]$ 或 $\displaystyle  \sum_{k=i+1}^{j-1}b[c_k]$。在枚举字符时，需要特判一下只有一种字符的情况。
>
> 时间复杂度：$O(n + 26^3)$

```cpp
#include <iostream>
#include <cstring>
#include <vector>
#include <queue>
#include <stack>
#include <algorithm>
#include <unordered_map>
#include <set>
using namespace std;

const int N = 26;

string s, t;
int a[N], b[N];

void solve() {
	cin >> s >> t;
	
	for (auto c: s) a[c - 'a']++;
	for (auto c: t) b[c - 'a']++;
	
	int res = -1;
	
	for (int i = 0; i < N; i++) {
		for (int j = i; j < N; j++) {
			if (j == i) res = max(res, min(a[i], b[j]));
			else {
				bool ok = true;
				int mid = 0;
				for (int k = i + 1; k <= j - 1; k++) {
					if (a[k] != b[k]) ok = false;
					mid += a[k];
				}
				
				if (ok) {
					res = max(res, min(a[i], b[i]) + mid + min(a[j], b[j]));
				}
			}
		}
	}
	
	cout << res << "\n";
}

int main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```