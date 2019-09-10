---

title: (原创)卷积三种模式使用numpy实现

date: 2019-07-23 13:52:26
tags: [Guide, Python]
categories: [ML]

---

<!-- vim-markdown-toc GFM -->

* [Mode](#mode)
    * [full](#full)
    * [same](#same)
    * [valid](#valid)
* [Codes](#codes)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Mode

Below the output of three modes:

Filter size must be odd number, eg: 3x3, 5x5. here we consider the situation which the size of image is
not less than the size of filter.

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/conv-mode.png){ width=70% }

let image: m x m and filter: n x n

mode | size of ouptut | extend matrix size
:---: | :---: | :---:
full | m + n - 1 | m + 2(n - 1)
same | m | m + n - 1
valid | m - n + 1 | m

if:
    A is 6x6, m = 6
    V is 3x3, n = 3

## full

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/conv-full.png "full mode"){ width=300px }


mode | size of ouptut | extend matrix size
:---: | :---: | :---:
full |  6+3-1 = 8 | 6+2*(3-1) = 10

## same

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/conv-same.png "same mode"){ width=300px }


mode | size of ouptut | extend matrix size
:---: | :---: | :---:
same |  6 | 6+3-1 = 8

## valid

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/conv-valid.png "valid mode"){ width=300px }


mode | size of ouptut | extend matrix size
:---: | :---: | :---:
valid |  6-3+1 = 4 | 6

# Codes

Only using numpy implement the conv2d, and compare the ouput with scipy.signal.convolve2d.

{% asset_jupyter python3 notebook/conv_mode.ipynb %}

# References

1. `http://www.songho.ca/dsp/convolution/convolution2d_example.html?source=post_page`

2. `https://blog.csdn.net/leviopku/article/details/80327478`

3. `https://docs.scipy.org/doc/numpy/reference/generated/numpy.convolve.html`

4. `https://blog.csdn.net/majinlei121/article/details/50256049`
