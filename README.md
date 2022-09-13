# release-please-example


## Added github/workflow/release-please.yml

- Use `git commit --allow-empty -m "chore: release 0.0.3" -m "Release-As: 0.0.3"` && ggp

This creates a PR and email.

- Merge the PR

This will email you a release note.

## add npm-publish.yml

```
# This workflow will run tests using node and then publish a package to GitHub Packages when a release is created
# For more information see: https://help.github.com/actions/language-and-framework-guides/publishing-nodejs-packages

name: Node.js Package

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
      - run: npm ci
      - run: npm run package
        env:
          NODE_AUTH_TOKEN: ${{secrets.npm_token}}
```


