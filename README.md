# GC Content of Sequences

This example contains a small Python package and CLI to calculate GC content from sequences in a FASTA file.

The more important part of this repository are the GitHub Actions.

The CI (continuous integration) workflows showcase automated formatting, linting, testing, doctests, and coverage reporting.

The CD (continuous delivery/deployment) workflows showcase package upload to TestPyPI, which works analogously to PyPI, and generating platform-dependent binaries, which are uploaded to GitHub as release artifacts.

[![Licence](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/acg-team/DSaaP-examples-FS26-workflow#license) [![CI](https://github.com/acg-team/DSaaP-examples-FS26-workflow/actions/workflows/pytest_doctests.yml/badge.svg)](https://github.com/acg-team/DSaaP-examples-FS26-workflow/actions) [![codecov](https://codecov.io/gh/acg-team/DSaaP-examples-FS26-workflow/branch/main/graph/badge.svg)](https://codecov.io/gh/acg-team/DSaaP-examples-FS26-workflow) [![Release](https://img.shields.io/github/v/release/acg-team/DSaaP-examples-FS26-workflow)](https://github.com/acg-team/DSaaP-examples-FS26-workflow/releases/tag/v0.1.1)


## GitHub Actions

The workflows in `.github/workflows/` show how the project is validated automatically on pushes and pull requests to `main` and how releases are published on tag creation.

- CI:
  - [`black.yml`](.github/workflows/black.yml): installs the package with dev dependencies and checks that the repository is correctly formatted with Black.
  - [`pylint.yml`](.github/workflows/pylint.yml): installs the package with test and dev dependencies, and runs Pylint across all tracked Python files on Python 3.10, 3.11, 3.12, and 3.13.
  - [`pytest_doctests.yml`](.github/workflows/pytest_doctests.yml): installs the package with test dependencies, runs pytest with doctests enabled, and publishes JUnit test results as workflow artifacts.
  - [`pytest_codecov.yml`](.github/workflows/pytest_codecov.yml): installs the package with test dependencies, runs pytest with coverage, writes `coverage.xml`, and uploads coverage results to Codecov.
- CD:
  - [`deploy_to_testpypi.yml`](.github/workflows/deploy_to_testpypi.yml): triggered on `v*` tags, builds a source distribution and wheel with `build` and publishes them to TestPyPI using trusted publishing.
  - [`release_pyinstaller_binaries.yml`](.github/workflows/release_pyinstaller_binaries.yml): triggered on `v*` tags, uses PyInstaller to build standalone executables for Linux, Windows, and macOS (including amd64 and Intel). Each executable is accompanied by a SHA-256 checksum file. Both executables and checksums are uploaded as workflow artifacts and attached to the corresponding GitHub release.

Together, these workflows enforce code style, static analysis, test execution, doctest validation, coverage reporting, and allow package publishing and binary distribution.

To verify a downloaded binary:

```zsh
sha256sum -c gc-content-linux.sha256
```

## Setup

To run these examples locally, you need Python 3 installed and a clone of the [DSaaP examples (workflow) repository]( https://github.com/acg-team/DSaaP-examples-FS26-workflow ).

This project is configured with `pyproject.toml`, which is the source of truth for package metadata and dependencies.

You can use a virtual environment to install the package and its development dependencies:

```zsh
# Clone the repository if you haven't already
git clone git@github.com:acg-team/DSaaP-examples-FS26-workflow.git
cd DSaaP-examples-FS26-workflow

# Create and activate virtual environment
python3 -m venv gcc_venv
source gcc_venv/bin/activate

# Install the package
pip install .

# Install test tooling as defined in pyproject.toml
pip install .[test]

# Install development tooling (black, pylint)
pip install .[dev]
```

If you only want to use the CLI without the test tools, installing `.` is enough.

Alternatively, you can install the package system-wide:

```zsh
pip install .
```

Or install the latest published version from TestPyPI:

```zsh
pip install -i https://test.pypi.org/simple/ gc-content-DSaaP-FS26
```

## Running the Script

After installation, you can run the command-line entry point on a FASTA file of your choosing:

```zsh
gc-content /path/to/fasta/file
```

You can also run the module file directly:

```zsh
python gc_content/gc_content.py /path/to/fasta/file
```

To run the example FASTA file:

```zsh
gc-content data/DNA_example.fasta
```

The output of the script is the GC content for each sequence in the input file.

## Testing

The project uses `pytest`, with test dependencies defined in `pyproject.toml` under the optional `test` extra.

To run unit tests:

```zsh
pytest test
```

To run unit tests together with doctests:

```zsh
pytest --doctest-modules
```

### Test Coverage

To run unit tests with coverage and generate an HTML report:

```zsh
pytest --cov gc_content --cov-report=xml --cov-report=html
```

To view the coverage report in your browser:

```zsh
open htmlcov/index.html
```

## Data

The `data/` folder contains an example fasta file with DNA sequences.

## Authors

Jūlija Pečerska, Applied Computational Genomics Team.

Developing Software as a Product (DSaaP) course, Spring semester 2026 (FS26).

## Licence

This project is licensed under the MIT Licence – see the LICENSE file for details.