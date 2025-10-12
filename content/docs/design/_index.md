---
weight: 2
title: "Design Choices"
---

# Design Choices

Three main choices:

- Grew as the backbone pattern matching system
- The extraction and the search space are defined in a YAML file
- One script by ML model

## Grew pattern matching

Thanks to Grew, it is possible to capture complex patterns. However, this requires an understanding of Grew syntax. You can start [here](https://grew.fr/grew_match/help/) and then follow the Grew(-match) tutorial ([top-left corner](https://universal.grew.fr/)).

## YAML format

The extraction task itself is defined in the [YAML](https://yaml.org/spec/1.2.2/#chapter-2-language-overview) file. In other words, this file can be used to specify the input data, the target variables to predict and the features to fit the models. See the tutorial for more information on the format.

## ML models

Depending on the user's final objective and the nature of the rules they want to extract, there are different ways of extracting grammar rules. For example, sparse logistic regression favours the extraction of overlapping rules with a stronger linguistic motivation. Decision trees, on the other hand, favour disjoint rules, which are typically the first rules learned by language learners.

Users can run their preferred model using different scripts.