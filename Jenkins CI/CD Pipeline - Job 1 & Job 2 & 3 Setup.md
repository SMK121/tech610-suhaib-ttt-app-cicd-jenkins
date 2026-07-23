# Jenkins CI/CD Pipeline - Job 1 & Job 2 Setup

## Overview

This project uses Jenkins to automate Continuous Integration (CI).

The CI pipeline automatically:
1. Detects code changes pushed to GitHub.
2. Runs automated tests.
3. Merges approved changes from the `dev` branch into the `main` branch.

The pipeline consists of two Jenkins jobs:

- **Job 1: CI Test** - Runs automated tests on the latest code changes.
- **Job 2: CI Merge** - Merges tested changes from `dev` into `main`.
- **Job 3

---

# CI/CD Pipeline Flow

Insert diagram here.


Developer
|
| git push to dev branch
v
GitHub Repository
|
| Webhook Trigger
v
Jenkins Job 1
(CI Test)
|
| Tests pass
v
Jenkins Job 2
(CI Merge)
|
| Merge dev → main
v
GitHub Main Branch


---

# Job 1 - Continuous Integration (CI) Test

## Overview

Job 1 is responsible for Continuous Integration (CI).

The purpose of this job is to automatically test code changes before they are merged into the main branch.

When a developer pushes code changes to GitHub, a webhook triggers Jenkins. Jenkins then downloads the latest code and runs automated tests.

If all tests pass successfully, Job 2 is triggered to merge the changes.

---

## Jenkins Job Name


Suhaib-TTT-Job1-CI-Test


---

## Pipeline Flow


Developer
|
| git push
v
GitHub Repository
|
| Webhook Trigger
v
Jenkins Job 1
(CI Test)
|
| Tests Passed
v
Jenkins Job 2
(CI Merge)


---

# Source Code Management

Repository:


git@github.com:SMK121/tech610-suhaib-ttt-app-cicd-jenkins.git


Branch:


main


Credentials:


suhaib-jenkins-2gh-ttt-app


The SSH credential allows Jenkins to securely authenticate with GitHub and access the repository.

---

# GitHub Webhook Setup

A GitHub webhook was configured to automatically trigger Jenkins when code is pushed.

Workflow:


GitHub Push
|
v
Webhook
|
v
Jenkins Job 1 Starts


Example Jenkins output:


Started by GitHub push by SMK121


This confirms that GitHub successfully communicated with Jenkins.

---

# Build Steps

The Jenkins build runs the application tests.

Commands used:

```bash
cd app
npm ci
npm test

Explanation:

npm ci

Installs the project dependencies using the package lock file.

npm test

Runs the automated test suite to check that the application works correctly.



# Jenkins Job 2 - CI Merge Setup

```markdown
# Job 2 - Continuous Integration (CI) Merge

## Overview

Job 2 is responsible for merging tested changes from the development branch into the main branch.

This ensures that only code which has passed the CI testing stage is merged into the main branch.

---

## Jenkins Job Name


Suhaib-TTT-Job2-CI-Merge


---

# Purpose

Job 2 performs the following actions:

1. Fetches the latest GitHub branches.
2. Switches to the main branch.
3. Merges changes from dev into main.
4. Pushes the updated main branch back to GitHub.

---

# Source Code Management

Repository:


git@github.com:SMK121/tech610-suhaib-ttt-app-cicd-jenkins.git


Branch:


*/dev


Credentials:


suhaib-jenkins-2gh-ttt-app


The job checks out the development branch because this is the branch containing the tested changes.

---

# Trigger Configuration

Job 2 is triggered automatically by Job 1.

Flow:


Job 1
(CI Tests Passed)
|
v
Job 2
(CI Merge)


Configuration:


Build after other projects are built


Project:


Suhaib-TTT-Job1-CI-Test


Condition:


Trigger only if build is stable


---

# Build Environment

Enabled:


SSH Agent


Credential:


suhaib-jenkins-2gh-ttt-app


Purpose:

Allows Jenkins to:

- Authenticate with GitHub
- Fetch branches
- Push merged changes back to GitHub

---

# Build Script

The following shell script is used:

```bash
echo "Fetching latest branches"
git fetch origin

echo "Switching to main branch"
git checkout main

echo "Merging dev branch into main"
git merge origin/dev

echo "Pushing merged code to GitHub main branch"
git push origin main
Script Explanation
Fetch Latest Branches
git fetch origin

Downloads the latest information from GitHub.

Switch to Main Branch
git checkout main

Moves Jenkins to the main branch before merging.

Merge Development Changes
git merge origin/dev

Merges the remote development branch into main.

origin/dev is used because Jenkins only had the remote branch available.

Push Updated Main Branch
git push origin main

Uploads the merged code back to GitHub.

Troubleshooting

Initially the merge failed.

Original command:

git merge dev

Error:

merge: dev - not something we can merge

Reason:

Jenkins had:

origin/dev

but did not have a local:

dev

branch.

The command was changed to:

git merge origin/dev

After this change, the merge completed successfully.

Successful Job 2 Output

Example:

Fetching latest branches

Switching to main branch

Merging dev branch into main

Already up to date.

Pushing merged code to GitHub main branch

Everything up-to-date

Finished: SUCCESS
GitHub Result

After successful completion, the merged code appeared on the GitHub main branch.

Example:

Merge remote-tracking branch 'origin/dev'

This confirms Jenkins successfully merged dev changes into main.

Job 2 Completion Checklist

✅ Job 1 triggers Job 2
✅ Dev branch changes are detected
✅ Jenkins fetches latest branches
✅ Dev branch merges into main
✅ Jenkins pushes updated main branch
✅ CI Merge process completed successfully


