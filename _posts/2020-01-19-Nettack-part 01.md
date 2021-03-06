# Nettack part 01

Nettack [^1] is the attack algorithm proposed in the work from [TUM](https://www.in.tum.de/daml/nettack/) .
Since nettack is targeted attacker for [GCN](https://arxiv.org/abs/1609.02907), 
so we can pick the node either from training set (poison attack) or test set (evasion attack).

Input: **A, X, W1, W2, u**

W1, W2 are used to find the best edges to be perturbed,
while the u is the target node

In the demo, the target is coming from test set

```python
assert u in split_unlabeled
```

Settings: **perturb_A, perturb_X, n_perturbation, n_influence, direct**

The empirical results show that perturbing structure is more effective than
perturbing the features.
So we ignoring the perturbation of the features here.
And also, a direct attack is more effective than the indirect attack,
so we use the direct attack.

In default,

* we assume 10% of the nodes are used in training, 10% for validation  
* we use the implementation of GCN in Nettack
* we replace the measure of performance from `(f1_micro + f1_macro)` with `accuracy_score`

Here, I want to address the question:
> How is the test accuracy performance if we do the combination in two models with two adjacency matrices, 
  one from original graph, one from the perturb graph after nettack.


## Procedure

  ```text
  for i in (# of evaluation):
    for u in test:
      A' : surrogate(A, X, u)
      s_acc : surrogate(A, X)
      s'_acc : surrogate(A', X)
      g_acc : GCN(A, X)
      g'_acc : GCN(A', X)
  ```
  $A$ is the original graph, and $A'$ is the perturbed graph with target $u$.

## Results

| dataset        | citeseer        | cora            | polblogs        | cora_ml         | dblp            | pubmed          |
| -------------- | --------------- | --------------- | --------------- | --------------- | --------------- | --------------- |
| surrogate (A)  | 71.77 $\pm$ .62 | 83.08 $\pm$ .67 | 95.08 $\pm$ .38 | 84.49 $\pm$ .34 | 84.63 $\pm$ .10 | 82.65 $\pm$ .41 |
| surrogate (A') | 71.68 $\pm$ .77 | 83.10 $\pm$ .71 | 94.95 $\pm$ .33 | 84.38 $\pm$ .40 | 84.64 $\pm$ .09 | 82.70 $\pm$ .39 |
| gcn (A)        | 72.47 $\pm$ .81 | 83.93 $\pm$ .62 | 95.26 $\pm$ .60 | 84.82 $\pm$ .36 | 85.80 $\pm$ .22 | 83.32 $\pm$ .21 |
| gcn (A')       | 72.44 $\pm$ .69 | 83.92 $\pm$ .51 | 95.26 $\pm$ .40 | 84.88 $\pm$ .39 | 85.79 $\pm$ .22 | 83.28 $\pm$ .25 |


* compare the surrogate model and gcn model with A as input, the linearized model won't hurt the performance. It is also one of the main results form SGCN [^2]
* compare the performance in the same model with different input A and A', we cannot see a big difference in accuracy. It is mainly because the Nettack itself is a targeted attacker.

## Attack the training set (poison attack)

Similar to the previous setting, and now we set the target to be in the training set.

| dataset        | citeseer        | cora            | polblogs        | cora_ml         | dblp            | pubmed          |
| -------------- | --------------- | --------------- | --------------- | --------------- | --------------- | --------------- |
| surrogate (A)  | 72.06 $\pm$ .62 | 83.08 $\pm$ .59 | 95.12 $\pm$ .18 | 84.66 $\pm$ .42 | 84.68 $\pm$ .06 | 82.71 $\pm$ .43 |
| surrogate (A') | 71.85 $\pm$ .47 | 83.18 $\pm$ .60 | 95.08 $\pm$ .23 | 84.49 $\pm$ .46 | 84.62 $\pm$ .14 | 82.60 $\pm$ .55 |
| gcn (A)        | 72.96 $\pm$ .77 | 84.26 $\pm$ .59 | 95.33 $\pm$ .33 | 84.99 $\pm$ .28 | 85.75 $\pm$ .12 | 83.37 $\pm$ .11 |
| gcn (A')       | 72.63 $\pm$ .71 | 83.91 $\pm$ .59 | 95.21 $\pm$ .45 | 84.78 $\pm$ .27 | 85.87 $\pm$ .17 | 83.34 $\pm$ .17 |

* no big difference compared with attacking on the test set.

## References

[^1]: Nettack Github repo: [https://github.com/danielzuegner/nettack](https://github.com/danielzuegner/nettack)
[^2]: SGCN: [http://proceedings.mlr.press/v97/wu19e.html9](http://proceedings.mlr.press/v97/wu19e.html9)