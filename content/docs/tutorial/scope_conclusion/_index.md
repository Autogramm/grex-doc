---
weight: 2
title: "Scope and Conclusion"
---

# Tutorial: Scope and Conclusion

Remember that in our formalism, a grammar rule is defined as a conditional of three patterns:

- Scope (S)
- Conclusion (Q)
- Predictor/Trigger (P)

In practice, predictors P are filled by the ML model from a feature space well-defined (see 
[Triggers]({{< ref "/docs/tutorial/triggers" >}} "Triggers")). But the scope and the conclusion are define manually.

## Scope

The scope of the rule, as defined, is the sample space of a linguistic question. For example, if we are interested in the order of attributive adjectives, the scope will include all adjectives in a `amod` relation with their governing noun.

```grew
N[upos=NOUN]; A[upos=ADJ]; N-[amod]->A
```

The scope S can be any Grew pattern.

## Conclusion

The conclusion Q or the linguistic phenomenon of interest is also defined with a pattern. To continue with the example, what we want to know is when the adjectives are placed before their noun. 

```grew
A << N
```

It's possible to use metadata feature as a conclusion. For example, given a treebank containing sentences in two languages, it could be interesting to identify the distinct linguistic features of one language with respect to the other. In this case, the conclusion is the language feature itself, which we want to predict. Grex can do this is this **if the information is encoded for as medadata for each sentence**. 

```
meta.lang=Romanian
```

Simple linguistic patterns and metadata constraints can be combined. For example, we might want to know what favours the fact of being a pre-nominal adjective in Romanian, given sentences from Romanian and other languages. 

```grew
scope: N[upos=NOUN]; A[upos=ADJ]; X-[amod]->Y
conclusion: Y << X; meta.lang=Romanian
```

This is particularly useful for contrastive studies in general, such as comparing the speech of different speakers or texts from different periods.

There are **two important aspects** to bear in mind:

- Firstly, conclusions are defined with respect to the scope and the nodes declared in the conclusion must be present in the scope.

- Conclusions partition the occurrences of the scope in two sets. In practice, they are implemented as a [Grew whether](https://grew.fr/doc/clustering/).