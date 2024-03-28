# Steps to follow

- [Setting up Tool Cache](https://docs.github.com/en/enterprise-server@3.10/admin/github-actions/managing-access-to-actions-from-githubcom/setting-up-the-tool-cache-on-self-hosted-runners-without-internet-access)
- [Link to create a tool cache](https://docs.github.com/en/enterprise-server@3.10/actions/managing-workflow-runs/downloading-workflow-artifacts)
- [Link to download the cache](https://docs.github.com/en/enterprise-server@3.10/actions/managing-workflow-runs/downloading-workflow-artifacts?tool=cli)

## Steps to create a action from local

- Download repo using above step 2
- Copy paste the action repository in your using repository under the below path
  - `<UsingRepo>\.github\workflows\actions\<actionRepo>`
- See below code for example
- 
  ```yaml
    name: Create a comment on new issues

    on:
    issues:
        types: [opened]

    permissions: write-all

    jobs:
    comment-with-action:
        runs-on: windows-latest
        steps:
        - name: Checkout code
            uses: actions/checkout@v2

        - name: Create Comment
            uses: ./.github/workflows/actions/create-or-update-comment
            with:
            issue-number: ${{ github.event.issue.number }}
            body: |
                This is a multi-line test comment
                - With GitHub **Markdown** :sparkles:
                - Created by [create-or-update-comment][1]
                [1]: https://github.com/peter-evans/create-or-update-comment
            reactions: '+1'
  ```