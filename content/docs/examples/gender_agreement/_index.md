---
weight: 2
title: "Gender Agreement"
---

# Example: Gender Agreement

Some languages have gender agreement. But agreement can occur by chance. Grex can find the agreement patterns that do not occur by chance.

## Scope and conclusion


```yaml
scope: pattern { X->Y; X[Gender]; Y[Gender] }
conclusion: X.Gender = Y.Gender
```

Given all pairs of nodes in a dependency relation that have the gender feature, we look for patterns that trigger agreement — where nodes X and Y share the same gender value.

## Features or triggers

We define the features to use in the YAML file:

```yaml
templates:
    base:
        own:
            method: include
            regexp: ["upos", "rel.*"]
            lemma_top_k: 0
features:
    X: base
    Y: base
```

We only include the upos and the dependency relations. We want to know between which 2 parts of speech there is agreement in French.
Of course, we could include all the features if we wanted to.


## Extraction

This time, we look for patterns of degree 2.

```bash
python3 extract_rules_via_lasso.py \
~/data/treebanks/SUD_French-Sequoia-r2.15 \
--patterns patterns_agreement.txt \
--output rules_agreement.json \
--max-degree 2
```

## Results

WIP 🚧