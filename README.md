# release-please-example


## Added github/workflow/release-please.yml

- Use `git commit --allow-empty -m "chore: release 0.0.3" -m "Release-As: 0.0.3"` && ggp

This creates a PR and email.

- Merge the PR

This will email you a release note.

## add npm-publish.yml

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
        with:
          release-type: node
          package-name: release-please-action
```



