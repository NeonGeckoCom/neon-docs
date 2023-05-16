---
description: >-
  A Skill's `requirements.txt` and `requirements.sh` files provide an
  alternative method to define the dependencies of a Neon Skill.
---

# Requirements files

The older method of defining requirements is still available, and is present in the majority of Skills available in the Mycroft Marketplace. This uses a `requirements.txt` and/or `requirements.sh` file.

## requirements.txt

The `requirements.txt` file can only be used to define Python package dependencies. It uses the [standard Python PIP format](https://pip.readthedocs.io/en/1.1/requirements.html) to list Python packages from [PyPI](https://pypi.org/) to install. Each line in the file represents a separate package to install and must match the title provided by PyPI.

`requirements.txt` should typically be used only by `setup.py`. Simply including it in your skill repository will not guarantee that the packages will be installed.

The following example will install the latest available versions of the [`requests`](https://pypi.org/project/requests/) and [`gensim`](https://pypi.org/project/gensim/) packages.

```text
requests
gensim
```

If specific versions of a package are required, we can use comparison operators to indicate which version.

- `requests==2.22.0` The package must must be version `2.22.0`.
- `requests>=2.22.0` The package must be version `2.22.0` or higher.
- `requests<=2.22.0` The package must be version `2.22.0` or lower.

It is strongly recommended to only use these operators when required. If submitting a Skill to the Marketplace, you will be asked to provide reasoning as to why a specific version of a package is necessary.

### Examples of requirements.txt

- [Weather Skill](https://github.com/NeonGeckoCom/skill-weather/blob/dev/requirements.txt)
- [Wikipedia Skill](https://github.com/NeonGeckoCom/skill-wikipedia/blob/dev/requirements.txt)

## requirements.sh

The `requirements.sh` file is no longer supported in Neon. If you are using a skill that requires this, please reach out to its developer to refactor it.
