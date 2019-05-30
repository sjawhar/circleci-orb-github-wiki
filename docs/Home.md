## Purpose

This CircleCI orb automates the building and deployment of documentation to the wiki repo available every GitHub repository.

What? You didn't know that the wiki tab was controlled by a git repo? Well, it's true! And this orb will help you make the most of it.

## Jobs
### build-and-push
Populate sidebar files with page header links, commit to wiki repo, and push.

**Parameters**
* `commit-user-name`: User name with which wiki changes should be commited. Default: `CircleCI`
* `commit-user-email`: User email with which wiki changes should be commited
* `push-wiki-repo`: Push to wiki repo after commit. Default: `true`
* `sidebar-placeholder`: String in sidebar files to replace with page header links. Default: `{{SIDEBAR_POPULATE}}`
* `ssh-key-fingerprint`: Fingerprint of the SSH key with push access to your repo.
* `wiki-folder-path`: Path to directory containing wiki files. Default: `docs`

## Commands
### populate-sidebars
Populate sidebar files with page header links.

**Parameters**
* `sidebar-placeholder`
* `wiki-folder-path`

### commit-wiki-repo
Copy files to wiki repo and commit.

**Parameters**
* `commit-user-name`
* `commit-user-email`
* `wiki-folder-path`

### push-wiki-repo
Push to wiki repo

**Parameters**
* `ssh-key-fingerprint`
* `wiki-folder-path`

## Example
```yaml
version: 2.1

orbs:
  github-wiki: sjawhar/github-wiki@dev:0.2.0

jobs:
  wiki-push:
    docker:
      - image: circleci:node
    steps:
      - github-wiki/push-wiki-repo:
          ssh-key-fingerprint: YOUR_SSH_FINGERPRINT_HERE

workflows:
  deploy-wiki:
    jobs:
      - github-wiki/build-and-push:
          name: wiki-commit
          commit-user-email: circleci+orb-github-wiki@thecybermonk.com
          push-wiki-repo: false
      - wiki-push:
          requires:
            - wiki-commit
          filters:
            branches:
              only: master
```
