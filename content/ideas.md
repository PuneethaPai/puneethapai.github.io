---
title: "Ideas"
date: 2020-08-12T10:58:35+05:30
draft: true
---

August 5 2020:

Early prediciton:
- while training a Classification NN, we will have learnt theta min for L(theta, f, D) is minimum
- we have final layer where P(y/x) is predicted
- If we compute P(a/y) at previous layers and reduce n/w computations
  - How much back we can come in layers?
  - Will it actually reduce computation and optimises state of the art NN?


August 12 2020:

Batch selection process:
- We tend to use same amount of resource even at the end of trainig where improvement in loss tend to decrease
- As humans we tend to skip already understood concepts and focus on difficult concepts during revision or when we are under time/resource contraints
- Why can't we do batch selection such that it selects and learn from difficult samples?
    - After each epoch compute loss for each sample (or aggregate over sample size)
    - Sort and choose data from samples with high loss
    - Thus we choose samples for which the loss is high to learn from.
    - We are also not ignoring already seen/learnt samples as at end of each epoch loss gets re-computed and samples gets re-ordered
    - Ideas is to see sample specific model loss and reducing sample size from N -> 1 as we advance in Number of epochs
    - Things to be afraid of:
        - If model has high variance it can easily overfit and loss graph will have noise
        - To smoothen loss graph we can tune learning rate, etc