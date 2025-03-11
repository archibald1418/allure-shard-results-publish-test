# Allure Results Combiner and Publisher

A GitHub Action that combines Allure results from matrix jobs, generates a unified report, and publishes it to GitHub Pages.

## Features

- ✅ Combines Allure results from multiple artifacts
- ✅ Preserves test history across builds
- ✅ Generates Allure reports
- ✅ Publishes reports to GitHub Pages
- ✅ Supports custom result processing
- ✅ Configurable with sensible defaults

## Usage

Basic usage:

```yaml
- name: Generate and Publish Allure Report
  uses: Valiantsin2021/allure-results-combiner-publisher@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

Advanced usage:

```yaml
- name: Generate and Publish Allure Report
  uses: your-username/allure-results-combiner-publisher@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    results-pattern: 'allure-results-*'
    results-directory: 'combined-allure-results'
    report-directory: 'allure-report'
    history-directory: 'allure-history'
    gh-pages: 'gh-pages'
    gh-pages-branch: 'gh-pages'
    report-title: 'My Test Report'
    retention-days: '30'
    publish-to-pages: 'true'
    process-results: 'node ./path/to/your/reporter.js'
    fail-on-empty-results: 'false'
```

## Inputs

| Input                | Description                                           | Required | Default                |
|----------------------|-------------------------------------------------------|----------|------------------------|
| github-token         | GitHub token for publishing to GitHub Pages           | Yes      | N/A                    |
| results-pattern      | Pattern to match when downloading results artifacts   | No       | 'allure-results-*'     |
| results-directory    | Directory where combined Allure results will be stored| No       | 'combined-allure-results' |
| report-directory     | Directory where the Allure report will be generated   | No       | 'allure-report'        |
| history-directory    | Directory for Allure history                          | No       | 'allure-history'       |
| gh-pages             | Directory for GitHub Pages                            | No       | 'gh-pages'             |
| gh-pages-branch      | Branch for GitHub Pages                               | No       | 'gh-pages'             |
| report-title         | Title for the Allure report                           | No       | 'Allure Test Report'   |
| retention-days       | Number of days to retain the Allure report artifact   | No       | '14'                   |
| publish-to-pages     | Whether to publish the report to GitHub Pages         | No       | 'true'                 |
| process-results      | Custom script to process the Allure report results    | No       | ''                     |
| fail-on-empty-results| Fail if no Allure results are found                   | No       | 'false'                |

## Example Workflow

```yaml
name: Test Workflow with Allure Reporting

on:
  pull_request:
    branches: [main]
  workflow_dispatch:

jobs:
  run-tests:
    name: Run Tests
    runs-on: ubuntu-latest
    strategy:
      matrix:
        shardIndex: [1, 2, 3, 4, 5, 6]
    steps:
      # Your test steps here
      - name: Upload Allure Results
        uses: actions/upload-artifact@v4
        with:
          name: allure-results-${{ matrix.shardIndex }}
          path: path/to/your/allure-results
          if-no-files-found: warn

  generate-report:
    name: Generate Allure Report
    needs: run-tests
    runs-on: ubuntu-latest
    steps:
      - name: Generate and Publish Allure Report
        uses: your-username/allure-results-combiner-publisher@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Prerequisites

- Allure results should be uploaded as artifacts in previous jobs
- GitHub Pages should be enabled for your repository
- Appropriate permissions should be set in your workflow

## License

MIT