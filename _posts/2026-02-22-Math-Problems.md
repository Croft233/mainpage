---
title: "几个颇有难度的算法"
description: >-
  Hard but fun.
  
# author: Retr0
date: 2026-02-22 21:20:00 +0900
categories: [Math, Algorithm]
tags: [Algorithm]
pin: true
math: true
render_with_liquid: false
media_subpath: '/posts/20240518'
---

**1. Toss a cubic dice 600 times. P denotes the probability of getting each face exactly 100 times. Find round 10^12P to the nearest integer.**

骰子每一面是公平的，那么在 600 次投掷中，每个面恰好出现 100 次的概率就是多项式概率。

$$ P = \frac{600!}{{100!}^6}(\frac{1}{6})^{600}$$

这个数大约是

$$ P≈2.4632858255234954×10^{−7}$$

所以最接近10^12P的整数是：

$$ 10^{12}P≈246328 $$

<br>

---

<br>

**2. When 4444^4444 is written in decimal notation, the sum of its digits is A. Let B be the sum of digits of A. Find the sum of the digits of B.**

- 关键
  
  如果不断累加各位数字直到只剩一位数字，那么最后一位数字等于：
  
  - 如果原数为 0，则等于 0；
  - 否则等于原数模 9 的结果，其中余数 1 到 8 对应自身，余数 0 对应 9。
  
  这是因为 10 ≡ 1 (mod 9) ≡ 101 (mod 9)，所以一个数与其各位数字之和模 9 同余，并且重复累加不会改变这个结果

所以我们只需计算 N(mod 9)

- **计算 $$ {4444}^{4444} (mod 9)$$**

$$ 4444≡7 (mod 9)$$

所以，$$ {4444}^{4444}≡7^{4444} (mod 9)$$
 
根据欧拉公式：$$ \varphi(9) = 6, gcd(7, 9)=1 $$

$$7^{4444}≡7^{4444 mod  6} (mod  9)$$

计算 4444 mod 6：
    $$ 4444 = 6*740 + 4$$，所以指数为4。

$$ 7^2 = 49 ≡ 4 ( mod 9), 7^4 ≡ 42 = 16 ≡ 7 ( mod 9) $$

<br>

---

<br>

**3. In how many ways can you evenly divide {1,2,3,...,24,25} into five groups, such that each group contains five numbers, and the sum of the numbers in each group is 65?**

- 首先列出集合 {1, …, 25} 中所有和为65的5个元素集合。这样的5个元素的集合共有 1394 个
- 为了避免因排列这5个元素的集合而导致重复计数，强制执行规范选择：始终选择包含剩余元素最小的子集。
- 递归地移除选定的5个元素集合，并继续处理剩余的元素。

```
from functools import lru_cache

combo_list = combos
contain = {i: [] for i in nums}
for idx,c in enumerate(combo_list):
    for x in c:
        contain[x].append(idx)

def mask_from_set(s):
    m=0
    for x in s:
        m |= 1<<(x-1)
    return m
combo_masks=[mask_from_set(c) for c in combo_list]

fullmask = (1<<25)-1

def count_partitions(rem_mask):
    if rem_mask==0:
        return 1
    # find smallest number in rem
    lsb = rem_mask & -rem_mask
    x = (lsb.bit_length())  # because bit_length gives index, since lsb is 1<<(x-1)
    # choose a combo containing x that is subset of rem
    total=0
    for cm in contain_masks[x]:
        if (cm & rem_mask)==cm:
            total += count_partitions(rem_mask ^ cm)
    return total

ans = count_partitions(fullmask)

```

```
3245664
```

<br>

---

<br>

**4. We call a positive integer n a blue number, if and only if $$ 4^n+2^n+1 $$ is divisible by $$ n^2 $$. The first three blue numbers are 1, 7, and 2359. The next one is**

$$ 4^n+2^n+1 $$ 可以写成 $$ (2^n)^2+2^n+1 $$

上面是个分圆多项式 $$ \Phi_n(x)$$ , 其中下标的$$ n = 3 $$, $$ x=2^n $$ 

每个质因数p的n必须满足相关的特定条件 $$2 ( mod p^2) $$

第一个blue number是 `1` 。其中，$$ 4^1 + 2^1 + 1= 7 $$ 可以被 $$ 1^2 $$因式分解 ($$ 7 = 1^2 * 7 $$)。

第二个blue number是 `7`。 其中，$$ 4^7 + 2^7 + 1= 16513 $$ 可以被 $$ 7^2 $$因式分解 ($$ 16513 = 7^2 * 337 $$)。

第三个blue number是 `2359`。 其中，$$ 4^{2359} + 2^{2359} + 1 $$ 可以被 $$ 2359^2 $$因式分解 ($$ 4^{2359} + 2^{2359} + 1 = 2359^2 * p = (7 * 337)^2 * p $$)。

那么第四个blue number，需要找到一个比例项p分解 $$ \frac{4^{2359}+2^{2359}+1}{2359^2}$$ 和 $${(2359p)}^2$$分解$$4^{2359}+2^{2359}+1$$。

满足必要条件 `p≡1 (mod 3)`, 可以找到下一个 `p=42463`是第四个blue number的分解因数。

$$ 2359 * 42463 = 100170217 $$
