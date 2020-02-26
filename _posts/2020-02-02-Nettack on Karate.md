# Nettack on Karate club network

This is just a interesting demo to show how the Nettack affect the prediction of node labels in small graphs.

Zachary's karate club is a social network of a university karate club, described in the paper "An Information Flow Model for Conflict and Fission in Small Groups" [^1] by Wayne W. Zachary. There are 34 nodes splitting into two groups between the administrator and the instructor.

## Procedure

* run GCN on karate club data
* training using the two node (id: [0, 33], representing the administrator and instructor, respectively)
* standard nettack structure attack with degree as the budget
* monitor the changes in structure
* use the degree matrix as feature matrix
* setting: epochs 20, randomness fixed

## Results

### Changes of probability and predicted labels

* first figure:
  * x-axis is the probability of label 0
  * y-axis is the probability of label 1
  * blue nodes and black nodes represent two groups
  * green dot represents the target
  * red nodes represent the mis-classified nodes
* third figure:
  * dash line represents the deleted edges and purple line represents new edges in the graph.

* results
  * target 0 (poisoning attack)
  ![img](/images/scatter_attack_0.gif)
  ![img](/images/networkx_attack_0.gif)
  ![img](/images/networkx_attack_0_v2.gif)

  * target 33 (poisoning attack)
  ![img](/images/scatter_attack_33.gif)
  ![img](/images/networkx_attack_33.gif)
  ![img](/images/networkx_attack_33_v2.gif)

  * target 9 (evasion attack)
  ![img](/images/scatter_attack_9.gif)  
  ![img](/images/networkx_attack_9.gif)  
  ![img](/images/networkx_attack_9_v2.gif)


### Iteratively add budget $\Delta$

For the target node 33

![img](/images/nettack_budget_karate.gif)

### Conclusion

* Nettack is not successful on node 0 and 33.
* After attack, it worse the prediction accuracy, while didn't flip the predicted label.
* The reason why some are successful and some are not is still unknown, probably it is related to the prediction margin, the degree of the target (features).
* Need further investigation of the attacker.

[^1]: An Information Flow Model for Conflict and Fission in Small Groups [https://www.jstor.org/stable/3629752?seq=1](https://www.jstor.org/stable/3629752?seq=1)