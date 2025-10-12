---
weight: 1
title: "Getting started"
---

# Getting started

## Quick Overview

Grex is a tool that extracts expressive, fine-grained grammar rules from treebanks. Designed for linguists and computational linguists, it is a versatile and robust analytical tool for studying and describing annotated corpora.

There are three ways to interact with Grex:

- CLI: command line tool
- API (WIP 🚧)
- Graphical Interface (WIP 🚧)

Grex is coded in Python and uses the OCaml `grewpy` library.

## Installation

Clone the project repository
```bash
git clone https://github.com/Autogramm/grex
cd grex
```

Create a virtual environment
```bash
python3 -m venv .venv
source .venv/bin/active
```

Install python packages
```pip
pip install -r requirements.txt
```

Grex use [Grew](https://grew.fr/), a rewriting graph library, which allows writing very flexible queries over graphs. Grew is implemented in Ocaml.

- Install grewpy: https://grew.fr/usage/python/

### Troubleshooting

If you get an error caused by Cython, try installing the python3-dev package.

```bash
apt install python3-dev
```

`distutils` missing?
```bash
pip install setuptools
```

A missing dependency during Grew installation? Install it manually:

```bash
opam install dependency_missing
```

`grewpy_backend` is well installed but not in PATH? Try:

```bash
echo ‘eval $(opam env)’ >> ~/.bashrc
```

