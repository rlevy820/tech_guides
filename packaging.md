# Packaging Code

Notes from [Taylor Brown's](https://tbrown122387.github.io/) Data Analysis with Python (STAT 3250) class at UVA

## Motivation
- Making reusable code
- Having other people be able to use it
- Enable testing and versioning
- Prepare for publishing

## Types of code
- **Script**: one file meant to be run
- **Module**: one file meant to be imported so you can use its functions
- **Package**: folder full of modules with an `__init__.py`
- **Distribution**: installable project

# Overview

## Standard layout
```
my_project/
├── pyproject.toml # has to be in a specific format
├── README.md # markdown document about the project
├── src/ # all the code you write goes in here (src stands for source)
│   └── my_package/ # usually same name as root directory; holds all your modules named by what they do; you might have multiple folders if your package is really big
│       ├── __init__.py
│       ├── io.py
│       ├── clean.py
│       └── stats.py
└── tests/ # code to run/test your code, not related to the core functionality
    ├── test_io.py
    └── test_stats.py
```

## Why `src/`
- As an end user you just do `pip install` and run the libraries `from module import function`
- Prevents accidental imports from the project root so its looking for that `module` in `src/` not the top root directory

## Naming
- Keep package names short, specific, and lowercase

# Making Packages

## `__init__.py` file
- You can have an empty init file, but it can be good for hiding things that the end user can't see
- Intelligently seperate thigns users can and can't see
- The less you have in here the better
```
# src/my_package/__init__.py

# select modules in my_package that you want the user to be able to use
from .clean import normalize_names
from .stats import summarize_numeric

__all__ = ["normalize_names", "summarize_numeric"] # this is the only stuff the user can access, denoted by `__all__`
```

## TOML file
```
# my_package/pyproject.toml

[build-system]
requires = ["setuptools>=68", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-project"
version = "0.1.0"
description = "Reusable utilities for STAT 3250 demos"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [ # change with what libraries the end user will need
  "numpy>=2.0",
  "pandas>=2.2",
]
```
For developers, you can add
```
[project.optional-dependencies]
dev = [ # change with what additional libraries the developer user will need
  "pytest>=8.0",
  "ruff>=0.6",
]
```
And devs install with
```
python -m pip install -e ".[dev]"
```

## Unit tests
- Code can break. Testing all the edge cases helps prevent future breaking
- You can write tests that are meant to return a certain value, or that are meant to throw an error (i.e., testing that something you don't want to happen doesn't happen)
- Name files with "test_" as convention
```
# tests/test_stats.py
import numpy as np
from my_package.stats import summarize_numeric # import the module/function you want to test

def test_summarize_numeric_mean():
    x = np.array([1, 2, 3])
    out = summarize_numeric(x)
    assert out["mean"] == 2
```

## Tips
- Write pure functions without side effects (don't have a function change a variable outside of the function)
- Use triple quotes to write docstrings for public functions
- Seperate input/output from computation (i.e., taking in different data could go in io.py and cleaning it could go in clean.py)

# Workflow when writing the package
When you're writing a package in python, it's helpful, during development to use a virtual environment
- Run `conda create -n projenv python=3.13 -y` (you can change `projenv` to a more specific name for your project)
- This creates the environment for your own development on your own machine (this does not impact others)
- To go into this, run `conda activate projenv`
- Then, run `pip install --upgrade pip` and `pip install -e ".[dev]"`
