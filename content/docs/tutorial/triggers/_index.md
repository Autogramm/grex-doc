---
weight: 3
title: "Triggers"
---

# Tutorial: Triggers

However, due to possible combinatorial or computational constraints, or simply because a more controlled extraction process is wanted, the linguistic features included in the set of possible predictors are limited to a specific search space.


The feature space is defined in the YAML file (see [Extraction]({{< ref "/docs/tutorial/extraction" >}} "Extraction"))

## Feature space

For each node in the scope, it is possible to decide whether to include or exclude the features of their linear and depedency context.

![search space](search_space.png)

E.g. Given the following scope with two nodes bound (N and A)

```grew
N[upos=NOUN]; A[upos=ADJ]; N-[amod]->A
```

For each of these nodes, we can add or exclude any feature encoded in the parent, previous, next, and the child nodes.

Note:

- It's essential to remove features that are already defined in the scope and conclusion. Otherwise, the classifier will exploit this leaked information to make predictions, leading to an uninformative prediction.
- A node can have several children, and we choose to encode their features as sets. Therefore, the inclusion of a child's feature should be interpreted as existential statement: *there exists at least one child of X with feature A*.
- Features are not included twice. For example, if the parent node of Y is already defined in the scope, it will not be added again.

**In practice, the selection of features within the search space is made through the YAML file.**


## Check the included features

The script `check_features.py` can be used to check the included features, helping to avoid any leaked information or undesirable features.