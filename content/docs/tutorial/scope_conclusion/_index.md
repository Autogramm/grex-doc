---
weight: 2
title: "Scope and Conclusion"
---

# Tutorial: Scope and Conclusion

Remember that in our formalism, a grammar rule is defined as follows:

- Scope (S)
- Conclusion (Q)
- Trigger (P)

In practice, triggers or patterns P are filled by the ML model from a search sample of features well-defined (see 
[Triggers]({{< ref "/docs/tutorial/triggers" >}} "Triggers")). But the scope and the conclusion have to define manually.

## Scope

The scope of the rule, as defined, is the sample space given a linguistic question. For example, if we are interested in the order of adjectives, the scope will include all adjectives that have a governor noun.


```grew
X[upos=NOUN]; Y[upos=ADJ]; X-[amod]->Y
```

The scope S of the rule is a given pattern, and we consider all occurrences matching this pattern. This pattern can be any Grew pattern.

## Conclusion

The conclusion Q or the linguistic phenomenon of interest is also defined with a pattern. To continue with the example, what we want to know is when the adjectives are placed before the noun. 

```grew
Y << X
```
It's possible to use metadata as a conclusion. For example, given a treebank of sentences in two languages, it could be interesting to find distinct linguistic features of one of those languages with respect to the other language. In that case, the conclusion and what we want to predict is the language itself. Grex can do this is this **if the information is encoded for each sentence**. 

```
lang=Romanian
```

Simple linguistic patterns and metadata conclusions can be combined. For example, given all adjectives having a governor noun, we might want to know what favors the fact of being a pre-nominal adjective in Romanian given sentences from Romanian and other language.


```grew
scope: X[upos=NOUN]; Y[upos=ADJ]; X-[amod]->Y
conclusion: Y << X
conclusion_meta: lang=Romanian
```

We can imagine extracting distinct patterns for specific speakers or from texts from different periods.

There are **two important aspects** to bear in mind:

- conclusions are defined with respect to the scope and the nodes declared in the conclusion must be present in the scope.
- conclusions must be binary. In practice, they are implemented as a [Grew whether](https://grew.fr/doc/clustering/).