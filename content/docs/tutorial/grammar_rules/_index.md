---
weight: 1
title: "Grammar Rules"
---

# Tutorial: Grammar Rules

Before looking into Grex, we need to understand what it can be used for, that is are the grammar rules it can extract from treebanks? We first give a very brief overview of what syntactic dependencies are, and then we will formalize what Grex consider as a grammar rule.

## Dependency Treebanks

There are two main family:

- **Constituency trees** (or phrase-structure trees):
- **Dependency trees**:

A treebank is a dataset containing annotated


Grex requires the treebank to be in the **conllu data format**.
This format store data in a column structured text file where sentences are separated by (at least one) empty line.
A sentence can contain meta-data which (for example to indicate the origin of the sentence, the language, etc) which can be used by Grex.

Here is an example of a treebank containing two sentences:

```
# ...
x   b   f
wef c   d
```
Lines starting with # are comments.
However, when places right before a sentence, they can be used to define meta-data.


Grex does not make any assumption of how you store you data: you can have the full treebank in a single file, or splitted across different files.
Note however that Grex uses Grew as a backbone, which can be slow when reading very large files.
We therefore recommend to split very large treebank in several conllu files.


## Grammar Rule Definition

We now give a definition of what we call a grammar rule.

TODO.


## Limitations

Todo:

- Give some examples of what Grex can't do (yet)
- Explain that Grex cannot be used with glosses
