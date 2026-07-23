# Tic Tac Toe App - Jenkins CI/CD

## Overview

This project demonstrates a Jenkins CI/CD pipeline for a Tic Tac Toe application.

The pipeline automates the process of testing and merging code changes using Jenkins.

The CI pipeline consists of two Jenkins jobs:

- **Job 1 - CI Test:** Automatically runs application tests when changes are pushed to GitHub.
- **Job 2 - CI Merge:** Merges tested changes from the `dev` branch into the `main` branch.

---

# CI/CD Pipeline Architecture

Insert diagram here.


Developer
|
| git push (dev branch)
v
GitHub Repository
|
| Webhook Trigger
v
Jenkins Job 1
(CI Test)
|
| Tests Pass
v
Jenkins Job 2
(CI Merge)
|
| Merge dev → main
v
GitHub Main Branch


---

# Job 1 - Continuous Integration (CI) Test

## Purpose

Job 1 is responsible for Continuous Integration testing.

The purpose of this job is to automatically test code changes before they are merged into the main branch.

When a developer pushes code changes to GitHub:

1. GitHub sends a webhook notification to Jenkins.
2. Jenkins downloads the latest code.
3. Jenkins runs automated tests.
4. If the tests pass, Job 2 is triggered.

---

# Jenkins Job Configuration

## Job Name


Suhaib-TTT-Job1-CI-Test


---

# Source Code Management

Repository:


git@github.com:SMK121/tech610-suhaib-ttt-app-cicd-jenkins.git


Branch:


main


Credentials:


suhaib-jenkins-2gh-ttt-app


The SSH credential allows Jenkins to securely authenticate with GitHub.

---

# GitHub Webhook Setup

A GitHub webhook was configured to automatically trigger Jenkins when code is pushed.

Pipeline trigger flow:


Developer Push
|
v
GitHub Webhook
|
v
Jenkins Job 1


Example Jenkins output:


Started by GitHub push by SMK121


This confirms that GitHub successfully triggered Jenkins.

---

# Job 1 Build Steps

Jenkins runs the following commands:

```bash
cd app
npm ci
npm test
```

## npm ci

Installs project dependencies using the package lock file.

## npm test

Runs the automated test suite to verify the application works correctly.

---

# Job 1 Test Result

The automated tests completed successfully.

Example output:

```text
# tests 111
# pass 105
# fail 0
# skipped 6
```

Result:

```text
Finished: SUCCESS
```

This confirms the application passed the CI testing stage.

---

# Triggering Job 2

After Job 1 completes successfully, it automatically triggers Job 2.

Configured using:

**Post-build Actions**

Option:

```
Build other projects
```

Project:

```
Suhaib-TTT-Job2-CI-Merge
```

Condition:

```
Trigger only if build is stable
```

This ensures only tested code moves to the merge stage.

---

# Job 2 - Continuous Integration (CI) Merge

## Purpose

Job 2 is responsible for merging tested changes from the development branch into the main branch.

This creates a controlled workflow where:

- Developers work on the `dev` branch.
- Jenkins tests the changes.
- Successful changes are merged into `main`.

---

# Jenkins Job Configuration

## Job Name

```
Suhaib-TTT-Job2-CI-Merge
```

---

# Source Code Management

Repository:

```
git@github.com:SMK121/tech610-suhaib-ttt-app-cicd-jenkins.git
```

Branch:

```
*/dev
```

Credentials:

```
suhaib-jenkins-2gh-ttt-app
```

The job checks out the development branch because this contains the changes that passed testing.

---

# Job 2 Trigger Configuration

Job 2 is triggered automatically after Job 1 succeeds.

Flow:

```
Job 1
(CI Tests Passed)
        |
        v
Job 2
(CI Merge)
```

Configuration:

```
Build after other projects are built
```

Upstream project:

```
Suhaib-TTT-Job1-CI-Test
```

Condition:

```
Trigger only if build is stable
```

---

# SSH Agent Configuration

SSH Agent was enabled in Jenkins.

Credential used:

```
suhaib-jenkins-2gh-ttt-app
(To Read Write To Repo)
```

This allows Jenkins to:

- Access GitHub securely.
- Fetch branches.
- Push merged changes back to GitHub.

---

# Job 2 Build Script

The following shell script was added:

```bash
echo "Fetching latest branches"
git fetch origin

echo "Switching to main branch"
git checkout main

echo "Merging dev branch into main"
git merge origin/dev

echo "Pushing merged code to GitHub main branch"
git push origin main
```

---

# Script Explanation

## Fetch Latest Branches

```bash
git fetch origin
```

Downloads the latest branch information from GitHub.

---

## Switch to Main Branch

```bash
git checkout main
```

Moves Jenkins to the main branch before performing the merge.

---

## Merge Dev into Main

```bash
git merge origin/dev
```

Merges the remote development branch into main.

`origin/dev` is used because Jenkins has the remote branch available.

---

## Push Changes

```bash
git push origin main
```

Uploads the updated main branch back to GitHub.

---

# Job 2 Troubleshooting

Initially, the merge failed.

Original command:

```bash
git merge dev
```

Error:

```
merge: dev - not something we can merge
```

## Cause

Jenkins only had:

```
origin/dev
```

available.

It did not have a local:

```
dev
```

branch.

---

## Fix

The command was changed to:

```bash
git merge origin/dev
```

After this change, the merge completed successfully.

---

# Successful Job 2 Result

Example Jenkins output:

```
Fetching latest branches

Switching to main branch

Merging dev branch into main

Already up to date.

Pushing merged code to GitHub main branch

Everything up-to-date

Finished: SUCCESS
```

---

# GitHub Result

After successful completion, Jenkins merged the development changes into the main branch.

Example GitHub commit:

```
Merge remote-tracking branch 'origin/dev'
```

The updated README changes appeared on the GitHub main branch.

---

# Completed CI Pipeline Checklist

## Job 1 - CI Test

✅ Jenkins job created  
✅ GitHub repository connected  
✅ SSH credentials configured  
✅ GitHub webhook configured  
✅ Push triggers Jenkins automatically  
✅ Automated tests run successfully  
✅ Job 1 triggers Job 2  

---

## Job 2 - CI Merge

✅ Jenkins job created  
✅ Dev branch connected  
✅ Job triggered after successful Job 1 build  
✅ SSH Agent configured  
✅ Dev branch merged into main  
✅ Jenkins pushed changes to GitHub main  
✅ Merge completed successfully  

---

# Current Pipeline Status

The Continuous Integration pipeline is complete.

Next step:

# Job 3 - Continuous Deployment (CD)

The deployment stage will:

- Copy tested code from Jenkins to AWS EC2.
- SSH into the EC2 instance.
- Restart the application.
- Verify the updated application is running.
