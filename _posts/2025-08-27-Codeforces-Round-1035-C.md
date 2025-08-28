---
title: "Codeforces Round 1035 C: A Good Problem"
date: 2025-08-27
categories: [Algorithm, Codeforces]
tags: [Bitmasks, Constructive Algorithms]
mathjax: true
---

## Problem Description

You are given four integers: n, l, r, and k.
You need to find the k-th element of an lexicographically smallest array a of length n that satisfies the following condition:
$$
a_1\mathbin{\&}a_2\mathbin{\&}...\mathbin{\&}a_n=a_1 \oplus a_2 \oplus...\oplus a_n
$$

Where & = bitwise AND and ⊕ = bitwise XOR.
If there is no possible array, print -1.

### Constraints

- $$ 1 \leq l \leq r \leq 10^{18} $$
- $$ 1 \leq n \leq 10^{12} $$
- $$ 1 \leq k \leq n $$

## My Journey
1. Initial Observations
When n is odd, the array should always be {l...}.
- Applying the AND operation on the same number does not change the value.
  - ex) $$ 3\mathbin{\&}3\mathbin{\&}3\mathbin{\&}3=3 $$
- Applying the XOR operation on the same number an odd number of times also does not change the value.
  - ex) $$ 3 \oplus 3 \oplus 3 = 3 $$
- So, you should choose l, which is the smallest number, to get lexicographically smallest array.
   
2.  Hypothesis When n is Even
When n is even, it was not that simple. I initially approached with Most Significant Bit (MSB) of l and r.
Hypothesis:
- if MSB(l) != MSB(r), there exists a valid array.
- else if MSB(l) == MSB(r), the MSB of both l and r is 1, which means all the numbers $r_i$ between l and r have the same MSB. $r_i$ cannot satisfy $$r_i\mathbin{\&}r_{i+1} = r_i \oplus r_{i+1}$$ because $1 \mathbin{\&} 1 = 1$ and $1 \oplus 1 = 0$

3. Challenge
I could not figure out how to find the lexicographically smallest array when n is even and MSB(l) != MSB(r). I was only one step behind the answer. Let's continue.

4. Wrong Approach
My first idea for even n was to take
$$ [a,a,...,a,a-1,a-1,...,a-1] $$ with exactly $\frac{n}{2}$ copies of each.
I thought this would always make XOR = 0 and also make AND = 0.

Problem: it only works when $n \equiv 0 (mod 4) $.
When $n \equiv 2 (mod 4) $, the number of 1-bits on the changed positions becomes odd, so XOR on those bits becomes 1 → the whole equality breaks.

ex) 
When $\frac{n}{2}=$odd, try $a = 8(1000_2)$ and $b = 7(0111_2)$ and array a = {8,8,8,7,7,7} does not satisfy the condition.
Bit counts on low bits are odd → XOR $\neq 0$.

5. Fixing the Idea
The important point from the previous approach was:
- Repeating the XOR operation on the same number an even number of times results in 0.
- The answer array only requires the two repeated numbers.
Therefore, I fixed my array to $(\frac{n}{2}+1,\frac{n}{2}-1)$ from $(\frac{n}{2},\frac{n}{2})$
Also pick two values whose 1-bits don’t overlap: $x\mathbin{\&}y=0$.

Example that works:
- $n=6$ and {6,6,6,8,8,8}.
  - $6(0110_2)$ and $b = 8(1000_2)$, so $6\mathbin{\&}8=0$ and XOR = 0.

This fixed point was also close to the editorial, but I could not reach that.

5. Important Observation: n=2 is impossible
For two numbers a,b, the equality $a\mathbin{\&}b=a\oplus b$ holds only if every bit is 0.
But here $l\geq1$.

1. The Correct Approach
I finally read the editorial and looked at top contestant's solutions after these long consideration.  
Here are the correct approach.
If n is even, the lexicographically smallest valid array is:
$$
[\,\underbrace{l,\dots,l}_{n-2\ \text{times}},\ z,\ z\,]
$$
where z is the smallest number in $[\ l, \ r]$ that satisfies ($z\mathbin{\&}l=0$)
- If such $\ z$ does not exist, answer = -1.
- If exists, then:
  - positions 1 ... n-2: value $\ l$
  - positions n-1,n: value $\ z$

## Solution Code

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <set>
#include <stack>
#include <cmath>
#include <map>
#include <queue>
#include <algorithm>
#include <functional>
#define rep(i, j, k) for (int i = j; i <= k; i++)
#define per(i, j, k) for (int i = j; i >= k; i--)
#define pb push_back

using namespace std;
using ll = long long;
using ull = unsigned long long;

ll n, l, r, k;

unsigned long long getMSBauto(unsigned long long x) {
    if (x <= 0xFFFFFFFFu) 
    {
        // 32bit
        unsigned int v = (unsigned int)x;
        v |= (v >> 1);
        v |= (v >> 2);
        v |= (v >> 4);
        v |= (v >> 8);
        v |= (v >> 16);
        return v - (v >> 1);
    } else 
    {
        // 64bit
        x |= (x >> 1);
        x |= (x >> 2);
        x |= (x >> 4);
        x |= (x >> 8);
        x |= (x >> 16);
        x |= (x >> 32);
        return x - (x >> 1);
    }
}
void solve()
{
    cin >> n >> l >> r >> k;

    if (n % 2) 
    {
        cout << l << '\n';
        return;
    }
    else if (n == 2)
    {
        cout << -1 << '\n';
    }
    else
    {
        ull a = getMSBauto(l);
        ull b = getMSBauto(r);
        if (a == b)
        {
            cout << -1 << '\n';
            return;
        }
        if (k >= n-1)
        {
            cout << (a << 1) << '\n';
            return;
        }
        cout << l << '\n';
    }
}

int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int t;
    cin >> t;
    for (int i = 0; i < t; i++)
    {
        solve();
    }
}
```

## Feelings
It has been a long time that I have not solved bitmask. It was literally a good problem that recall my memory and reactivate my brain. It was close that I could solve this problem by myself. I should practice regularly.
## Time Complexity
- getMSBauto(): O(1)
- solve(): O(1)
- **O(1)**

## References
- [Problem Link](https://codeforces.com/contest/2119/problem/C)