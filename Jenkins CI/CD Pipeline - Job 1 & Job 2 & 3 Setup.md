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

# Job 1 - CI Test Setup

## Purpose

The purpose of Job 1 is to automatically test changes before they are merged into the main branch.

If the tests fail, the merge process does not continue.

---

## Jenkins Job Name


Suhaib-TTT-Job1-CI-Test


---

## Source Code Management

Repository:


git@github.com:SMK121/tech610-suhaib-ttt-app-cicd-jenkins.git


Branch:


main


Credentials:


suhaib-jenkins-2gh-ttt-app


The SSH credential allows Jenkins to securely access the GitHub repository.

---

## GitHub Webhook Configuration

A GitHub webhook was configured so that a push event automatically triggers Jenkins.

Flow:


GitHub Push
|
v
Webhook
|
v
Jenkins Job 1


Testing was completed by pushing changes to GitHub and confirming Jenkins started automatically.

Example:


Started by GitHub push by SMK121


---

## Build Steps

Job 1 runs the application tests.

Example:

```bash
cd app
npm ci
npm test

The job installs dependencies and runs the automated test suite.

Job 1 Test Result

Successful test execution:

# tests 111
# pass 105
# fail 0
# skipped 6

Result:

Finished: SUCCESS
Job 2 - CI Merge Setup
Purpose

Job 2 automatically merges tested changes from the development branch into the main branch.

This prevents untested code from being merged into production.

Jenkins Job Name
Suhaib-TTT-Job2-CI-Merge
Source Code Management

Repository:

git@github.com:SMK121/tech610-suhaib-ttt-app-cicd-jenkins.git

Branch:

*/dev

Credentials:

suhaib-jenkins-2gh-ttt-app
Triggering Job 2

Job 1 was configured with a post-build action:

Build other projects

Selected project:

Suhaib-TTT-Job2-CI-Merge

Trigger condition:

Trigger only if build is stable

This means Job 2 only runs when Job 1 tests successfully.

Job 2 Build Environment

Enabled:

Add timestamps to Console Output

This improves Jenkins logs by showing when each step runs.

SSH Authentication

SSH Agent was configured:

Credential:

suhaib-jenkins-2gh-ttt-app
(To Read Write To Repo)

This allows Jenkins to:

Clone the repository
Fetch branches
Push merged changes back to GitHub
Job 2 Build Script

The following shell script was used:

echo "Fetching latest branches"
git fetch origin

echo "Switching to main branch"
git checkout main

echo "Merging dev branch into main"
git merge origin/dev

echo "Pushing merged code to GitHub main branch"
git push origin main
Script Explanation
Fetch latest branches
git fetch origin

Downloads the latest branch information from GitHub.

Switch to main
git checkout main

Moves Jenkins to the main branch.

Merge dev into main
git merge origin/dev

Combines the tested development changes into the main branch.

Push changes
git push origin main

Uploads the updated main branch back to GitHub.

Job 2 Troubleshooting

Initially the merge failed because Jenkins only had:

origin/dev

available, not a local:

dev

branch.

The original command:

git merge dev

caused:

merge: dev - not something we can merge

The fix was changing it to:

git merge origin/dev

This successfully merged the remote development branch.

Final Job 2 Result

Successful output:

Fetching latest branches

Switching to main branch

Merging dev branch into main

git merge origin/dev

git push origin main

Finished: SUCCESS
Final CI Pipeline Result

Completed successfully:

✅ GitHub webhook configured
✅ Git push triggers Jenkins
✅ Job 1 runs automatically
✅ Automated tests pass
✅ Job 1 triggers Job 2
✅ Job 2 merges dev into main
✅ Jenkins pushes merged code to GitHub main branch

The Continuous Integration pipeline is now complete.
