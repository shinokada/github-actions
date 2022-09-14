# release-please-example


## Added github/workflow/release-please.yml

- Use `git commit --allow-empty -m "chore: release 0.0.3" -m "Release-As: 0.0.3"` && ggp

This creates a PR and email.

- Merge the PR

This will email you a release note and publish a new release.

**Not able to run GitHub action to publish npm package**

Add NPM_TOKEN with **automation**

GitHub: Not to Environments create it to Secrets Actions


## add npm-publish.yml

```
# This workflow will run tests using node and then publish a package to GitHub Packages when a release is created
# For more information see: https://help.github.com/actions/language-and-framework-guides/publishing-nodejs-packages

name: NPM Package

on:
  release:
    types: [created]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
      - run: npm ci

  publish-npm:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
          registry-url: https://registry.npmjs.org/
      - run: npm install
      - run: npm run package
        env:
          NPM_TOKEN: ${{secrets.NPM_TOKEN}}
```

Add NPM_TOKEN to release-please-example repo


