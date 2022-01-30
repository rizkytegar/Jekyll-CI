# Jekyll CI

Conducted Continuous Integration Tests on Jekyll with Github Action and Travis CI.

## Github Action

Create a folder in your repository like this

```
.github/workflows
```

create a file with the name ```jekyll.yml```, then fill the file like this

```
name: Jekyll site CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Build the site in the jekyll/builder container
      run: |
        docker run \
        -v ${{ github.workspace }}:/srv/jekyll -v ${{ github.workspace }}/_site:/srv/jekyll/_site \
        jekyll/builder:latest /bin/bash -c "chmod -R 777 /srv/jekyll && jekyll build --future"

```

If you want to use cache

```
 - uses: actions/cache@v2
      with:
        path: vendor/bundle
        key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile') }}
        restore-keys: |
          ${{ runner.os }}-gems-
```

Check your branch first

```
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]
```

## Travis CI

Check your gemfile, add the following code if it doesn't already exist

```
source "https://rubygems.org"

gem "jekyll"
gem "html-proofer"
```

Check cibuild

```
script: script/cibuild
```

Edit ```script/cbuild.sh``` according to your needs

Travis Ci files like this

```
language: ruby
cache: bundler
rvm: 2.5

before_install:
  - gem update --system --no-document
  - gem install bundler --no-document

script: script/cibuild

env:
  matrix:
    - JEKYLL_VERSION="~> 3.8.0"
    - JEKYLL_VERSION="~> 3.9"
    - JEKYLL_VERSION="~> 4.0"
```


## Reference