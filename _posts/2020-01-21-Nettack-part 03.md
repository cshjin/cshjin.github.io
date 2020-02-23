# Nettack part 03

As mentioned in previous work [^1], the Nettack needs `W1, W2` from a learned surrogate model to find the perturbations.
However, if we replace the `W1, W2` with the parameter from the GCN model, this raise another question

> what if we use the vanilla GCN to find the edges to be perturbed, will that also maintain the effectiveness or not

## Procedure

  ```text
  W_s <= surrogate(A, X)
  W_g <= GCN(A, X)

  Given a target node $u$ in test set,

  A_s <= nettack(A, X, W_s, u)
  A_g <= nettack(A, X, W_g, u)


  Train a new model using the perturbed graph,
  and check whether the predicted label of the target node changes or not?
  c_u : true label
  c_u': label from A_s
  c_u'': label from A_g
  ```

## Results

* pick 100 nodes in test set which are classified correctly in vanilla GCN
* if the budget is set to the degree of the target:
  
  |          | surrogate | gcn  |
  | -------- | --------- | ---- |
  | citeseer | 0.91      | 0.91 |
  | cora     | 1.00      | 0.97 |
  | cora_ml  | 0.96      | 0.96 |
  | polblogs | 0.96      | 0.98 |
  | dblp     | 0.71      | 0.82 |
  | pubmed   | 0.95      | 0.92 |

* if the budget is set to a fixed number (5):

  |          | surrogate | gcn  |
  | -------- | --------- | ---- |
  | citeseer | 0.94      | 0.96 |
  | cora     | 0.94      | 0.93 |
  | cora_ml  | 0.93      | 0.93 |
  | polblogs | 0.30      | 0.32 |
  | dblp     | 0.76      | 0.78 |
  | pubmed   | 0.93      | 0.92 |

* we can use the weights from not only surrogate models, but also the vanilla gcn
* it is still unknown whether using the weights from similar models can be also applied to nettack

## Reference

[^1]: Nettack part 01: [https://cshjin.github.io/2020/01/19/Nettack-part-01.html](https://cshjin.github.io/2020/01/19/Nettack-part-01.html)
