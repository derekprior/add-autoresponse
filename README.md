# add-autoresponse

add-autoresponse is a GitHub Action that automatically adds a preconfigured
response to commentable items.

## Configuration

add-autoresponse _requires_ the following inputs:

* `respondableId`: The GraphQL node id of the issue, pull request or other
  commentable content that the autoresponse will be added to
* `response`: The message to be left as the autoresponse
* `author`: The author of the event that triggered the workflow

In addition, the following optional inputs are accepted:

* `exemptedAuthors`: A comma-separated string of logins that are exempt from the
  autoresponse

## Usage

Here's an example workflow that adds a comment to all new issues that aren't
created by exempted authors.

```yaml
name: Issue Autoresponse

on:
  issues:
    types: [opened]

jobs:
  auto-response:
    runs-on: ubuntu-latest

    steps:
    - uses: derekprior/add-autoresponse@master
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        respondableId: ${{ github.event.issue.node_id }}
        response: "Maintenance Mode!"
        author: ${{ github.event.issue.user.login }}
        exemptedAuthors: "johnsmith, janedoe"
```

Another example workflow that adds a comment to the successfully merged pull requests 
that aren't created by exempted authors.

```yaml
  
name: Auto message on pr merge

on:
  pull_request_target:
    types: [closed]

jobs:
  auto-response:
    runs-on: ubuntu-latest

    steps:
    - uses: derekprior/add-autoresponse@master
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        respondableId: ${{ github.event.pull_request.node_id }}
        response: "Well Done !! Thank you @${{ github.event.pull_request.user.login }} for your contribution :)"
        author: ${{ github.event.pull_request.user.login }}
        exemptedAuthors: "johnsmith, janedoe"
```
