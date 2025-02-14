---
weight: 1
title: "Getting started"
---

# Getting started

## Quick Overview

## Installation

Clone the project repository
```bash
git clone https://github.com/FilippoC/grex2
```

Install python packages
```bash
pip install -r requirements.txt
```

Grex use [Grew](https://grew.fr/), a rewriting graph library, which allows writing very flexible queries over graphs. Grew is implemented in Ocaml.

- Install grewpy: https://grew.fr/usage/python/

### Troubleshooting

If you get an error caused by Cython, try installing the python3-dev package.

```bash
apt install python3-dev
```

A missing dependency during Grew installation ? Install it manually:

```bash
opam install dependency_missing
```

grewpy_backend is well installed but not found ? Try:

```bash
echo ‘eval $(opam env)’ >> ~/.bashrc
```

## Usage

## A flexible tool

Thanks to YAML format and Grew, Grex is a flexible tool. However, that means that it's necessary to learn Grew syntax. You can start [here](https://grew.fr/grew_match/help/) and then follow the Grew(-match) tutorial ([top-left corner](https://universal.grew.fr/))


## Results

