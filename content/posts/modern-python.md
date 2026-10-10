---
title: On modern Python development
author: Karl Yngve Lervåg
date: 2026-10-10
---

In this post, I want to share my current project setup for writing modern Python with static typing and good tooling.
The setup is basically:

- `uv` for the project and dependency management.
- `mise` to install tools (notably `uv`), auto-activate the virtual environment, and define common tasks.
- `ruff` to do formatting and linting.
- `pyrefly` to do type checking.
- Proper setup in a CI pipeline.

I've spent a lot of time writing Python code during my career.
I think the first time I wrote Python may have been around 2007, so almost 20 years now.
I always liked it, but with the modern setup, I find I like it even more.
The static type checking is something I particularly like, and I think its value has only increased in the modern era of coding agents.

In the following, I'll explain each part in more detail.
I'm assuming some Python knowledge, such as virtual environments, and that the reader is already used to things like `git`.

## Project and dependency management

There are a lot of different packaging and dependency tools available for Python.
I've tried many of them, e.g. [Poetry](https://python-poetry.org/), [PDM](https://pdm-project.org/en/latest/), and also pure `pip` with manual virtual environments.
These days I prefer [uv](https://docs.astral.sh/uv/), as it is fast and supports all the features I need.
For example:

- `uv sync` to install the dependencies in a virtual environment.
- `uv run ...` to run a tool or script inside the virtual environment.
- `uv add name` to add new dependencies.
- `uv add --dev name` to add new _development_ dependencies.
- `uv remove ...` to remove dependencies.
- `uv build ...` and `uv publish` to build and publish a project.
- `uv version --bump major|minor|patch` to bump the project version.

The setup of a modern Python project is defined in [`pyproject.toml`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/).
It looks something like this:

```toml
[project]
name = "my-project"
version = "1.2.3"
description = "This is a cool project."
readme = "README.md"
requires-python = ">=3.14"
dependencies = [
  # dependencies go here, e.g.
  "pydantic==2.14.0",
]

[dependency-groups]
dev = [
  # development dependencies go here, e.g.:
  "pyrefly==1.3.2",
  "pytest==9.1.1",
  "ruff==0.17.0",
]

[tool.uv]
add-bounds = "exact"
required-version = "==0.12.23"
```

With `add-bounds = "exact"`, `uv add ...` writes an exact version (`==`) to `pyproject.toml`.
I like this for applications: you can see what you actually depend on, and tools like Renovate and Dependabot can bump the versions visibly.
For libraries, you should remove that option and use looser bounds instead.

When you do `uv sync`, `uv` will create a `uv.lock` file.
This file should also be committed to your repository.

## Environment and tasks with mise

[`mise`](https://mise.jdx.dev/) is a tool for setting up well-defined development environments.
I already wrote [a short post about `mise`](/posts/mise).
For Python projects, I mainly use `mise` to install `uv` and to ensure that the virtual environment is activated when I work in a project.

I use the following `mise.toml` file in most of my Python projects these days.

```toml
[tools]
uv = "0.12.23"

[settings]
python.uv_venv_auto = "source"

[tasks.check]
description = "Run formatting, linting and type checking"
usage = """
flag "--no-fix" help="Specify to disable auto-fix for CI"
"""
run = """
if [ "${usage_no_fix}" = "true" ]; then
  uv run ruff format --check
  uv run ruff check
else
  uv run ruff format
  uv run ruff check --fix
fi

uv run pyrefly check
"""
```

The check task makes it easy to run the static checkers both locally and in CI pipelines.
I'll show an example of how to run this in CI at the end of the post.

## Formatting and linting

Python already has a well established style guide, see [PEP 8](https://peps.python.org/pep-0008/) and [PEP 257](https://peps.python.org/pep-0257/).
So, let's not spend time arguing, let's just enforce the styles and use an autoformatter.
There are plenty of formatters, and tools like `black` and `isort` work well.
But I've found [`ruff`](https://docs.astral.sh/ruff/) to be great here.
It's really fast and it is easy to integrate in your editor/IDE.
It also acts as a linter, and so replaces e.g. `flake8` and `pylint`.
It should be mentioned that `pylint` has more rules, but the list of rules supported by `ruff` is large and increasing.

When `ruff` is installed, you can format or check the formatting like this:

```sh
# format all files
uv run ruff format [PATH...]

# check that files are properly formatted (useful in CI)
uv run ruff format --check [PATH...]
```

Similarly, the linter can be run like this:

```sh
# check all files, fix issues where possible
uv run ruff check --fix [PATH...]

# check all files (don't use `--fix` in CI)
uv run ruff check [PATH...]
```

`ruff` can be configured in `pyproject.toml`.
And it can easily be integrated into your favorite editor or IDE, see [Ruff > Editor integrations](https://docs.astral.sh/ruff/editors/).
I typically use configuration like this:

```toml
[tool.ruff]
line-length = 100

[tool.ruff.lint]
# use the default rules, but add a few additional ones
extend-select = [
  "E",  # pycodestyle rules: checks against pep 8 convention
  "TID252",  # Checks for relative imports
  "W",  # pycodestyle rules (warnings)
]
ignore = [
  "E501",   # Line too long - fixed by formatter
]

[tool.ruff.lint.flake8-tidy-imports]
ban-relative-imports = "all"

# These are the default values, but I like to include them just to be explicit.
# You can safely remove the block.
[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"
```

Here I include `TID252` because PEP 8 recommends absolute imports.
This rule will enforce that recommendation.

If you are introducing `ruff` as a formatter and linter to a project, you should likely do it in a couple of steps:

1. Add the formatter first.
   Agree with your colleagues on the desired style.
   Commit the updated `pyproject.toml`.

2. Run `uv run ruff format` and commit the changes.
   Use `.git-blame-ignore-revs` to have `git blame` ignore the formatting commit.

3. For linting, start with the default set of rules.
   Check if the lint rules already pass with `uv run ruff check`.
   If they do, fine, commit the configuration and setup your CI pipeline to run the checks.

4. If the lint step does not pass, then you can either a) fix the problems and commit it, or b) ignore the problems.
   In my experience, there may be several problems, and fixing them is not necessarily worth it immediately.
   Thus, I tend to add to the `ignore` list in `pyproject.toml`, or I use the `# noqa: ...` comment to disable rules in the code.
   You can run `ruff check --add-noqa` to add `# noqa` comments for every existing violation.
   The `--add-noqa` option is the `ruff` counterpart to the `pyrefly` baseline described below.

I recommend reading the [Ruff tutorial](https://docs.astral.sh/ruff/tutorial) if you are new to it.
It'll cost you about 15-30 minutes, and it will go into a lot more detail than I'm doing here.

## Type checking in Python

Over the last 10 years or so, the development community has started to embrace the value of static type systems.
Static types help us avoid a lot of trivial mistakes without needing tests.
Yes, it does add some verbosity, but in my opinion, it also adds _readability_, because it forces us to be more explicit about the intent of our code.
In my experience, readable code with explicit intent is easier to work with, not only for humans, but also for coding agents.

Modern Python is still a dynamically typed interpreted language.
However, since [PEP 484](https://peps.python.org/pep-0484/) and Python 3.5, Python supports the use of type annotations.
These annotations are not enforced at runtime (although libraries like Pydantic read them), but they allow a type checker like the original [`mypy`](https://mypy-lang.org/) to do static type checking.
Things like this become possible:

```python
# untyped
def greeting(name):
    return "Hello " + name

# typed
def greeting(name: str) -> str:
    return "Hello " + name
```

The untyped variant cannot guarantee that `"Hello " + name` is going to be safe at runtime.
The typed variant helps here, in the sense that it allows tools like `mypy` to do static analysis that will complain if our typed code is e.g. used in a way that breaks the type specifications.
So, for this to be useful, you need to consistently use the type annotations correctly.
In a fully typed project, the type checker will catch the large majority of type-related errors before they reach runtime.
For instance, if `greeting` is typed as shown above, then `pyrefly` would show the following type error for an invocation `greeting(42)`:

```
ERROR Argument `Literal[42]` is not assignable to parameter `name` with type `str` in function `greeting` [bad-argument-type]
 --> example.py:4:10
  |
4 | greeting(42)
  |          ^^
 INFO 1 error
```

Python also supports generics, that is, functions and classes with type parameters that are filled in where they're used.
For example:

```python
from typing import TypeVar

T = TypeVar("T")

def first(items: list[T]) -> T:
    ...
```

Here `first` takes a list of some type `T` and is guaranteed to return a `T`.
For instance, pass it a `list[str]` and the type checker knows the result is a `str`.
Since [PEP 695](https://peps.python.org/pep-0695/) and Python 3.12, Python supports type parameter syntax, which allows us to instead write:

```python
def first[T](items: list[T]) -> T:
    ...
```

That is, the syntax for adding type annotations, including generic types, is now quite similar to languages like TypeScript or Scala.
As far as I know, the main reference for Python typing support is here: [Static typing with Python](https://typing.python.org/en/latest/).
I think it is worth skimming it to get a feel of what's possible.

## Type checking with pyrefly

There are plenty of type checkers available, for instance: [mypy](https://mypy-lang.org/), [pyright](https://github.com/microsoft/pyright), [basedpyright](https://docs.basedpyright.com/latest/), [ty](https://docs.astral.sh/ty/), [pyrefly](https://pyrefly.org/), [Zuban](https://zubanls.com/), and more.
I've tried a lot of these, and right now, I find I prefer `pyrefly`.
It is fast, and it is very easy to add both to new projects and to existing projects with a lot of untyped code.
It has very solid support for Python typing and does type inferencing very well.

It is easy to get started.
Add something like this to your `pyproject.toml`:

```toml
[tool.pyrefly]
# set the directories Pyrefly will search for files to type check; e.g.
project-includes = [
  "src",
  "tests",
  "scripts",
]

# enable more checks
[tool.pyrefly.errors]
bad-dataclass-descriptor = true
deprecated = true
division-by-zero = true
empty-body = true
implicit-abstract-class = true
implicit-any-attribute = true
implicit-any-parameter = true
implicit-import = true
invalid-abstract-method = true
invalid-cast = true
missing-attribute-patch-target = true
missing-override-decorator = true
no-any-return-explicit = true
non-exhaustive-match = true
not-required-key-access = true
redundant-cast = true
redundant-condition = true
regex = true
string-as-iterable = true
unannotated-return = true
unimported-directive = true
unknown-attribute-type = true
unnecessary-comparison = true
unnecessary-type-conversion = true
unreachable = true
unsafe-overlap = true
unsupported-dynamic-base = true
untyped-class-decorator = true
untyped-function-decorator = true
unused-ignore = true
unused-type-ignore = true
```

Notice that I enable a lot of additional checks, such as `implicit-any-parameter`, which makes `pyrefly` stricter.
I prefer to make it deliberately strict, but the reader should feel free to drop some of those keys!

If this is a new project, then you're set.
Run the type checker regularly, e.g. with the `mise run check` task defined earlier or with

```sh
uv run pyrefly check
```

I recommend integrating it with your editor/IDE, see [Add Pyrefly to your IDE](https://pyrefly.org/en/docs/IDE/).
And ensure that this also runs in CI (see below).

For existing projects, we need to handle what may be a large number of type errors.
Fixing them all at once is rarely an option; instead, take it step by step.
So, with `pyrefly`, I recommend doing it like this:

1. Start by ignoring every single type issue.
   Run `uv run pyrefly check --baseline pyrefly-baseline.json --update-baseline` to create a baseline file (see the docs for [Baseline Files](https://pyrefly.org/en/docs/error-suppressions/#baseline-files-experimental)).
   It will create the file `pyrefly-baseline.json` which we can now use to suppress all issues.
   Commit this to your repository.

2. Whenever you run `pyrefly check`, ensure you include the option `--baseline pyrefly-baseline.json`.
   Update the `mise.toml` file with this option, and use it in your CI pipeline:

   ```toml
   [tasks.check]
   description = "Run formatting, linting and type checking"
   run = """
   # ...
   uv run pyrefly check --baseline pyrefly-baseline.json
   """
   ```

   You _could_ specify the baseline option in `pyproject.toml`, e.g.:

   ```toml
   [tool.pyrefly]
   baseline = "pyrefly-baseline.json"
   ```

   Then it would be respected without needing it specified as a command line option, and editor integrations would also respect it.
   But I actually _prefer_ that my editor shows all the errors, because it reminds me that I should fix them in future work.

3. With this, you can add `pyrefly` with all the desired configuration without touching and fixing any code.
   It still has value, because it will ensure that you (and your colleagues) don't introduce _more_ issues.

4. Then, as future work, you can run `uv run pyrefly check` and start fixing the issues one by one.
   Every once in a while, you should also update the baseline file with `uv run pyrefly check --baseline pyrefly-baseline.json --update-baseline`.
   Doing that will prevent a fixed error that has been suppressed from being ignored if the fix happens to be reverted.

I used this exact process over about 6-8 months to fully add proper typing to a project.
That is, I fixed a few errors here and there, now and then, alongside my other work.
I started out with over 2,000 issues.
Fixing them was usually simple and consisted of adding the correct type annotations.
And I'm sure I fixed a whole bunch of bugs in the process!

## CI

Adding formatting, linting and type checking as explained above is neat.
But in most projects, we need to enforce this in our CI pipelines as well.
I'm using the following setup in my GitHub workflows (`.github/workflows/ci.yml`); I think it should be relatively straightforward to do the same in other systems like GitLab.
The snippet also includes the test job for completeness.

```yaml
name: CI

on:
  pull_request:

# Cancel in flight runs on a new push to a PR
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  lint:
    name: Run code linter and type checker
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v7.0.1

      # We could let `mise` install `uv`, but with the setup-uv action we get
      # caching, which is very convenient
      - name: Install uv
        uses: astral-sh/setup-uv@v10.0.1
        with:
          enable-cache: true

      - name: Set up mise
        uses: jdx/mise-action@v4.3.0
        # `uv` is already installed above - we only want to run the mise task here
        with:
          install: false

      - name: Install the project
        run: uv sync --locked

      - name: Run checks
        run: mise run check --no-fix

  test:
    name: Run tests
    runs-on: ubuntu-latest
    permissions:
      contents: read
      # `checks: write` lets the JUnit reporter annotate failing tests on the PR.
      checks: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v7.0.1

      - name: Install uv
        uses: astral-sh/setup-uv@v10.0.1
        with:
          enable-cache: true

      - name: Install the project
        run: uv sync --locked

      - name: Run tests
        run: uv run pytest --durations=50 --junitxml=junit.xml

      - name: Publish test summary
        uses: mikepenz/action-junit-report@v6.5.0
        if: ${{ !cancelled() }}
        with:
          report_paths: junit.xml
          annotate_only: true
          detailed_summary: true
          include_time_in_summary: true
```

## Conclusion

I've described my modern Python setup and the main tools I use for it: `uv`, `mise`, `ruff`, and `pyrefly`.
`uv` and `mise` solve the project management, reproducibility, and dependency management.
`ruff` provides formatting and linting, and `pyrefly` adds fast and robust type checking.
Combined, these tools help me write better quality Python projects and provide a good developer experience.
I get the same checks locally and in CI and fast feedback in the editor.

For a project configured as explained here, it becomes very easy to get up and running for other people as well.
If they already have `mise`, then:

```sh
mise trust && mise install
uv sync
mise run check
```

If you want to test this, feel free to clone the example repository:
[modern-python-example](https://github.com/lervag/modern-python-example).
