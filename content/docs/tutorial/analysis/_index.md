---
weight: 5
title: "Results analysis"
---

# Tutorial: Results Analysis

Grex returns raw results. Rules can overlap and be sub-rules matching subsets of others. The postprocessing filters are under development (WIP 🚧)

Rules capture strong correlations between the features in the sample space of the scope and the conclusion. Rules could be:

- pertinent linguistic patterns
- corpus properties
- irrelevant patterns

The results are encoded in a JSON file. This file contains information on the scope and conclusion, the given file path, the intercept values for each model, and the predicted patterns.

Grex returns patterns for

- **positive rules**: patterns that favor the conclusion
- **negative rules**: patterns that support the negation (¬) of the conclusion

At the top of the file, you will find some general information about the extraction.

- **s_occs**: number of occurrences matched by the scope.
- **q_occs**: number of occurrences matched by the scope and the conclusion.

Each pattern is associated with a series of information and statistics to interpret the model's decision.

- **p_occs**: number of matches of the selected pattern within the scope.
- **pq_occs**: number of occurrences matching the pattern p and the conclusion within the scope.
- **decision**: indicator of a positive or negative rule
- **coverage**: The proportion of occurrences matching the conclusion that are captured by the pattern P, within the scope.
- **precision**: the proportion of occurrences matched by the pattern that actually match also the conclusion, whithin the scope.
- **delta**: the difference in frequency between occurrences matching the pattern and conclusion, and the expected frequency under the independence hypothesis.

Some information from statistical inference:

- **g-statistic**: value of the g-statistic
- **p-value**: the probability of observing the value of the g-statistic or a more extreme value under the independence hypothesis (Note that if sample is too large, it becomes uninformative.)
- **cramers_phi**: effect size measure

And some information regarding the model and the ranking:

- **alpha**: the value of the penalty parameter when the function is activated. The partial order given by the alpha values can be interpreted as a salient order.
- **coef**: weight assigned to the feature
