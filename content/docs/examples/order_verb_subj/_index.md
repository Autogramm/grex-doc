---
weight: 1
title: "Subject Inversion"
---

# Example: Subject Inversion

In French, the dominant order is subject-verb. But, in some contexts the subject occupies the post-verbal position. We can find these contexts favoring the inversion with Grex.

## Scope and conclusion


```yaml
scope: pattern { X-[1=subj]->Y }
conclusion: X << Y
```

For all the subjects (scope), we want to predict when the subject if after the verb (conclusion)

## Features or triggers

We define the features to use in the YAML file:

```yaml
templates:
    base:
        own:
            method: exclude
            regexp: ["form", "textform", "wordform", "lemma", "SpaceAfter", "Typo", "Correct.*", "position"]
            lemma_top_k: 0
        parent:
            method: exclude
            regexp: ["form", "textform", "wordform", "lemma", "SpaceAfter", "Typo", "Correct.*"]
            lemma_top_k: 0
        child:
            method: exclude
            regexp: ["form", "textform", "wordform", "lemma", "SpaceAfter", "Typo", "Correct.*"]
            lemma_top_k: 0

features:
    X: base
    Y: 
        own:
            method: exclude
            regexp: ["form", "textform", "wordform", "lemma", "SpaceAfter", "Typo", "Correct.*", "rel.*"]
            lemma_top_k: 0
        child:
            method: exclude
            regexp: ["form", "textform", "wordform", "lemma", "SpaceAfter", "Typo", "Correct.*"]
            lemma_top_k: 0

```

For each declare node, we exclude the following regexps: ["form", "textform", "wordform", "lemma", "SpaceAfter", "Typo", "Correct.*"]

It's important to note that we HAVE to exclude the dependency relation between X and Y, and the position of the parent. If not, these features will be used to predict the position.

## Extraction

An easy way to run the script is through a bash file:

```bash
python3 extract_rules_via_lasso.py \
~/data/treebanks/SUD_French-Sequoia-r2.15 \
--patterns patterns_subject_inversion.txt \
--output rules_subject_inversion.json
```

## Results

WIP 🚧