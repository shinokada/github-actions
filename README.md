# GitHub actions

Collections of GitHub actions.

## More actions

You can find them from [Awesome-Actions](https://github.com/sdras/awesome-actions)


## Cypress 

[Cypress GitHub workflows](https://github.com/cypress-io/github-action/tree/master/.github/workflows)

## Added github/workflow/release-please.yml

- Use `git commit --allow-empty -m "chore: release 0.0.3" -m "Release-As: 0.0.3"` && ggp

This creates a PR and email.

- Merge the PR

This will email you a release note and publish a new release.

**Not able to run GitHub action to publish npm package**

Add NPM_TOKEN with **automation**

GitHub: Not to Environments create it to Secrets Actions

## release-please

This will publish NPM when you merge a PR.

```yml
on:
  push:
    branches:
      - main
      
name: release-please
jobs:
  release-please:
    runs-on: ubuntu-latest
    steps:
      - uses: google-github-actions/release-please-action@v3
        id: release
        with:
          release-type: node
          package-name: release-please-action
      - name: Checkout Repo
        uses: actions/checkout@v3
        if: ${{ steps.release.outputs.release_created }}
      - name: Setupt Node.js 16.x 
        uses: actions/setup-node@v3
        with:
          node-version: 16
        if: ${{ steps.release.outputs.release_created }}
      - name: Install Dependencies 
        run: npm install
        if: ${{ steps.release.outputs.release_created }}
      - name: Creating .npmrc
        run: |
          cat << EOF > "$HOME/.npmrc"
            //registry.npmjs.org/:_authToken=$NPM_TOKEN
          EOF
        env:
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        if: ${{ steps.release.outputs.release_created }}
      - name: Create a package and Publish to npm 
        run: npm run package
        if: ${{ steps.release.outputs.release_created }}
      - name: Change dir
        run: cd package
        if: ${{ steps.release.outputs.release_created }}
      - name: Run npm publish
        run: npm publish
        env:
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        if: ${{ steps.release.outputs.release_created }}
```

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


