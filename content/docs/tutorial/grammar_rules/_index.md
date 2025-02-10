---
weight: 1
title: "Grammar Rules"
math: katex
---

# Tutorial: Grammar Rules

Before looking into Grex, we need to understand what it can be used for, that is, which are the grammar rules it can extract from treebanks? We first give a very brief overview of what syntactic dependencies are, and then we will formalize what Grex consider as a grammar rule.

## Dependency Treebanks

A treebank is a set of sentences associated with linguistic annotations in form of syntactic trees.

There are two main tree families, **dependency trees** (left) and **constituency trees** (right):

![A dependency and a constituency tree](trees.png)

*Trees from Daniel Jurafsky and James H. Martin. 2025. Speech and Language Processing: An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition with Language Models, 3rd edition. Online manuscript released January 12, 2025. https://web.stanford.edu/~jurafsky/slp3.*


Grex *currently* works with dependency trees and requires them to be encoded in the **conllu data format**.
This format store data in a column structured text file where sentences are separated by (at least one) empty line.
A sentence can contain meta-data which can be use by Grex (e.g. to indicate the origin of the sentence, the language, etc.).

Here is an example of a treebank encoded in a conllu format containing two sentences:

```
# sent_id = GUM_vlog_pizzeria-42
# s_prominence = 4
# s_type = decl
# transition = null
# speaker = Giuliana
# addressee = MrsPellegrino,Nonna
# text = That's great!
1-2	That's	_	_	_	_	_	_	_	_
1	That	that	PRON	DT	Number=Sing|PronType=Dem	3	nsubj	_	Discourse=restatement-repetition_m:55->54:0:_|Entity=(44-event-giv:inact-cf1-1-coref)
2	's	be	AUX	VBZ	Mood=Ind|Number=Sing|Person=3|Tense=Pres|VerbForm=Fin	3	cop	_	_
3	great	great	ADJ	JJ	Degree=Pos	0	root	_	SpaceAfter=No
4	!	!	PUNCT	.	_	3	punct	_	_

# sent_id = GUM_fiction_falling-56
# s_prominence = 3
# s_type = q
# transition = zero
# speaker = Derya
# text = Got it?”
1	Got	get	VERB	VBN	Tense=Past|VerbForm=Part	0	root	_	Cxn=Interrogative-Polar-Direct|CxnElt=1:Interrogative-Polar-Direct.Clause|Discourse=restatement-partial:105->101:2:_
2	it	it	PRON	PRP	Case=Acc|Gender=Neut|Number=Sing|Person=3|PronType=Prs	1	obj	_	SpaceAfter=No
3	?	?	PUNCT	.	_	1	punct	_	SpaceAfter=No

```
Lines starting with # are comments.
However, when places right before a sentence, they can be used to define meta-data.


Grex does not make any assumption of how you store your data: you can have the full treebank in a single file, or split across different files.
Note however that Grex uses [Grew](https://grew.fr/) as a backbone, which can be slow when reading very large files.
We therefore recommend splitting very large treebank in several conllu files.


## Grammar Rule Definition

We understand a grammar rule as conditional relations between corpus occurrences. 
E.g. A linguistic question like *when are adjectives placed before the NOUN in French?* can be made operational to work with corpora as follows.

Given all adjectives of nouns of the corpus (S for scope of the rule or sample space), we look for linguistic patterns or predictors (P) that favors the occurrences of pre-nominal adjectives (Q or conclusion) and we want to know to what extent they do it (α).

We formalize and generalize this rule with a logical formula:


{{< katex >}}
    S\implies (P\overset{\alpha\,\%}{\implies} Q)
{{< /katex >}}



Or using [Grew patterns](https://universal.grew.fr/?custom=67a7be8bead7b):

```
S: X[upos=NOUN]; Y[upos=ADJ]; X-[amod]->Y
P: X[NumType=Ord]
Q: Y << X
```

The rules can be interpreted in a probabilistic way (in a frequentist interpretation): it's more probable to have pre-nominal adjectives when they are ordinals adjectives.


## Limitations

### One rule at the time

Since rule extraction is implemented as a binary classification where the target variable Y can be either Q or ¬Q, it is not possible to interrogate two different, non-disjoint problems simultaneously. Every question must be converted into a binary answerable question. 

E.g. TODO

### Treebanks, yes; interlinear glosses (IGT)? 
Grex does not accept IGT annotations unless they are encoded according to the conllu format. However, IGT annotations usually do not include syntactic relations or dependencies. In that case, only queries concerning linear ordering phenomena between tokens are accepted.
