---

title: MSE和CE不同

date: 2019-06-25 21:41:53
tags: [Passage]
categories: [Story]

---



> [What's the difference between cross-entropy and mean-squared-error loss functions?][1]
> 
> These loss functions have different derivatives and different purposes. From a probabilistic point of view, the cross-entropy arises as the natural cost function to use if you have a sigmoid or softmax nonlinearity in the output layer of your network, and you want to maximize the likelihood of classifying the input data correctly.
> 
> If instead you assume the target is continuous and normally distributed, and you maximize the likelihood of the output of the net under these assumptions, you get the MSE (combined with a linear output layer).
> 
> For classification, cross-entropy tends to be more suitable than MSE -- the underlying assumptions just make more sense for this setting. That said, you can train a classifier with the MSE loss and it will probably work fine (although it does not play very nicely with the sigmoid/softmax nonlinearities, a linear output layer would be a better choice in that case). For regression problems, you would almost always use the MSE.
> 
> Another alternative for classification is to use a margin loss, which basically amounts to putting a (linear) SVM on top of your network.


[1]: https://www.reddit.com/r/MachineLearning/comments/3klqdh/q_whats_the_difference_between_crossentropy_and/
