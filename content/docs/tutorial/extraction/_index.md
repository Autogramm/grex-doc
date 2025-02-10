---
weight: 4
title: "Extraction"
---

# Tutorial: Extraction

## Download a treebank
First, get a treebank:

- [Universal Dependencies (UD)](https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-5787)
- [Surface Syntactic UD (SUD)](https://surfacesyntacticud.github.io/data/)

## YAML File Configuration

The scope, the conclusion and the features are defined via a YAML file.

The YAML format requires a strict use of tabs and quotation marks.
For more information, see [the wiki page](https://en.wikipedia.org/wiki/YAML).

###  Definition of your Scope and Conclusion(s)

```yaml
scope: pattern { X-[subj]->Y }
conclusion: X << Y
conclusion_meta:
    sent_id: "n01.*"
```

###  Features Definition

Features can be selected within the fixed search space defined here. 

Here’s a nonsensical example with all possible options:

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

For each node of the scope (here X), we defined the nodes of the search we are going to use (own, parent, child, etc.)

Three keywords must be defined by each item of the search space. 

- **method**
    - Specifies whether the features should be included or excluded.
    - Values : include, exclude
- **regexp**
    - A list of regular expressions matching the features to either include or exclude. The list can be a comma-separated list between brackets or a list which each new item is a new line
- **lemma_top_k**
    - The number of the most frequent lemmas to include. If set to -1, all lemmas are included. If the lemmas are excluded in the regexp section, this line will not be considered.

###  Feature Templates

It's possible to define templates to reuse them for selecting the features to include:

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
- patterns: the yaml file
- max_degree: number of feature item by predictor (see [Degree]({{< ref "/docs/tutorial/triggers/#degree-of-predictors" >}} "Degree"))
- min_feature_occurrence: features with a number of occurrences below this threshold are ignored.

#### Regularization Path

We don't trust the weights of the model to rank the rules. So, we apply a different technique. 

We don’t rely on the model’s weights to rank the rules, so we use a different approach.

We train the model multiple times (n iterations), each with a different penalization force (alpha). The range of alpha is defined by `alpha-start` and `alpha-end`, and it’s divided into `alpha-num` intervals. We assume that rules activated first are more distinctive and influential, making them more important.

By default, the model will run 100 iterations. If ranking the rules isn’t important, set `alpha-num` to `1`.

TODO: rename alpha to lambda

### Dtree - Decision Tree

```bash
python3 -m extract_rules_via_dtree.py \\
treebank.conllu \\
output=rules.json \\
patterns=patterns.yaml
# min_samples_leaf=5 \\
# node_impurity=0.15 \\
# threshold=1e-1 \\
# tree_depth=12
```
To learn more about the arguments to pass to the decision tree, see the [scikit-learn dedicated page](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html#sklearn.tree.DecisionTreeClassifier).

## Limitations

- While it's possible to select and remove features from the search space, it's not possible (yet) to remove specific feature values (e.g. remove only punct dependencies)
- A large degree can cause the tool to crash. 