---
weight: 4
title: "Contrastive Analysis"
---

# Example: Contrastive Analysis

It's possible to compare two languages given a target linguistic phenomenon. We call this a constrative grammar. We look for linguistic patterns that will predict one language over another, our sub-treebank over another, given a treebank that combined sentences of two different sources.

## Scope and conclusion

As an example, let's see the different between Romanian finite verbs and French finite verbs.

```yaml
scope: 'pattern { X[upos="VERB", VerbForm="Fin"] }'
conclusion: meta.language=Romanian
```

## Features

```yaml
templates:
    base:
        own:
            method: exclude
            regexp: ["lemma", "textform", "form", "wordform", "SpaceAfter", "xpos", "Typo", "Language", "upos", "VerbForm"]
            lemma_top_k: 0
        parent:
            method: exclude
            regexp: ["lemma", "textform", "form", "wordform", "SpaceAfter", "xpos", "Typo", "Language"]
            lemma_top_k: 0
        child:
            method: exclude
            regexp: ["lemma", "textform", "form", "wordform", "SpaceAfter", "xpos", "Typo", "Language"]
            lemma_top_k: 0
        prev:
            method: exclude
            regexp: ["lemma", "textform", "form", "wordform", "SpaceAfter", "xpos", "Typo", "Language"]
            lemma_top_k: 0
        next:
            method: exclude
            regexp: ["lemma", "textform", "form", "wordform", "SpaceAfter", "xpos", "Typo", "Language"]
            lemma_top_k: 0

features:
    X: base
```

We exclude the features declared in the scope and in the conclusion.


## Extraction

```bash
python extract_rules_via_lasso.py \
    examples/UD_French-GSD-UD_Romanian-RTT-5kfinite_verbs_train.conllu
    --output examples/romanian_french_finite_verbs.json \
    --patterns examples/patterns_ro_fr_finite_verbs.yml \
    --max-degree 2 \
```

## Results 

WIP 🚧
