---
weight: 4
title: "Extraction"
---

# Tutorial: Extraction

## Download a treebank
First, get a treebank:

- [Universal Dependencies (UD)](https://lindat.mff.cuni.cz/repository/items/55b06337-e49c-4631-9328-b1a38322b1d4)
- [Surface Syntactic UD (SUD)](https://surfacesyntacticud.github.io/data/)

## YAML File Configuration

The scope, the conclusion and the features are defined via a YAML file.

The YAML format requires a strict use of tabs and quotation marks. For more information, see [the Wki page](https://en.wikipedia.org/wiki/YAML).

###  Definition of your Scope and Conclusion

```yaml
scope: pattern { X-[subj]->Y }
conclusion: X << Y; meta.sent_id=re"n01.*"
```

###  Features

Features can be selected within the fixed search space defined in a YAML. 

Here’s a nonsensical example with all possible options and formats:

```yaml
features:
    X:
        own:
            method: include
            regexp: ".*"
            lemma_top_k: 50
        parent:
            method: exclude
            regexp: ["form", "textform", "lemma", "Number"]
            lemma_top_k: 0
        child:
            method: include
            regexp:
                - "upos"
                - "PronType"
            lemma_top_k: 0
        prev:
            method: include
            regexp: ".*"
            lemma_top_k: -1
        next:
            method: exclude
            regexp: ".*"
            lemma_top_k: 0
```

For the `X`, we defined the nodes of the immediate context we are going to use (own, parent, child, prev, next)

Three keywords must be defined for each node: 

- **method**
    - Specifies whether the features should be included or excluded.
    - Values : [include, exclude]
- **regexp**
    - A list of regular expressions matching the features to either include or exclude. The list can be a comma-separated list between brackets or a list where each new item is on a new line.
- **lemma_top_k**
    - The number of the most frequent lemmas to include. If set to -1, all lemmas are included. If the lemmas are excluded in the regexp section, this line will not be considered.


####  Metadata

If you want to add metadata as possible predictors, you need to add the special keys **sentence** and **meta**: 

```yaml
features:
    X:
        own:
            method: include
            regexp: ".*"
            lemma_top_k: 50
    sentence:
        meta:
            method: include
            regexp: ["speaker", "language"]
```

####  Feature Templates

It is possible to define templates (see `base`) for reusing them for different nodes:

```yaml
templates:
    base:
        own:
            method: exclude
            regexp: ["form", "textform", "wordform", "SpaceAfter", "Correct.*"]
            lemma_top_k: 50
        parent:
            method: include
            regexp: ["form", "textform", "wordform", "SpaceAfter", "Correct.*"]
            lemma_top_k: 50
        child:
            method: include
            regexp: ["form", "textform", "wordform", "SpaceAfter", "Correct.*"]
            lemma_top_k: 50
features:
    X: base
    Y: base
```

## Extraction

### Lasso - Sparse Logistic Regression

Run the script from the CLI:

```bash
python3 -m extract_rules_via_lasso.py \\
treebank.conllu \\
output=rules.json \\
patterns=patterns.yaml
# max_degree=2
# min_feature_occurrence=5
# alpha_start=0.1
# alpha_end=0.001
# alpha_num=100
```
- patterns: the YAML file
- max_degree: number of feature attributes by predictor (see [Feature interaction]({{< ref "/docs/tutorial/extraction/#feature-interaction" >}} "Feature Interaction"))
- min_feature_occurrence: features with a number of occurrences below this threshold are ignored.
- alpha values: arguments defining the (see [Regularization path]({{< ref "/docs/tutorial/extraction/#regularization-path" >}} "Regularization Path")) 

## Feature Interaction

Predictors can be built using a single feature or a combination of features.

A predictor ca be:

- A feature node
    - e.g. X.parent.Number=Sing, parent of X has a the feature Number=Sing
    - e.g. X.rel_shallow=nsubj, X is a head of the subject
- A combination of features
    - e.g. X.parent.Number=Sing;X.rel_shallow=nsubj

The degree of the predictors is passed as an argument to Grex.

#### Regularization Path

The weights of the model can be misleading when it comes to ranking the rules. So, we follow a different approach. 

We train the model multiple times (n iterations), each with a different penalization strenght (alpha). The range of alpha is defined by `alpha-start` and `alpha-end`, and it’s divided into `alpha-num` intervals. We assume that rules activated first are more salient and important.

By default, the model will run 100 iterations. If ranking the rules isn’t important, set `alpha-num` to `1`.

### dtree - Decision Tree

```bash
python3 -m extract_rules_via_dtree.py \\
treebank.conllu \\
output=rules.json \\
patterns=patterns.yaml \\
--only-leaves
# min_samples_leaf=5 \\
# node_impurity=0.15 \\
# threshold=1e-1 \\
# max_depth=12
```
The argument `only-leaves` will build grammar rules only using decision paths from leaves (recommended). To learn more about the arguments to pass to the decision tree, see the [scikit-learn dedicated page](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html#sklearn.tree.DecisionTreeClassifier).

## Limitations

- While it's possible to select and remove features from the search space, it's not possible (yet) to remove specific feature values (e.g. remove only punct dependencies)
- A large degree can cause the tool to crash. 