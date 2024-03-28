# CI with Actions

## The Past

- Repository to store the Source Code
- Assuming there are 4 developers working on an Application, each developer would have his own feature branch to work on part of the application.
- After few months when the 4 features are ready, the features would have to be integrated to the main branch by the Integration team.The would checkout each branch , they will run smoke test etc and send the code base to QA (Quality Assurance Team).
- The QA will verify if the Features meet the Spec and Business Requirement.If it fails they will send back to the developers and they will have to do he whole process again.
- Imagine the pain and also if new features come by the way or bugs are found. That is why CI came and took to mainstream so quickly because all this would be done in a matter of hours. Next lets look at CI.

## The Modern DevOps way

- Repository to store the Source Code
- Assuming there are 4 developers working on an Application, each developer would have his own feature branch to work on part of the application.
- Some things CI should solve
  - Automating the builds
  - Introduce Automated Tests
    - Unit Tests
    - Integration Tests
    - End User Testing
  - Linting for Unified coding style (Ofcouse CI should not introduce any changes but only report back of any shortcomings)
  - Security Checks and scans
- A PR is created when the feature is ready to be merged
- The CI will run all these automated tasks and the developers and teams involved will see already if something fails . And Dev can reiterate immediately instead of waiting for Integration team and QA team to get back.
- Once the tests pass before merging to the main we could push the feature and deploy to a staging branch and environment for the QA and other teams to do some testing.Remember CI should never deploy to production . It can deploy to stage. Remember if multiple developers are deploying to stage it can overwrite all the time. This can be handled too 😍✌
- Once the staging env is good we could automate the PR to merge with Main for production and rerun all the automated tests so nothing fails.
- Obviously bugs might still creep in because we cannot cover all things in test but we can have high level of confidence and so quickly and all the important things will be checked and passed.
- Why do we need to run the tests again before integrating to main ?
  - Imaging Developer 2 also has a feature branch and it was created before developer 1 feature branch was merged to main . The Checks will verify if the Dev2's PR is working as expected with the main+dev1 Feature + dev2 feature.
- Awesome right. All this is so fast comparing with the past method of deployment. CI Rocks !!!

## Experienced Developers CI Practice

- [cPython Project](https://github.com/python/cpython/blob/main/.github/workflows/build.yml)
  - Naming the workflow `Tests`
  - Trigger
    - `workflow_dispatch` - Ondemand run from GUI
    - push
      - Branches
        - Different branches version
      - PR
        - Different branches version
  - Jobs
    - check_source
      - name, runs_on
      - outputs
      - checkout the repository (This is important when you are unsing something in the repository, you need to checkout so its available to the runner)
        - Doing a git diff and based of some changes they set the outputs
    - check_generated_files
      - needs: check_source - this specify it is dependent on previous job and it has to be passed
      - uses - setup some branch and actions required
    - build on windows,OS - This allows us to build and test on different environments

### Demo with NodeJS

- [Repository](https://github.com/KiranChilledOut/ci-testing)
- Just clone/ make change and push
- Adding branch protection
  - Setting -> Branched -> Require status checks to pass before merging
    - Only `Job names` will be recognized
  - Try to include code scanning to scan all security issues