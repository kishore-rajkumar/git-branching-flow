#  Git branching model blueprint

This blueprint can be adapted for your team and is well-suited for software requiring multiple version support, hotfixes, and managed releases.

## Core branches (permanent):

- **master/main:** Always reflects a production-ready state. All commits here are stable releases.
- **develop:** Active development; latest delivered features for the next release. Nightly builds originate here.

## Supporting branches (temporary):

- **Feature branches:**
  - **Purpose:** Develop new features
  - **Originates from:** develop
  - **Merges into:** develop
  - **Naming:** freestyle, except reserved prefixes
  - **Example:** git checkout -b myfeature develop
  - **Merge:** git merge --no-ff myfeature
- **Release branches:**
  - **Purpose:** Prepare for new production releases; bug fixes, version bumps
  - **Originates from:** develop
  - **Merges into:** master and develop
  - **Naming:** release-*, e.g. release-1.2
  - **Actions:** Bump version, minor fixes
  - **Finish:** Merge into master, tag the release, then merge into develop
  - **Example:**
    ```
    git checkout -b release-1.2 develop
    ./bump-version.sh 1.2
    git commit -a -m "Bumped version number to 1.2"
    git checkout master
    git merge --no-ff release-1.2
    git tag -a 1.2
    git checkout develop
    git merge --no-ff release-1.2
    git branch -d release-1.2
    ```
- **Hotfix branches:**
  - **Purpose:** Quick fixes to production releases
  - **Originates from:** master
  - **Merges into:** master, develop (or current release branch)
  - **Naming:** hotfix-*, e.g. hotfix-1.2.1
  - **Example:**
    ```
    git checkout -b hotfix-1.2.1 master
    ./bump-version.sh 1.2.1
    git commit -a -m "Bumped version number to 1.2.1"
    git commit -m "Fixed severe production problem"
    git checkout master
    git merge --no-ff hotfix-1.2.1
    git tag -a 1.2.1
    git checkout develop
    git merge --no-ff hotfix-1.2.1
    git branch -d hotfix-1.2.1
    ```
## Best practices reference:
- Always use **--no-ff** for merges to preserve feature branch history.
- Strictly keep new features in feature branches, and never in release/hotfix branches.
- Tag every release on master.
- Remove temporary branches after merging.
- For teams with simpler workflows or continuous delivery, consider alternatives like GitHub Flow.

## Git branching model diagram
[View](https://github.com/kishore-rajkumar/git-branching-flow/blob/main/Git-branching-model.pdf)

