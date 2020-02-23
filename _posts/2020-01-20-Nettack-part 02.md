# Nettack part 02

Continue with the previous work [^1], since the Nettack will generate a new graph for the targeted node.
A straight-forward question is

> What if we iteratively change the perturbed graph?

## Procedure

  ```text
  acc : GCN(A, X)
  accs : [acc]
  for iter in iterations:
    W : surrogate(A, X)
    A' : nettack(A, X, W, u) // u is a random target in test set
    acc : GCN(A', X)
    accs.append(acc)
    A : A'
  OUTPUT : accs
  ```

## Results

|                     citeseer                     |                     cora                     |
| :----------------------------------------------: | :------------------------------------------: |
| ![img](/images/iteratively_perturb_citeseer.png) | ![img](/images/iteratively_perturb_cora.png) |

* intuitively, yes, with more perturbations to the graph, the performance will decrease.

## Reference

[^1]: Nettack part 01: [https://cshjin.github.io/2020/01/19/Nettack-part-01.html](https://cshjin.github.io/2020/01/19/Nettack-part-01.html)
