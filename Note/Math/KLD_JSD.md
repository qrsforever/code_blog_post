---

title: KL散度和JS散度

date: 2020-04-15 19:55
tags: [Math]
categories: [Note]

---

{% asset_jupyter python3 notebook/KLD_JSD.ipynb 300 %}


# KLD

When there are two probability distributions $P(x)$ and $Q(x)$ for the same random variable x, the
distance of these probability distributions can be evaluated using Kullback-Leibler (KL) divergence.

$$ 
\begin{align}
D_{KL} (P || Q) &= \mathbb {E}_{x \sim P} [log \frac {P (x)} {Q (x)}] \\
& = \mathbb {E}_ {x \ sim P} [log P (x)-log Q (x)] \\ & = \ int_ {x} P (x) (log P (x)-log Q ( x)) \ end
{align} $$


# References

[浅谈KL散度](https://www.cnblogs.com/hxsyl/p/4910218.html)
[JSD可視化](http://yusuke-ujitoko.hatenablog.com/entry/2017/05/07/200022)
