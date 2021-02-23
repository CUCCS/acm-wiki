# Welcome to CUC ACM-wiki!
[![Build Status](https://travis-ci.com/CUCCS/acm-wiki.svg?branch=master)](https://travis-ci.com/CUCCS/acm-wiki)

## How to contribute

1. Fork or clone this repo.
2. Add markdown files under `/docs` directory at the `master` branch.
3. Change the config in `mkdocs.yml: nav`.
4. Commit or Pull Request.


## Usage
Follow the instructions in `.travis.yml` to set the environment.

You can use this command to deploy in local.

```bash
../acm-wiki $ mkdocs build 
```

Following commands are **NOT Recommended** to run in local.

```bash
../acm-wiki $ mkdocs gh-deploy
```
## URL
https://cuccs.github.io/acm-wiki/