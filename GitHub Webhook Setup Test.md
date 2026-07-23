# Tic Tac Toe App - Jenkins CI/CD

## Project Overview

This project demonstrates a CI/CD pipeline using Jenkins and GitHub.

The Tic Tac Toe application source code is stored in GitHub and Jenkins is used to automatically trigger builds when changes are pushed.

---

# Git Setup

## Repository Setup

The GitHub repository was cloned locally:

```bash
git clone https://github.com/SMK121/tech610-suhaib-ttt-app-cicd-jenkins.git

Moved into the project directory:

cd tech610-suhaib-ttt-app-cicd-jenkins

Verified the repository:

git status
GitHub Webhook Setup Test
Purpose

A GitHub webhook was tested to automatically trigger Jenkins whenever code changes are pushed to GitHub.

Workflow:

Developer makes changes
        |
        v
git add .
        |
        v
git commit
        |
        v
git push
        |
        v
GitHub webhook sends event
        |
        v
Jenkins pipeline starts
Testing the Webhook

A test change was added to the README file:

line added to ttt app

Changes were committed:

git add .
git commit -m "Test GitHub webhook trigger"

Changes were pushed:

git push

The push successfully updated GitHub:

main -> main

Jenkins received the webhook event and started a new build.

Git Branching

A development branch was created for testing:

Create branch:

git branch dev

Switch to development branch:

git switch dev

Verify current branch:

git branch

Expected output:

* dev
  main
Current Status

Completed:

✅ GitHub repository cloned
✅ README updated
✅ Changes committed
✅ Changes pushed to GitHub
✅ GitHub webhook tested
✅ Jenkins build triggered successfully
✅ Development branch created

Next steps:

Configure Jenkins pipeline stages
Connect Jenkinsfile
Automate build and testing process
Merge dev changes into main branch
