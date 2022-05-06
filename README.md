# GitHub Actions Test

The purpose of this repository is to test a workflow for generating testing links from the changes in a pull request. This is done using just GitHub Actions and GitHub Pages. This should help with the initial boilerplate of setting up this pipeline.

## Generating a Link

Every time a pull request is opened, the workflow defined in `.github/workflows/publish.yml` runs by subscribing to the `pull_request` event. This event fires not just when a pull request is opened, but also when it is updated. The workflow will checkout the changes, install, and then build the changes. This will generate a static directory for deployment (`build` directory). Using a custom action, the workflow will then copy the generated output to the `staging` branch, placing it in a directory named after the PR number. Elsewhere, GitHub Pages is configured to serve the content on the `staging` branch. The PR number naming scheme allows multiple pull request changes to exist in the `staging` branch.

Finally, the workflow leaves a comment on the pull request with the link.

## Merging Changes

When changes get merged, the workflow defined in `.github/workflows/publish-main.yml` executes. This builds the new content of the `main` branch to the root directory of the `staging` branch. Over time, the file structure will resemble:

```
/ latest changes from main
  4/ changes from PR #4
  5/ changes from PR #5
  6/ changes from PR #6
```

This structure is reflected in the GitHub Pages URL.

## Demo

- [Deployed output from main](https://lucasgrinspan.github.io/github-actions-deploy-test/)
- [Deployed output](https://lucasgrinspan.github.io/github-actions-deploy-test/5/) from [PR#5](https://github.com/lucasgrinspan/github-actions-deploy-test/pull/5)
- [Deployed output](https://lucasgrinspan.github.io/github-actions-deploy-test/6/) from [PR#6](https://github.com/lucasgrinspan/github-actions-deploy-test/pull/6)
