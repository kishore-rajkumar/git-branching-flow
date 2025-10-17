# A Simulation - Collaborative Feature Development with Git Flow
_Multi-Developer Branching and Release Management for Expense Tracker MVP_

## Overview

This guide provides an industry-standard Git Flow branching model example tailored for multi-developer collaboration on the expense tracker MVP, focusing on the feature-expense-logging component. 
It details how individual developers work on their own branches derived from a shared feature branch, collaboratively integrate via pull requests, and manage releases with version bumping and hotfixes. 
The workflow emphasizes clean integration, efficient teamwork, consistent versioning, and stable releases suitable for professional software projects.

## Git Flow Workflow for Multi-Developer Feature Branch

**1. Repository Initialization**
- Initialize the repository and create the main branches:
```
git init
git checkout -b main        # Stable, production-ready branch
git checkout -b develop     # Integration branch for new features
```
- Add essential base files such as **README.md**.

**2. Shared Feature Branch**
- Alice creates the shared feature branch from **develop**:
```
git checkout develop && git pull
git checkout -b feature-expense-logging
git push -u origin feature-expense-logging
```
**3. Developer-Specific Branches**
- Both Alice and Bob create personal branches off the shared feature branch to work independently:
  - Alice
    ```
    git checkout -b feature-expense-logging-alice feature-expense-logging
    ```
  - Bob
    ```
    git checkout -b feature-expense-logging-bob feature-expense-logging
    ```

**4. Parallel Development and Commits**
 - Alice and Bob work on separate parts of the feature and commit their changes:
   - Alice commits expense validation logic:
     ```
     git add expense_validator.py expenses.py
     git commit -m "Add expense validation logic"
     git push -u origin feature-expense-logging-alice
     ```

   - Bob commits reporting features:
     ```
     git add report.py expenses.py
     git commit -m "Add expense reporting functionality"
     git push -u origin feature-expense-logging-bob
     ```
**5. Pull Requests and Code Review**
- Each developer opens a pull request (PR) from their personal branch (feature-expense-logging-alice or feature-expense-logging-bob) to the shared feature branch (feature-expense-logging).
- They conduct code reviews, resolve conflicts if any, and merge PRs into the shared branch.

**6. Integrate Feature Branch into Develop**
- After finalizing the shared work, merge the feature branch into develop:
```
git checkout develop
git pull
git merge feature-expense-logging
git push origin develop
```
- Optionally delete the shared feature branch for cleanup:
```
git branch -d feature-expense-logging
```
**7. Release Branch Creation and Version Bump**
- When ready for release, create a release branch from develop:
```
git checkout -b release-1.0.0 develop
```
- Use a version bump tool to update the version consistently (example for patch bump):
```
bump-version patch .
git commit -am "Bump version to 1.0.0"
git push origin release-1.0.0
```
**8. Finalize and Tag Release**
- Merge release branch into main:
```
git checkout main
git pull
git merge release-1.0.0
git tag -a v1.0.0 -m "MVP: Expense Logging release"
git push origin main --tags
```
- Merge release branch back into develop to keep it updated:
```
git checkout develop
git merge release-1.0.0
git push origin develop
```
- Delete the release branch:
```
git branch -d release-1.0.0
```
**9. Hotfix Process (Post-Release)**
- For urgent production bugs, create a hotfix branch from main:
```
git checkout -b hotfix-expense-bug main
# Fix the bug
git commit -am "Hotfix: correct expense amount type"
git push origin hotfix-expense-bug
```
- Merge hotfix into main, tag new patch version, and push:
```
git checkout main
git merge hotfix-expense-bug
git tag -a v1.0.1 -m "Hotfix: correct expense amount data type"
git push origin main --tags
```
- Also merge hotfix into develop:
```
git checkout develop
git merge hotfix-expense-bug
git push origin develop
```
- Delete the hotfix branch:
```
git branch -d hotfix-expense-bug
```
**10. Deployment**
- Deploy production code from the tagged main branch release:
```
git checkout v1.0.1
# Deploy to production environment here
```

## Summary Table

| Branch                         |  Purpose                                      |  Key Actions                                   |
| -------------------------------|-----------------------------------------------|------------------------------------------------|
| main                           |  Stable production code, deployment, tagging  |  Merge releases/hotfixes, tag, deploy          |
| develop                        |  Integration of feature branches              |  Merge shared features, prepare releases       |
| feature-expense-logging        |  Shared collaborative feature development     |  Integration of individual dev branches        |
| feature-expense-logging-\<dev>  |  Developer-specific workspaces                |  Local commits, PRs into shared feature branch |
| release-1.0.0                  |  QA, bugfixes, version bump before release    |  Finalize release, tag, merge to main & develop|
| hotfix-expense-bug             |  Urgent post-release bug fixes                |  Patch, tag, merge to main & develop           |

## Conclusion

This Git Flow-based guide provides a practical, scalable, and collaborative developer workflow ensuring feature isolation, orderly integration, consistent versioning through bumping, structured release processes, and rapid hotfix management. It aligns with industry best practices, simplifying professional-grade development suitable for teams of any size.
