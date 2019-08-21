---

title: (draft)机器学习术语

date: 2019-08-19 10:35:45
tags: [Guide, Terms]
categories: [ML]

---


<!-- vim-markdown-toc GFM -->

* [Gradient Descent](#gradient-descent)
    * [Epoch](#epoch)
    * [Batch Size](#batch-size)
    * [Iterations](#iterations)

<!-- vim-markdown-toc -->

<!-- more -->

# Gradient Descent

It is a iterative optimization algorithom.

Gradient means the rate of inclination or declination of a slope.

Descent means the instance of descenting.

we also need terminologies like learning rate, cost function, and below:

## Epoch

One Epoch is when an **ENTIRE** dataset is passed forward and backward through the neural network only
**ONCE**.

**So, what is the right numbers of epochs?**

> No right answer to this question. The answer is different for different dataset.
> the numbers of epochs is related to how diverse your data is.

## Batch Size

The total number of tranning examples present in a signal batch.

**But what is a Batch?**

> we can't pass the entire dataset into the nerual net at once, so we divide dataset tinto number of
> batches or sets or parts

Note: Batch size and number of batches are two different things.

## Iterations

It is the number of batches need to complete once epoch. **W** and **b** update for one iteration

Note: The number of batches is equal to number of iterations for one epoch.

Examples:

> We can divide one dataset of 2000 examples into batches of 500 then it will take 4 iterations to
> complete 1 epoch.

**Where Batch size is 500 and iterations is 4, for 1 complete epoch.**

#. https://towardsdatascience.com/epoch-vs-iterations-vs-batch-size-4dfb9c7ce9c9
#. https://www.cnblogs.com/mstk/p/8214499.html


# Optimization Algorithom

## Adam(Adaptive momentum algorithom)

pass
