---
weight: 5
title: "Results analysis"
---

# Tutorial: Results Analysis

For the moment, Grex returns raw results. Rules can overlap, be sub-rules matching subsets of others. The customisable filters are under development.

There rules capture strong correlations between the features in the sample space of the scope and the conclusion. Rules could be:

- pertinent linguistic patterns
- corpus properties
- irrelevant patterns

Results are encoded in a JSON file. The file contains information regarding the scope and the conclusion, the file path given, the intercept values for each model and the predictive patterns.

Grex returns patterns for

- **positive rules**: patterns that favor the conclusion
- **negative rules**: patterns that support the negation (¬) of the conclusion

In the head of the file, you will find some general information regarding the extraction:

- **data_len**: number of occurrences matched by the scope pattern
- **n_yes**: number of occurrences matched by the scope and the conclusion pattern

Each pattern is associated with a series of information and statistics to interpret the model's decision.


- **n_pattern_occurence**: number of matches 
- **n_pattern_positive_occurence**: coincidences that follow the conclusion 
- **decision**: indicator of a positive or negative rule
- **coverage (or recall)**: The proportion of occurrences matching the conclusion that are  captured by the pattern.
- **precision**: the proportion of occurrences matched by the pattern that actually match also the conclusion.
- **delta**: the difference in frequency between occurrences matching the pattern and conclusion, and the expected frequency under the independence hypothesis

Some information from statistical inference:

- **g-statistic**: value of the g-test
- **p-value** given the g-test
- **cramers_phi**: effect size measure

And some information regarding the model:

- **alpha**: value of the penalty parameter when the function is activated
- **value**: weight assigned to the feature
