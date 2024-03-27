- [Learning Github Actions](#learning-github-actions)
  - [Concepts](#concepts)
  - [Steps followed in order to create a simple WorkFlow](#steps-followed-in-order-to-create-a-simple-workflow)
  - [Comment on new issues](#comment-on-new-issues)
  - [Steps followed as below](#steps-followed-as-below)
    - [Fetching details of the Issue](#fetching-details-of-the-issue)
    - [Adding a Comment to the Issue created](#adding-a-comment-to-the-issue-created)
    - [Commenting with an API](#commenting-with-an-api)
  - [This completes the 1st part of the learning and you will have linked files for next steps](#this-completes-the-1st-part-of-the-learning-and-you-will-have-linked-files-for-next-steps)
    - [Chapter 2 - CI with Actions](#chapter-2---ci-with-actions)

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

- We will be using github community [action](https://github.com/marketplace/actions/create-or-update-comment)
- `Context` in order for the github actions to comment on an issue it needs to know some information like issue id etc. This is called Context
- Before proceeding please check if you actions have the `write` permissions given to it
  - Settings -> Actions -> General -> `Read and write permissions`
  
## Steps followed as below

### Fetching details of the Issue

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

  - Commit the above code and go the Github Browser and create a new issue.
  - You will notice that it triggers the above workflow and you will see all the content of the issue because that is what the context using `github.event` was used to trigger this workflow.
  - Go through the output and you will see a key called `number` which is the issue `number`
  - Now that we understood how the issue can be used as trigger lets add a comment when an issue is created.

### Adding a Comment to the Issue created

- Add the below step in the file `issue_comment.yaml` created in the above step

  ```yaml
    - name: Create comment
      uses: peter-evans/create-or-update-comment@v4
      with:
        issue-number: ${{github.event.issue.number}}
        body: |
                  This is a multi-line test comment
          - With GitHub **Markdown** :sparkles:
          - Created by [create-or-update-comment][1]
      
          [1]: https://github.com/peter-evans/create-or-update-comment
        reactions: '+1'
  ```

  - Notice `${{github.event.issue.number}}` is just the json tree output we saw in previous step.
- Commit changes and push the code.
- Create a new issue and you will see the Github Actions commenting on it. This is so cool . Ain't it ? 😜 Next let us create the same commenting with an API

### Commenting with an API

- Added the job for ubuntu and windows runners to the file `issue_comment.yaml` created in the above step
- Note the way you need to interact with Ubuntu and Windows Runner is different.
- Now create and issue and watch the action.
- [To get a list of API and actions](https://docs.github.com/en/rest?apiVersion=2022-11-28)

## This completes the 1st part of the learning and you will have linked files for next steps

### Chapter 2 - [CI with Actions](CIWithActions.md)
