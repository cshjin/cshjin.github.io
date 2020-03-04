# Revisiting Graph Auto-encoder (GAE)

Graph Auto-Encoder (GAE)[^1] is the work from T.N. Kipf and M. Welling,
and is published in NIPS 2016 workshop on Bayesian Deep Learning.
It is the first work apply the GCN layer, proposed by Kipf, node representation learning in th task of link prediction.

## Layer

GCN layer learns the node representation by aggregation neighborhoods' information. 
$$
\mathbf{H^{(l+1)}} = \sigma (\hat{\mathbf{A}} \mathbf{H^{(l)}} \mathbf{W^{(l)}})
$$
And the first layer hidden representation is $\mathbf{H^{(0)}} = \mathbf{X}$ is the input feature matrix.
In the paper, the $\hat{\mathbf{A}} := (\mathbf{D+I})^{-1/2} \mathbf{(A+I)} (\mathbf{D + I})^{-1/2}$.
Typically, a ReLU activation function is used in the GCN layer.

## Models

### GAE

Idea of Graph Auto-Encoder (GAE) comes from the auto-encoder in CNN, where the neural networks try to restore the image after training.

$$
\mathbf{P} = \sigma(\mathbf{Z} \mathbf{Z}^\top).
$$
$\mathbf{P}$ is the recovered matrix, and $\mathbf{Z}$ is the node representation after two GCN layers.
Here, $\sigma(\dot)$ is a sigmoid function, a smooth function ranging from $[0, 1]$, to indicate a prediction of existing or non-existing edge in the recovering matrix.

### VGAE

The idea of variational graph auto-encoder (VGAE) comes from the variational auto-encoder. The loss is constructed from two components, one is the expected loss on conditional embedding, and the second one is the KL divergence from the conditional probability and the parameterized probability.
$$
\mathcal{L} = \mathbb{E}_{q(Z|X, A)} \left[ \log p(A|Z) \right] - KL \left[ q(Z|X, A) || p(Z) \right]
$$

### Experiments

* measurement: auc score / ap score, because the link prediction is measure by the reconstructed matrix, and it is sparse.
* split data: from the adjacency matrix, split the 1s into training validation and testing as positive samples, and also sample the same size of edge from 0s to represent the negative samples.
* training input: the training adjacency matrix which constructed by training positive edges. 
  Key difference here is in the original work, the input is the original matrix, and it measures the accuracy of the prediction by having a threshold of 0.5. However, to be consistent with validation and testing, the training should only takes the training edges and measured with auc/ap score.


| dataset     | cora | cora | citeseer | citeseer | pubmed | pubmed |
| ----------- | ---- | ---- | -------- | -------- | ------ | ------ |
|             | roc  | ap   | roc      | ap       | roc    | ap     |
| paper       | 91.4 | 92.6 | 90.8     | 92.0     | 96.4   | 96.5   |
| this imple. | 93.5 | 92.2 | 93.0     | 93.2     | 96.5   | 96.2   |

### Reference 

[^1]: Graph Auto-Encoder: [paper](https://arxiv.org/abs/1611.07308), [code](https://github.com/tkipf/gae).