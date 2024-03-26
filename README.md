- [Learning Github Actions](#learning-github-actions)
  - [Concepts](#concepts)
  - [Steps followed in order to create a simple WorkFlow](#steps-followed-in-order-to-create-a-simple-workflow)
  - [Comment on new issues](#comment-on-new-issues)
    - [Steps followed as below](#steps-followed-as-below)

# Learning Github Actions

This is a beginner course on Github actions. I am creating this as a newbee who is also learning. Following this Repository will help you learn GitHub Actions as I learn too.

## Concepts

- `Event` - A triger that will kick of a Github Action.It could be a Pull Request, Push to a branch , Issue created, Closed or any thing else that you can imagine as a trigger
- `WorkFlow` - This fines the task that will be run or an action done based on the `trigger`
  - All workflows exist within a repository (As of the time of recording)
  - Jobs - this is within a workflow and runs parallel by default with other jobs in the workflow
    - Steps - These are steps done inside a Job and all steps inside a job is done sequentially by default.
- `Runners` - This is the host on which the job runns
  - GitHub Runners
    - Ubuntu
    - Windows
    - Mac
  - Self Hosted Runners
    - Built and customized by you

## Steps followed in order to create a simple WorkFlow

- Install the [GH CLI](https://github.com/cli/cli)
- Create a repo `gh repo create KiranChilledOut/LearningGithubActions --public`
- Clone the repo `gh repo clone kiranChilledOut/LearningGitHubActions`
- In order to have workflow we need a `.github` folder in the repository - `New-Item -Type Directory -Name '.github'`
- `cd` into the folder and great a new folder `workflows` - `cd .\.github\` and `New-Item -Type Directory -Name 'workflows'`
- `cd` into the `workflow` folder and create files `hello_world.yaml`  - `cd .\workflows\` and `New-Item -Type File -Name 'hello_world.yaml'`
- Edit the workflow file with `vsCode` and add a 1st line the defines the `workflow name` 
  - `name: Hello World workflow`
- Next specify a trigger

  ```yaml
    on:
      push:
        branches:
          - main
        pull_request:
          - main
        workflow_dispatch:
  ```

  - `on` - Keyword for the trigger
  - `push` - trigger action
    - `branches` - action entity
  - In short it says whenever there is a `push` action on the `main` branch or a `pull request` on the `main` branch or a `manual trigger` on the workflow
  - [All Triggers Documentation](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)
- Next specify a Job 
  
    ```yaml
    jobs:
        hello:
            runs-on: windows-latest
            steps:
                - uses: actions/checkout@v4
                - name: hello world
                  run: write-host "hello World"
                  shell: powershell
        
        goodbye:
            runs-on: windows-latest
            steps:
                - name: goodbye world
                  run: write-host "goodbye World"
                  shell: powershell
    ```

  - `jobs` - Specifies the actions based on the trigger.Here the jobs are `hello` and `goodbye`
  - `runs-on` - Specifies the host/runner the job runs on.
  - `uses: actions/checkout@v4` - this is used to use a community or a self created action. Click to know more about [actions/checkout](https://github.com/actions/checkout)
- Commit and push you code and go to the GitHub Repository in Browser
- You will see in Actions tab of your repository the workflow you created


## Comment on new issues

- We will be using github actions community
- `Context` in order for the github actions to comment on an issue it needs to know some information like issue id etc.This is called Context.
  
### Steps followed as below

- Create a new file `issue_comment.yaml`
- Add the below code
    ```yaml
  name: Create a comment on new issues
  on:
      issues:
          types: [opened]

  jobs:
      comment-with-action:
          runs-on: windows-latest
          steps:
              - name: "dump github context"
                run: |
                  $json = '${{toJson(github.event)}}'
                  Write-Output $json
                shell: pwsh 

    ```